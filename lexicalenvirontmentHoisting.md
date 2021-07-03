#  Lexical Environment

> 컨텍스트를 구성하는 환경 정보를 담고있다

## 구성 
### 1. Environtment Record
> 활성화 된 컨텍스트에 관련된 코드의 실행 전, 식별자 정보가 순서대로 담겨있다
- 실행할 컨택스트에 식별자에만 관심이 있다. 어떤 값이 할당되어있는지는 모른다.
- 그래서 할당과정은 그자리에 둔다!!

EvirontmentRecord가 식별자를 수집한 이후 일어난 Hoisting
```javascript=
const a = (x) =>    // 매개변수 수집
{
    console.log(x)
    var x
    console.log(x)
    var x =2
    console.log(x)
}
var x = 1
a(x)
```
```javascript=
const a = () => 
{
    var x //변수x를 선언 -> 메모리에 공간 확보 -> 변수x 에 주소값 연결
    var x
    var x

    var x = 1 //숫자 1을 메모리의 데이터공간에 할당, x 주소값고 연결
    console.log(x)
    console.log(x)
    var x = 2 //숫자 2를 데이터 공간에 할당, x 주소값에 대치
    console.log(x)
    // 모든 코드가 실행되어 실행 컨텍스트가 콜스택에서 제거됨
}
a()
```
#### Hoisting
>변수를 수집하는 과정이며, 선언된 변수를 최상단으로 끌어올린다.
    할당 된 경우, 그 자리에 남아있다.
#### 구성 요소
- 매개변수 이름, 함수 선언, 변수명 
``` javascript
const a = (param) =>    // 매개변수 또한 선언된 변수와 같다.
{
    console.log(param)
    param = 1
    let param;
    console.log(param)  // 이미 선언되어 에러발생
}
```
