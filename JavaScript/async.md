## JavaScript Runtime

JavaScript는 싱글 스레드 기반의 언어이다. 싱글 스레드에서 비동기로 어떤 식으로 동작하는 걸까? 

이를 알기 위해서는 JavaScript Runtime(실행 환경)에 대해서 알아보자

![](https://miro.medium.com/max/1400/1*FA9NGxNB6-v1oI2qGEtlRQ.png)

### JavaScript Engine & Web APIs

왼쪽의 큰 박스는 JavaScript Engine을 의미한다. 여기에는 메모리 할당이 이루어지는 메모리 힙, 코드를 실행하면서 (함수가 실행될 때마다) 컨텍스트가 쌓이는 콜 스택이 포함되어 있다. JavaScript에서 싱글 스레드라는 의미는 하나의 콜 스택을 갖고 있다는 뜻이다. 그렇다면 만약 콜 스택에 네트워크 요청과 같이 오래 걸리는 작업이 있다면 어떻게 될까? 응답을 받기 전까지 블로킹 상태가 되어 다른 작업(클릭 이벤트 처리, 렌더링 등)을 수행할 수 없는 상태가 된다. 이를 위해 브라우저(JavaScript Runtime)는 비동기 작업을 수행할 수 있도록 DOM, AJAX, setTImeout과 같은 API를 제공한다. 

### Callback

callback은 비동기로 동작하는 함수에게 특정 작업이 끝났을 때 실행하도록 파라미터로 넘겨주는 함수이다. 

### Event Loop

- Event Loop: 콜 스택과 태스크 큐를 모니터링하면서 콜 스택이 비면 태스크 큐에 쌓인 콜백 함수를 꺼내와 실행한다.
- Task Queue(Callback Queue): 이벤트 발생 후 호출되어야 할 콜백 함수들이 기다리는 공간이다.

### 동작 예시

```jsx
console.log('Hi');
setTimeout(function cb1() { 
    console.log('cb1');
}, 5000);
console.log('Bye');
```

1. 먼저 console.log('Hi')가 콜 스택에 쌓이고 실행된다.

![https://miro.medium.com/max/1400/1*dvrghQCVQIZOfNC27Jrtlw.png](https://miro.medium.com/max/1400/1*dvrghQCVQIZOfNC27Jrtlw.png)

2. 다음으로 setTimeout 함수가 실행되면서 Web APIs에서 timer를 설정한다.

![https://miro.medium.com/max/1400/1*vd3X2O_qRfqaEpW4AfZM4w.png](https://miro.medium.com/max/1400/1*vd3X2O_qRfqaEpW4AfZM4w.png)

3. setTimeout 함수는 바로 실행 종료되고 다음 줄의 console.log('Bye')가 실행된다.

![https://miro.medium.com/max/1400/1*1NAeDnEv6DWFewX_C-L8mg.png](https://miro.medium.com/max/1400/1*1NAeDnEv6DWFewX_C-L8mg.png)

4. 5초가 지난 후 setTimeout의 callback 함수가 콜백 큐에 쌓이게 된다. 이때 이벤트 루프는 콜 스택이 빈 것을 확인하고 콜백 큐의 cb1 함수를 콜 스택으로 옮겨 실행한다. 

![https://miro.medium.com/max/1400/1*eOj6NVwGI2N78onh6CuCbA.png](https://miro.medium.com/max/1400/1*eOj6NVwGI2N78onh6CuCbA.png)

![https://miro.medium.com/max/1400/1*jQMQ9BEKPycs2wFC233aNg.png](https://miro.medium.com/max/1400/1*jQMQ9BEKPycs2wFC233aNg.png)


## 참고
- https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5
