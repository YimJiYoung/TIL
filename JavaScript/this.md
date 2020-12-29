## this

- JavaScript에서는 모든 함수가 `this` 속성을 갖고 있으며, 해당 함수가 동작할 메인 객체를 의미한다. 
- this는 함수를 호출할 때 몇 가지 규칙에 따라서 결정된다. 

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

3. **메서드로서 함수가 호출되면(점 표기법으로 사용되면), this는 해당 함수를 속성으로 가지는 객체를 가르킨다.** 

    ```jsx
    const obj = {
        value: 5,
        printThis: function() {
          console.log(this);
        }
    };

    obj.printThis(); // -> { value: 5, printThis: ƒ }
    ```

    전역 공간에서 함수가 호출된 경우 this는 전역 객체를 가르킨다.(브라우저에서는 window)

    내부적으로 전역으로 선언된 함수는 전역 객체의 속성으로 설정되기 때문에 this가 전역 객체가 되는 것이다.

    ```jsx
    function fn() {
        console.log(this);
    }

    // console.log(fn === window.fn);
    fn(); // -> Window 
    ```

### Arrow Function

화살표 함수는 위의 규칙들이 적용되지 않고, 함수가 생성될 때의 스코프에 따라 this를 설정하게 된다.

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
