## 유튜브 클론코딩

### Node, express, NPM, JSON ?
node는 js가 웹에서 작동하는 것에서 벗어나 밖에서도 사용할 수 있게 해주는 프레임웤</br>
express는 Node.js 웹 애플리케이션 프레임워크</br>
npm은 node package manager로 node 중앙 관리하는곳이라고 이해하면 됨 </br>
JSON은 js가 데이터를 담는 방식 </br></br>

### babel
babel은 구식 JS를 최신 ES6로 바꿔주는 것 </br>
```javascript
const express = require('express')  // node package에서 express 를 import
import express from "express";  // babel로 ES6를 이용한 코드
```

### middleware
미들웨어는 서버의 양파 중앙부 같은 것</br>
마지막 리턴 함수 전에 로그인 같은 것을 처리해 준다</br>
미들웨어는 리턴 전에 얼마든지 마음대로 사용 가능 </br>

```javascript
// next 는 권한을 주는 것
// 권한을 주지 않으면 다음 단계로 넘어가지 않는다
const betweenHome = (req, res, next) => {   // 미들웨어 함수       
    console.log("between");
    next(); // 다음 함수 호출 (handleHome)
}
```
#### logging middleware
```javascript
app.use(morgan("dev")); // logging middleware
```
#### helmet
보안을 위한 middleware

#### body-parser
사용자가 웹사이트로 전잘하는 정보들을 검사하는 미들웨어, request 정보

#### cookie-parser
cookie를 전달받아서 사용할 수 있도록 만들어주는 미들웨어, 사용자 인증에 사용됨

### MVC pattern
django 에서 MTV처럼 </br>
view, controller, model </br>
html loading을 위해 pug 사용 </br>
pug는 express의 view engine</br>


