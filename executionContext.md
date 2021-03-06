# 실행 컨텍스트 <small>Execution Context</small>
코어 자바스크립트 02장 복습

### 실행할 코드에 제공할 환경 정보들을 모아놓은 객체가 바로 실행 컨텍스트!
- 실행할 코드들이 같은 환경에 구성되어 있다면, 이것들을 모아 컨텍스트를 구성하고 
**콜스택 Call Stack**에 올리고 실행한다 

여기서 **중요한 것** !!! 

#### stack 구조에 올라가는 실행 컨텍스트의 구동 방식
1. 우선 전역 컨텍스트를 담고, 
2. 다른 실행환경을 찾게 되면 바로 콜스택에 쌓기 때문에 이전에 실행 중인 컨텍스트는 중단되고, 
3. 다음 코드에서 발견된 실행환경을 콜스택 상단에 쌓게 된다. 
4. 그 컨텍스트를 구성하는 함수가 종료되면 콜스택에서도 제거 되고, 
5. 이전에 중단된 컨텍스트의 코드가 쌓인 순서대로 진행하게 된다.

#### 실행 컨텍스트가 활성화 되면 자바스크립트 엔진은 무엇을 하는가?
- 현재 시점 활성화 된 실행 컨텍스트는 콜스택 가장 최상단에 위치한다
- 실행 컨텍스트와 관련된 코드들에 필요한 환경 구성을 위해 수집하는 정보들이 있다! (객체로 저장)
#### 수집하는 정보
1.  Variable Environment 
  Lexical 환경과 같은 정보를 담고 있으나 최초 코드 실행시의 스냅샷을 유지**하고 
  실행 컨텍스트를 생성할 때는 우선 자신에 정보를 담고나서 **자기 자신과 같은 Lexical 환경을   만든다**
2.  Lexical Environment 
    컨텍스트를 구성하는 환경 정보를 담고 있다. Variable 환경이 우선 실행 컨텍스트 환경 정    보를 구성 하고나면 Lexical 환경정보를 주로 사용하게 된다.
