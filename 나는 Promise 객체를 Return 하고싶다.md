## Promise Return
### Reference
[스택 오버플로우 질문](https://stackoverflow.com/questions/37533929/how-to-return-data-from-promise)

### Promise에서 resolve되어, then API로 가져온 데이터 값을 return 해서 사용하고 싶다
결론 - 불가능하다<br/>
One of the fundamental principles behind a promise is that it's handled asynchronously.<br/>
This means that you cannot create a promise and then immediately use its result synchronously in your code<br/>
(e.g. it's not possible to return the result of a promise from within the function that initiated the promise).<br/>
