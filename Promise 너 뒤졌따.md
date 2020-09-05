## Promise

### Reference
[promise 1](https://medium.com/@kiwanjung/%EB%B2%88%EC%97%AD-async-await-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EC%A0%84%EC%97%90-promise%EB%A5%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-955dbac2c4a4)<br/>
[바보들을 위한 Promise 강의](https://programmingsummaries.tistory.com/325)

#### Overview
JS는 작업들이 대부분 비동기로 이루어지며, 어떤 작업을 요청하면서 콜백 함수를 등록하면 작업이 수행되고 나서 결과를 나중에 콜백 함수를 통해 알려주는게 매우 흔하게 사용됨<br/>
![image](https://user-images.githubusercontent.com/48672212/92307742-7ba88100-efd3-11ea-9b58-e4c954ba6b2b.png)
<br/>
하지만 코드가 복잡도가 높아지는 상황에서 콜백이 중첩되는 경우 콜백 지옥이 생김<br/>
<br/>
이런 상황을 극복하기 위해 오래전부터 Promise라는 패턴이 제안되어 왔음

#### Promise
```javascript
//Promise 선언
var _promise = function (param) {

	return new Promise(function (resolve, reject) {

		// 비동기를 표현하기 위해 setTimeout 함수를 사용 
		window.setTimeout(function () {

			// 파라메터가 참이면, 
			if (param) {

				// 해결됨 
				resolve("해결 완료");
			}

			// 파라메터가 거짓이면, 
			else {

				// 실패 
				reject(Error("실패!!"));
			}
		}, 3000);
	});
};

//Promise 실행
_promise(true)
.then(function (text) {
	// 성공시
	console.log(text);
}, function (error) {
	// 실패시 
	console.error(error);
});
```
몇 줄 안되지만, Promise의 기초에 대해 명확하게 이해할 수 있는 코드임 <br/>
그리고 위의 코드처럼 Promise 선언과 실행부로 나뉜다

#### 1) Promise 선언부
Promise 선언은 말 그래도 "약속"이다. 지금은 없으니까 이따가 줄게~라는 약속임. <br/>
지금은 없는데 이따가 실행부에서 준다는 소리이다<br/>
따라서 Promise는 선언되면 4가지 중 하나의 상태(State)가 됨
![image](https://user-images.githubusercontent.com/48672212/92307993-42711080-efd5-11ea-9971-aae94a5bec24.png)
<br/>
```javascript
var _promise = function (param) {

	return new Promise(function (resolve, reject) {

		// 비동기를 표현하기 위해 setTimeout 함수를 사용 
		window.setTimeout(function () {

			// 파라메터가 참이면, 
			if (param) {

				// 해결됨 
				resolve("해결 완료");
			}

			// 파라메터가 거짓이면, 
			else {

				// 실패 
				reject(Error("실패!!"));
			}
		}, 3000);
	});
};
```
Promise 선언부를 보면, 나중에 Promise 객체를 생성하기 위해 Promise 객체를 리턴하도록 함수로 감싸고 있다. <br/>
Promise 객체는 resolve와 reject라는 함수를 파라미터로 받고있다 <br/><br/>

New Promise로 Promise가 생성되는 직후부터 resolve나 reject가 호출되기 전까지의 순간을 pending 상태라고 볼 수 있다. <br/>
이후 비동기 작업이 마친 뒤 결과물을 약속대로 잘 줄 수 있다면 첫번째 파라메터인 resolve 함수를 호출하고, 실패했다면 두번째 파라메터인 reject 함수를 호출한다<br/>

#### 2) Promise 실행부
```javascript
_promise(true)
.then(function (text) {
	// 성공시
	console.log(text);
}, function (error) {
	// 실패시 
	console.error(error);
});
```
선언한 Promise를 return하는 함수인 _ promise()를 실행하면 promise 객체가 리턴된다 </br>

#### 3) then API
Promise 객체에는 정상적으로 비동기작업이 완료되었을때 호출하는 then 이라는 API가 존재한다. </br>
`then(function(성공){}, function(실패){})` 처럼 첫번째 파라메터는 성공시 호출할 함수를, 두번째 파라메터에는 실패시 호출할 함수가 수행된다.</br>

#### 4) catch API
만약 비동기 작업이 중간에 에러가 나면 에러를 catch해주는 API가 있다
`.then(null, function(){})`
<br/>
```javascript
_promise(true)
	.then(JSON.parse)
	.catch(function () { 
		window.alert('체이닝 중간에 에러가!!');
	})
	.then(function (text) {
		console.log(text);
	});
```
<br/>

앞서 _ Promise에서 만든 객체는 성공 혹은 실패시 JSON 객체가 아닌 String을 리턴하므로 <br/>
JSON.parse에서 Error가 나게된다. 따라서 다음 then으로 이동하지 못하고 catch에서 받게 됨<br/>
catch는 이와같이 promise가 연결되어 잇을 때 발생하는 오류를 처리해 줌

#### 5) Example
```javascripts
asyncThing1()
	.then(function() { return asyncThing2();})
	.then(function() { return asyncThing3();})
	.catch(function(err) { return asyncRecovery1();})

	.then(function() { return asyncThing4();}, function(err) { return asyncRecovery2(); })
	.catch(function(err) { console.log("Don't worry about it");})

	.then(function() { console.log("All done!");});
```
위의 로직을 표현하면 이렇다
![image](https://user-images.githubusercontent.com/48672212/92308828-8e26b880-efdb-11ea-9ea6-3d031cdea58f.png)
<br/>

#### 6) 여러 promise가 완료할 때 실행하기
`Promise.all`
```javascrips
var promise1 = new Promise(function (resolve, reject) {

	// 비동기를 표현하기 위해 setTimeout 함수를 사용 
	window.setTimeout(function () {

		// 해결됨 
		console.log("첫번째 Promise 완료");
		resolve("11111");

	}, Math.random() * 20000 + 1000);
});

var promise2 = new Promise(function (resolve, reject) {

	// 비동기를 표현하기 위해 setTimeout 함수를 사용 
	window.setTimeout(function () {

		// 해결됨 
		console.log("두번째 Promise 완료");
		resolve("222222");

	}, Math.random() * 10000 + 1000);
});


Promise.all([promise1, promise2]).then(function (values) {
	console.log("모두 완료됨", values);
});
```

#### 7) Return 하지않고 바로 New Promise 생성
바로 promise를 생성할 경우, 즉각 실행되므로 promise.then(alert)와 같이 바로 then 사용한다
```javascript
var _promise = new Promise(function(resolve, reject) {
	
	// 여기에서는 무엇인가 수행 

	// 50프로 확률로 resolve 
	if (+new Date()%2 === 0) {
		resolve("Stuff worked!");  
	}
	else {
		reject(Error("It broke"));
	}
});
```
