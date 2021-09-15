# DOM 기반 XSS 예방법

주로 XSS(Cross-Site Scripting)에는 3가지 알려진 방법이 있다
1. Reflected
2. Stored
3. DOM Based XSS

- Refleted와 Stored XSS가 DOM Based XSS와 다른 점은 공격이 앱에 주입되는 위치이다.

- Refleted와 Stored XSS는 서버 측 주입 문제이며 DOM Based XSS는 클라이언트 측 주입 문제이다.(클라이언트에서 런타임 중에 앱에 직접 주입된다.)

## DOM Based XSS


- 위험한 HTML 메소드의 예시
#### 속성
``` javscript
 element.innerHTML = "<HTML> Tags and markup";
 element.outerHTML = "<HTML> Tags and markup";
 ```
#### 메서드
``` javscript
 document.write("<HTML> Tags and markup");
 document.writeln("<HTML> Tags and markup")
 ```
 
 - 안전하게 동적으로 업데이트 하는 방법

신뢰하지 않는 입력값을 전부 인코딩 한다.
``` javscript

 const ESAPI = require('node-esapi');
 
 element.innerHTML = "<%=ESAPI.encoder().encodeForJS(ESAPI.encoder().encodeForHTML(untrustedData))%>";
 element.outerHTML = "<%=ESAPI.encoder().encodeForJS(ESAPI.encoder().encodeForHTML(untrustedData))%>";

 const ESAPI = require('node-esapi');
 document.write("<%=ESAPI.encoder().encodeForJS(ESAPI.encoder().encodeForHTML(untrustedData))%>");
 document.writeln("<%=ESAPI.encoder().encodeForJS(ESAPI.encoder().encodeForHTML(untr
 ```
 
- 실행 컨텍스트 내 HTML속성에 신뢰할 수 없는 데이터를 삽입하지 못하게 한다- javscript 이스케이프
``` javscript
 
   ESAPI = require('node-esapi');
   
 const x = document.createElement("input");
 x.setAttribute("name", "company_name");
 x.setAttribute("value", '<%=ESAPI.encoder().encodeForJS(companyName)%>');
 const form1 = document.forms[0];
 form1.appendChild(x
 ```
 코드를 실행하지 않는  HTML 속성을 설정할 때,
 인젝션 우려가 없이 HTML 요소 속성에 객체속성을 바로 설정 가능하다.
 공격자가 작은따음표, 인라인 코드 닫기나 HTML 로 이스케이프하고 새 스크립트 태그를 여는 것을 허용하지 않는 방법
