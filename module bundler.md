## Module bundler
Node.js와 React를 사용할 때 묻지도 따지지도 말고 Webpack과 babel를 설치해야 했었는데, 그 이유를 알아보자
## Result
- 웹팩은 기본적으로 모듈 번들러다.
- 엔트리로 그래프의 시작점을 설정하면 웹팩은 모든 자원을 모듈로 로딩한 후 아웃풋으로 묶어준다.
- 로더로 각 모듈별로 바벨, 사스변환 등의 처리하고 이 결과를 플러그인이 받아 난독화, 텍스트 추출 등의 추가 작업을 한다.
#### Reference
[1 module bundler](https://2dubbing.tistory.com/57)<br/>
[2 webpack](https://jeonghwan-kim.github.io/js/2017/05/15/webpack.html)


### 1 Module bundelr
javascript가 최초 나왔을 당시, 웹 페이지의 규모는 지금과 같이 크지 않았다.<br/><br/>
그러나 컴퓨터의 성능이 향상되고, 네트워크 속도가 빨라지며 웹 페이지 규모도 점차 커지게 되었다.<br/>
웹 페이지 규모가 커지다보니 javascript 코드 규모 또한 커지게 되었고, 중복되는 기능도 많았다.<br/>
그러다보니 중복되는 기능들을 하나의 모듈로 만들어 소스 코드의 규모를 줄이고, 재사용성을 좋게 하였다.<br/><br/>
하지만, 이 당시 javascript 모듈 사용을 위한 방법에는 정해진 표준이 없었다.<br/>
표준 없이 뒤죽박죽 개발을 하면 개발자들간에 혼선이 생기게 되고, 언어 사용성이 떨어지게 된다.<br/>
이를 막기위해 CommonJS 와 AMD (Asynchronous Module Definition) 방식의 모듈 사용이 등장하게 된다. <br/>

![image](https://user-images.githubusercontent.com/48672212/98960955-5f334280-2548-11eb-95da-2fd3125a8df6.png)

네트워크 속도가 빠르다고 해서 웹 페이지가 완벽하게 로드되는 시간까지 빠른 것은 아니다.<br/>
<br/>
웹 페이지를 구성하는 html, js, css 및 이미지 파일들이 많을 수록 그 갯수만큼 웹 서버에 요청을 하고 응답을 받게 된다.<br/>
여기서 동일한 타입의 파일끼리 묶어서 구성하면 웹 서버에 요청하는 수가 줄지 않을까??<br/>
<br/>
이 역할을 번들러를 통해 할 수 있다.<br/>
<br/>
예를 들어, 어떠한 웹 페이지를 로드하기 위해 html 파일 1개, js 파일 5개, css 파일 5개가 있다고 가정해보자.<br/>
한개의 파일을 요청/응답 받는데 걸리는 시간이 1초라면, 총 11초가 걸리게 된다.<br/>
<br/>
하지만, 동일한 타입의 파일로 묶게되면 총 3초가 걸리게 된다.<br/><br/>
javascript 번들러의 종류로는 여러가지가 있는데, 그 중에 대표적인 번들러로는<br/>
webpack, parcel, browserify 가 있다.

### 2 Webpack
웹팩의 주요 4가지 개념 엔트리, 아웃풋, 로더, 플러그인
#### 2-1)엔트리
웹팩에서 모든 것은 모듈이다. 자바스크립트, 스타일시트, 이미지 등 모든 것을 자바스크립트 모듈로 로딩해서 사용하도록 한다.
<br/>
웹팩은 엔트리를 통해서 필요한 모듈을 로딩한고 하나의 파일로 묶는다.
```javascript
// webpack.config.js

module.exports = {
  entry: {
    main: "./src/main.js",
  },
}
```
우리가 사용할 html에서 사용할 자바스크립트의 시작점은 src/main.js 코드다. entry 키에 시작점 경로를 지정했다.<br/>
#### 2-2) 아웃풋
엔트리에 설정한 자바스크립트 파일을 시작으로 의존되어 있는 모든 모듈을 하나로 묶을 것이다. 번들된 결과물을 처리할 위치는 output에 기록한다.
```javascript
// webpack.config.js

module.exports = {
  output: { // dist 폴더의 bundle.js 파일로 결과를 저장
    filename: "bundle.js",
    path: "./dist",
  },
}
```
html파일에서는 번들링된 이 파일을 로딩하게끔 한다.
```javascript
// index.html

<body>
  <script src="./dist/bundle.js"></script>
</body>
```
여기까지 간단히 웹팩으로 번들링하는 과정이다. 매우 간단하다.그럼 로더와 플러그인은 무슨 역할을 하는걸까?<br/>

#### 2-3) 로더
웹팩은 모든 파일을 모듈로 관리한다고 했다. <br/>
자바스크립트 파일 뿐만 아니라 이미지, 폰트, 스타일시트도 전부 모듈로 관리한다. <br/>
그러나 웹팩은 자바스크립트 밖에 모른다. <br/><br/>
비 자바스크립트 파일을 웹팩이 이해하게끔 변경해야하는데 로더가 그런 역할을 한다.<br/><br/>
<br/><br/>
가장 간간한 예가 바벨이다. ES6에서 ES5로 변환할 때 바벨을 사용할수 있는데 test에 ES6로 작성한 자바스크립트 파일을 지정하고 use에 이를 변환할 바벨 로더를 설정한다.
<br/>마침 위 코드를 ES6로 작성했으니 로더를 이용해 ES5으로 변환해 보겠다.<br/>
```javascript
// webpack.config.js:

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,            //  ES6로 작성한 자바스크립트 파일을 지정
        exclude: "node_modules",  // node_moudles 폴더는 패키지 폴더이므로 제외
        use: {                    // 이를 변환할 바벨 로더를 설정
          loader: "babel-loader",
          options: {
            presets: ["env"],
          },
        },
      },
    ],
  },
}
```
이건 css-loader
```javascript
// webpack.config.js

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
}
```

#### 2-4) 플러그인
. 로더가 파일단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다. 
