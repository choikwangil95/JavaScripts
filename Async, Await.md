## Async, Await

### Reference
[Promise와 Async/Await](https://medium.com/@kiwanjung/%EB%B2%88%EC%97%AD-async-await-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EC%A0%84%EC%97%90-promise%EB%A5%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-955dbac2c4a4)

### Promise
promise는 오브젝트 안에 오브젝트를 포함하는 javascript 오브젝트의 특별한 형태<br/>
그러면 어떻게 promise에 접근할 수 있을까요? `.then()`을 쓴다<br/>

```javascript
function getFirstUser() {
    return getUsers().then(function(users) {
        return users[0].name;
    });
}
```

<br/>
그럼 promise 체인에서 error를 어떻게 처리할 수 있을까요? `.catch()`를 사용
<br/>

```javascript
function getFirstUser() {
    return getUsers().then(function(users) {
        return users[0].name;
    }).catch(function(err) {
        return {
          name: 'default user'
        };
    });
}
```

<br/>
promise는 일관된 비동기를 강제합니다. 이렇게 말하는거죠. ‘이건 비동기 함수가 될거야. 리턴값이 지금 사용 가능하든지, 아니든지 말이야.’
<br/>

### Async, Await
자, 위에 있는 코드를 봅시다. `getUsers()` 는 promise를 리턴한다 <br/>
ES2016의 promise 이기만 하면 우리는 기다릴(await) 수 있어요. 진짜 말 그대로 promise에서 `.then()`을 쓰는거랑 똑같은거에요. (콜백 함수를 요구하지 않는다는점은 다르지만요)<br/>

```javascript
async function getFirstUser() {
    let users = await getUsers();
    return users[0].name;
}
```

우리는 무엇이든 기다릴(await) 수 있습니다. 그게 결정되었든(resolved) 안되었든, 그게 생성되었든 아니든 말입니다.<br/>
await은 내 메소드의 실행을 일시중지시킵니다. promise의 값이 사용가능할 때까지요.
<br/>
음.. 그러면 error 처리는 어떻게 할까요?

```javascript
async function getFirstUser() {
    try {
        let users = await getUsers();
        return users[0].name;
    } catch (err) {
        return {
            name: 'default user'
        };
    }
}
```

자, 이제 promise로 구현하는 법과 async/await로 구현하는 법이 있다는 걸 알았습니다. 그럼 왜 promise를 알아야 하는걸까요?<br/>

```javascript
async function getFirstUser() {
    let users = await callbackToPromise(getUsers);
    return users[0].name;
}
```

“promise를 이해하지 못하면 async/await를 사용하면서 진짜 진짜 이해하기 어려운 케이스와 버그를 만나게 된다”
