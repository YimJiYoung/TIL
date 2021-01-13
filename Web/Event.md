## 이벤트


### 이벤트 버블링

한 요소에 이벤트가 발생했을 때 해당 이벤트가 상위 요소로 전달되는 방식. 이벤트가 발생한 요소의 이벤트 핸들러가 먼저 실행되고 이어서 상위 요소의 이벤트 핸들러를 차례대로 실행된다.

### 이벤트 캡처링

한 요소에 이벤트가 발생했을 때 최상위 요소부터 이벤트가 발생한 요소로 이벤트가 전달되는 방식. 최상위 요소의 이벤트 핸들러부터 이벤트가 발생한 요소까지 이벤트 핸들러가 차례대로 실행된다. `addEventListener`의 세번째 인자로 true를 넘겨주면 캡처링 단계의 이벤트 핸들러를 등록할 수 있다. (default 버블링)

![https://gccontent.blob.core.windows.net/gccontent/blogs/legacy/wijmo/2015/06/event-diagram.png](https://gccontent.blob.core.windows.net/gccontent/blogs/legacy/wijmo/2015/06/event-diagram.png)

```jsx

// HTML
<body>
  <div id="one">
    one
    <div id="two">two</div>
  </div>
</body>

// JS
const one = document.querySelector('#one');
const two = document.querySelector('#two');
const body = document.querySelector('body');

body.addEventListener('click', () => {
  console.log('body');
}, true) // capturing

one.addEventListener('click', () => {
  console.log('one');
}, true) // capturing

one.addEventListener('click', () => {
  console.log('one bubble');
})

two.addEventListener('click', () => {
  console.log('two');
})

// two 클릭 시 
// body -> one -> two -> one bubble
```

### event.target / event.currentTarget

event.target: 실제 이벤트가 발생한 요소

event.currentTarget(=this): 이벤트 핸들러가 등록된 요소

### 이벤트 위임

하위 요소 각각에 이벤트를 등록하지 않고 상위 요소에서 하위 요소의 이벤트를 공통적으로 제어하는 방식이다.

다음과 같은 구조에서 li 태그 각각에 이벤트 핸들러를 등록하려고 하면 브라우저가 많은 이벤트를 핸들러를 기억해야 하고(메모리 낭비) 새로운 li 요소가 추가됐을 때 새로 이벤트 이벤트를 등록해야하는 비효율이 발생한다. 이때 ul 태그에만 공통적인 이벤트 핸들러를 딱 등록해주면 이벤트 버블링을 이용하여 li 태그의 이벤트가 상위로 전달되고 하위 요소의 이벤트를 제어할 수 있게 되는 것이다.

```jsx
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ul>
```

이런 이벤트 위임을 사용하면 브라우저가 너무 많은 이벤트 핸들러를 기억하지 않게 하고, 또한 DOM이 추가/삭제 될때 Event등록을 매번 해줘야 하는 수고를 없앨 수 있다.
