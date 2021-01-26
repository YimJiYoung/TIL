## this

- JavaScript에서는 (화살표 함수 제외)모든 함수가 `this` 속성을 갖고 있으며, 해당 함수가 동작할 메인 객체를 의미한다. 
- this는 기본적으로 실행 컨텍스트가 생성될 때 함께 결정된다. 실행 컨텍스트는 함수를 호출할 때 생성되므로, this는 함수를 호출할 때 결정된다.

1. **`new` 키워드를 통해 호출된 함수(생성자 함수)의 `this` 는 새로 생성된 인스턴스 객체이다.**

    ```jsx
    function Constructor(name) {
        this.name = name;
    }

    const obj1 = new Constructor('first');
    const obj2 = new Constructor('second');

    console.log(obj1, obj2);
    // Constructor { name: 'first' } Constructor { name: 'second' }
    ```

2. **apply, call, bind가 함수 호출 시 사용되면, `this` 는 apply, call, bind의 parameter로 넘겨진 객체를 가르킨다.**

    ```jsx
    function fn() {
        console.log(this);
    }

    const obj = {
        value: 5
    };

    const boundFn = fn.bind(obj);

    boundFn(); // -> { value: 5 }
    fn.call(obj); // -> { value: 5 }
    fn.apply(obj); // -> { value: 5 }
    ```

3. **메서드로서 함수가 호출되면(점 표기법으로 사용되면), this는 자신을 호출한 객체를 가르킨다.** 

    ```jsx
    const obj = {
        value: 5,
        printThis: function() {
          console.log(this);
        }
    };

    obj.printThis(); // -> { value: 5, printThis: ƒ }
    ```

    일반적인 함수 호출의 경우, this는 전역 객체를 가르킨다.(브라우저에서의 전역객체는 window)

    내부적으로 함수가 전역 객체의 속성으로 설정되어 this가 전역 객체가 되는 것이다.

    ```jsx
    function fn() {
        console.log(this);
    }

    // console.log(fn === window.fn);
    fn(); // -> Window 
    ```

### Arrow Function

화살표 함수는 위의 규칙들이 적용되지 않고, 함수가 생성될 때의 스코프에 따라 this를 설정하게 된다.
(화살표 함수는 this 바인딩을 갖지 않고 상위 스코프의 this 활용)

```jsx
const obj = {
    value: 'abc',
    createArrowFn: function() {
        return () => console.log(this);
    }
};

const arrowFn = obj.createArrowFn();
arrowFn(); // -> { value: 'abc', createArrowFn: ƒ }
```

- `obj.createArrowFn()` 실행 시 `createArrowFn` 내부의 this는 규칙 3에 따라 obj가 되고, 따라서 리턴되는 화살표 함수의 this는 obj로 설정(바인딩)된다.
- 화살표 함수가 전역에서 생성되었다면 this는 전역 객체가 된다.

### Call, Apply, Bind

`this` 를 조작할 수 있도록 하는 함수. parameter로 넘긴 객체가 this로 설정된다.

- Call은 argument를 one by one, Apply는 배열로 넘긴다.
- Bind는 Call, Apply와 달리 함수를 바로 실행하지 않고, this가 설정된(바인딩된) 함수를 리턴한다.

```jsx
function logThisAndArguments(arg1, arg2) {
    console.log(this);
    console.log(arg1);
    console.log(arg2);
}

const obj = { val: 'Hello!' };
const fnBound = logThisAndArguments.bind(obj, 'First arg', 'Second arg');

logThisAndArguments('First arg', 'Second arg');
// this -> Window {frames: Window, postMessage: ƒ, …}

logThisAndArguments.call(obj, 'First arg', 'Second arg');
logThisAndArguments.apply(obj, ['First arg', 'Second arg']);
fnBound();
// this -> { val: 'Hello!' }
```
- 유사배열 객체에 배열 메서드를 적용하고 싶을 때 활용할 수 있다. (sperad operator나 Array.from()으로 대체 가능)
```javascript
const nodeList = document.querySelectorAll('div');
const nodeArr =  Array.prototype.slice.call(nodeList);
```
### DOM event handler & Callback 함수에서의 this

함수가 DOM의 이벤트 핸들러로 사용될 때 `this`는 어떤 값으로 설정될까?

→ this는 이벤트가 적용되는 엘리멘트로 설정된다 

참고 : [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

```jsx
// 화살표 함수는 this를 생성될 때 시점에 결정 -> window
button.addEventListener('click', () => {
  console.log(this === window);  // true
});

// 일반 함수의 경우 event handler의 this는 이벤트를 등록하는 element로 설정된다
button.addEventListener('click', function() {
  console.log(this === button); // true
});
```
콜백 함수의 제어권을 가지는 함수가 콜백 함수의 this를 어떻게 설정하느냐에 따라 달라진다. 설정되지 않은 경우는 default로 전역 객체로 설종된다.]\


### Why 'this'?
JS에서는 왜 `this`가 동적으로(함수가 호출될 때) 결정되는 걸까? 
- 다양한 객체에 대해 동작하는 재사용가능한 유연한 함수로 만들 수 있기 때문이다 ->  함수가 선언되는 시점에 결정되는 스코프를 이용하는 클로저 함수와 달리 외부에서 동적으로 적용될 객체를 설정할 수 있다.
- prototype chain을 통해 연결된 객체의 메서드를 호출할 때도 원래 의도한 객체에 대해 동작할 수 있도록 만든다.
