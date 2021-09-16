# Validation 101

## Input Validation
- 신뢰하지 않는 불확실한 데이터를 인풋 폼에 사용하게 될 때, 서버 사이드와 클라이언트 사이드에서 의도하지 않은 코드 실행으로 이어질 수 있다.

### 방법
- 인풋 validation은 Syntactic-Semantic validation을 적용해야 한다
1. Syntactic validation
- 해당 필드에 맞는 syntax를 강제로 적용하도록 한다. ( 통화심볼, 날짜, GPS)
3. Semantic 
- 해당 필드 컨텍스트에 맞는 올바른 값을 적용하도록 강제한다. (시작날짜 뒤에 마지막 날짜, 금액에 한도 범위가 설정 됨)

Injection, XSS ... 공격을 막기 위해선, 사용자가 요청을 보내기 전에 가능한 일찍 요청 값들을 확인한다. Input Validation을 사용하는 이유가, 승인되지 않은 요청 값에대해 앱에서 서버로 요청을 하기 전에 사전에 검열이 가능하기 때문이다.

### 구현예시
1. 사용자 입력 데이터 타입 확인 
2. JSON/XML Schema가 인풋에 입력 값으로 들어온 경우 적절하지 않기 때문에 검열하여 요청을 막는다.
3. 엄격한 예외 핸들링을 이용한 타입 변환 (parseInt())
4. 최대 최소 값의 범위를 달력에서 사용하는 경우, 파라메터는 숫자 타입으로, 범위선택 한도 길이는 스트링 타입으로 입력 값으로 들어온 데이터를 검렬.


### 허용 리스트 vs 금지 리스트
1. 취약점 공격으로 이어질 수 있는 문자나 패턴 (예: "'", <script>태그)을 발견하기 위해 validation을 적용하는 것은 좋지 못한 방법이다
-> 정규표현식을 이용하여 validation 패턴을 생성해 적용해야 한다.


## Free-form Unicode text Validation
- 자유형식 폼의 유니코드 텍스트 값을 검열하기
- 사용자가 입력하고자 하는 값에 취약점 공격으로 이어질 수 있는 문자나 패턴이 있는 경우를 고려하여, 컨텍스트에 따라 해당 텍스트 출력 값이 인코딩 되어야 한다. - 데이터의 생명 주기를 고려해야 한다.

### 방법
1. Normalization : 전체 텍스트에 적용되는 형식으로 인코딩 되어야 한다. 유효하지 않는 문자들은 없어야 한다.
2. Character category allow-listing : 유니코드는 카테고리 리스팅을 지원한다. 예를 들면 라틴 알파벳 뿐만 아니라 글로벌 영역에 사용된 다양한 스크립트들도 적용된다.
3. Individual character allow-listing: 특정 글자들과 특수문자를 글로벌 하게 기능하는 카테고리에서 제외하고 단일적으로 사용하고 싶을 때. (예: 리스트에서 name에 해당하는 값에만 "'" 를 사용)

## Regex 정규표현식
- 정규식 패턴을 만들 때는 [RegEx Denial of Service (ReDoS)](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS) 를 고려하여 디자인 한다.
해당 공격은 잘못 디자인된 정규식을 CPU 리소스를 오랜 시간동안 사용하며 느리게 실행하게 한다.

- 모든 인풋 데이터에 적용
- 입력 값으로 허용 가능한 문자를 적용
- 데이터의 최소/ 최대 길이를 적용

## 클라이언트 단 vs 서버 단
- 클라이언트 단에서 실행되는 자바스크립트 인풋 유효성 검사는 공격자가 자바스크립트 동작을 막거나 웹 프록시를 사용하는 경우 무효화 될수 있다

## XSS 방어와 컨텐츠 보안
- 통제 된 사용자 데이터는 HTML페이지로 반영이 될 때, 인코딩이 되어야 한다.
```
<script> -> &lt;script&gt;
```
- 사용자 데이터가 자바스크립트 스크립트 내부에서 사용되는 경우 해당 데이터에 대한 출력물을 특정하여 인코딩하여야 한다.

<작성중>



레퍼런스
[Input_Validation_Cheat_Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
