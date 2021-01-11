## Promise

비동기 코드를 작성할 때 다음과 같은 콜백 지옥이 펼쳐질 수 있는데 이를 해결하기 위한 것이 Promise 객체이다.

```jsx
// callback hell 😱
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log('Got the final result: ' + finalResult);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);

// Promise를 사용하면 depth를 줄일 수 있다.
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```

Promise는 다음 중 하나의 상태를 가질 수 있다.

- pending: fulfilled도 rejected도 아닌 초기 상태
- fulfilled(resolved): 비동기 작업이 성공적으로 완료된 상태
- rejected: 비동기 작업이 어떤 이유 or 에러로 실패한 상태

![https://mdn.mozillademos.org/files/8633/promises.png](https://mdn.mozillademos.org/files/8633/promises.png)

### Promise.prototype.then()

then(onFulfilled, onRejected) 메서드는 Promise 객체를 반환하며 Promise가 resolve됐을 때 호출되는 onFulfilled, reject됐을 때 호출되는 onRejected 콜백 함수를 받는다.

- callback 함수에서 값을 반환할 경우, 해당 반환값을 resolve하는 Promise를 반환한다. (반환값 없으면 undefined)
- 오류가 발생할 경우, 해당 오류를 reject하는 Promise를 반환한다.

### Promise.prototype.catch()

catch(onRejected) 메서드는 Promise 객체를 반환하며 rejected된 경우만 다룬다. (내부적으로 `then(undefined, onRejected)`와 같이 동작)

```jsx
Promise.resolve()
  .then(() => {
    // .then()에서 거부한 프로미스를 반환함
    throw new Error('으악!');
  })
  .catch(error => {
    console.error('onRejected 함수가 실행됨: ' + error.message);
  })
  .then(() => { 
    console.log("처음 then의 프로미스가 거부했지만 그래도 이 코드는 실행됨");
  });
```

### Promise.prototype.finally()

finally(onFinally) 메서드는 마찬가지로 Promise 객체를 반환하며, Promise가 처리되면 resolved인지 rejected인지 관계없이 콜백 함수가 실행됩니다

### Promise.all()

 Promise.all(iterable)에서 iterable 내의 모든 Promise가 resolve하면 resolve하고, 어떤 하나가 reject하면 즉시 reject하는 프로미스를 반환합니다.
 
 ### executor 함수 실행 시점
- Promise를 생성할 때 넘겨주는 executor 함수는 언제 실행될까 ? -> 즉시 실행된다 ! 따라서 executor 함수 내에 동기적인 코드가 있으면 여전히 동기적으로 동작함. setTimeout이나 다른 비동기 함수를 써서 비동기적으로 동작하게 하는 것이 올바른 사용 방법이다.
```
function doSomethingAsynchronous() {
  return new Promise((resolve) => {
    console.log("a");
    resolve("promise result");
  });
}

doSomethingAsynchronous();
console.log("b");

// 출력
// a
// b
```
참고: https://stackoverflow.com/questions/42118900/when-is-the-body-of-a-promise-executed
