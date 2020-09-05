## 1 Axios ?
axios는 HTTP 클라이언트 라이브러리로써, 비동기 방식으로 HTTP 데이터 요청을 실행함<br/>
내부적으로 AXIOS는 직접적으로 XMLHttpRequest 를 다루지 않고 “AJAX 호출”을 할 수 있음<br/>
Axios는 Promise를 기반으로하여 async/await문법을 사용하여 XHR요청을 매우 쉽게 할 수 있음<br/>

#### Reference
[JS 비동기 좋아요 기능](https://tothefullest08.github.io/javascript/2019/07/03/JS05_JS_in_Django_like_function/)<br/>
[Axios 1](https://m.blog.naver.com/sssang97/221714680863)<br/>
[Axios 2](https://minwooks.tistory.com/6)<br/>
[JS DOM docs 1](https://developer.mozilla.org/ko/docs/Web/API/ChildNode/remove)<br/>
[JS DOM docs 2](https://www.w3schools.com/jsref/met_node_appendchild.asp)<br/>

### Ajax
Ajax는 Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자로<br/>
XMLHttpRequest 객체를 이용해서 데이터를 로드하는 기법 이며 <br/>
JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술 <br/>
`XMRHttpRequest 하려다가 포기했는데 axios라는 라이브러리가 있었다 ㅎㅎ`<br/>

### XML Http Request
xml은 html과 매우 비슷한 문자 기반의 마크업 언어<br/>
xmlhttpRequest는 웹 브러우저와 서버 사이에 메소드가 존재해서 데이터를 전송하는 객체 폼의 API이다! <br/>

### JSON
JSON(JavaScript Object Notation)은 "Key-Value 쌍"으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷<br/>
비동기 브라우저/서버 통(AJAX)을 위해, 넓게는 XML(AJAX가 사용)을 대체하는 주요 데이터 포맷이다.<br/>

### axios 행동 방식
```javascripts
  axiox.get('url')
    .then(function(response){
    }.catch(e){
    }
```
axios 함수는 get을 통해 비동기 처리를 위한 Promise 객체를 반환한다. <br/>
이 객체는 then과 catch method를 사용해서 완료시점에서 처리될 핸들러를 설정한다.<br/>
데이터를 성공적으로 받아오면, then에 전달된 함수를 호출하며 결과들을 인자로 전달한다(response)<br/>
그리고 전달된 response 객체의 data 멤버에 실제 데이터들이 있음 (response.data)

### Axios 장점
-> 요청을 중단시킬 수 있음 <br/>
-> 응답 시간 초과를 설정하는 방법이 있음 <br/>
-> JSON 데이터 자동변환 <br/>
