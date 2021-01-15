# Hoisting
## Variable hoisting

변수 선언이 scope의 위쪽으로 옮겨지는 것을 의미한다. 

변수 및 함수 선언이 물리적으로 작성한 코드의 상단으로 옮겨지는 것은 아니고, 컴파일 단계에서 메모리에 저장된다.

```jsx
console.log(x); // undefined
var x = 5;
console.log(x); // 5
```

 `var` 키워드로 선언하면 변수 호이스팅이 일어난다.

첫번째 줄의 변수  `x`는 선언되기 전이므로 `Reference Error`가 떠야 하지만 변수 호이스팅으로 선언이 먼저 되었기 때문에 (선언은 되었지만 할당되지 않은) `undefined` 가 된다.

## Function hoisting

**함수 표현식** ( Function expression) → 변수 호이스팅이 일어남 (선언부만 호이스팅됨)

```jsx
var fn = function() {
}
```

**함수 선언식** ( Function declaration) → 전체(선언과 초기화) 다 호이스팅됨 

```jsx
function fn() {
}
```

**Examples**

```jsx
fnDeclaration(); // -> This works!
fnExpression(); // undefined -> Uncaught TypeError: fnExpression is not a function

function fnDeclaration() {
    console.log('This works!');
}

var fnExpression = function() {
    console.log("This won't work :(");
}
```

## let, const

`var` : function-scope, 호이스팅 발생

`let`:  block-scope, 호이스팅 발생 X, 재선언 불가

`const` : block-scope, 호이스팅 발생 X,  재할당 불가

**Examples**

```jsx
function fn() {
  if(true) {
    var x = 10; // function-scoped
    let y = 20; // variable-scoped
  }
  console.log(x); // -> 10
  console.log(y); // -> ReferenceError
}

fn();
```

→  의도되지 않은 호이스팅이나 스코프 범위, 재선언/할당으로 인한 side effect를 막기 위해  `var` 사용을 지양하자 !

 <br>
 <br>
 
# Reference vs Value

- `Primitive type` :  Number, String, Boolean, undefined, null, Symbol ( **Pass by Value**)
- `Object type` : Array, Function, Object (**Pass by Refernece**)

primitive type을 담고 있는 변수의 경우 `=` 연산자를 통해 다른 변수에 할당했을 때 value 자체가 복사된다. 

그러나 Object type의 경우는 Object value 자체가 아니라 Object value에 대한 reference가 복사된다.

```jsx
var reference = [1];
var refCopy = reference;
```

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/81aa31f9-eec7-4d1f-bc14-1bbca157402c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20201025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201025T041337Z&X-Amz-Expires=86400&X-Amz-Signature=4c3416348fa56498c957b9b3a0418d7f8da1701fafc1edef1157b39a69b26c26&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## Pure function

외부 scope에 영향을 미치지 않는 함수 → 외부 변수의 값을 바꿔서는 안된다 ! (side effect 방지)

primitive type을 parameter로 받는 경우(외부 변수도 또한 사용하지 않을 경우) pure function인 것을 보장할 수 있지만 object의 경우 reference로 전달받기 때문에 함수 내에서 이 object를 조작하면 원본 객체에도 영향을 미친다. 따라서 원본 객체에 영향을 미치지 않기 위해서 복사 객체를 만들고 복사된 객체에 조작을 가하는 방식으로 pure function을 만들어야 한다 ! 이때 deep copy가 필요 !

가장 간단한 deep copy :  `JSON.parse(JSON.stringify(object))`

## Shallow copy vs Deep copy

- `=` 연산을 통해 객체를 복사했을 때 새로운 객체를 생성하는 것이 아니라 원본 객체의 레퍼런스를 공유하게 된다! (주소값을 공유하게 된다) → Pass by Reference
- **shallow copy** : 새로운 객체를 생성하여 원본 객체 내부의 자식 객체에 대한 레퍼런스 값을 가짐

    → 복사된 객체의 변화가 원본 객체에 반영될 수 있음

- **deep copy** : 새로운 객체를 생성하여 원본 객체 내부의 자식 객체까지 재귀적으로 복사 과정을 진행한다

    →복사된 객체의 변화가 원본 객체에 반영되지 않음

## Immutable update
Whenever your object would be mutated, don’t do it. Instead, create a changed copy of it. <br>
**Shallow equality check** (Reference equality check)

- 참조형 객체의 변경을 비교할때 쉽게 비교 가능 ( value equality의 경우 nested structure 비교할 때 재귀 작업 필요 → 시간복잡도 증가 )
- 리액트에서는 리렌더링 필요성을 판단할 때 얕은 비교를 활용

<br/>
<br/>

## Closure

> A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function

- 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합
- 어떤 함수에서 선언한  변수를 참조하는 내부함수를 외부로 전달할 경우 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상
- GC가 어떤 값을 참조하는 변수가 하나라도 있을 때 그 값을 수집 대상에 포함시키지 않기 때문에 발생한다.

### 활용 사례
1. 콜백 함수 내부에서 외부 데이터로 사용하는 경우
2. 접근 권한 제어(정보 은닉)
3. 부분 적용 함수(n개의 인자를 받는 함수에서 미리 m개의 인자를 넘겨 받고 나머지 인자를 받았을 때 실행) -> 인자를 한 번에 하나씩만 받는 커링 함수
```javascript
function makeAdder(baseNum) {
  return function(num) {
    return baseNum + num;
  };
}
const add5 = makeAdder(5)
// makeAdder가 return된 이후에도 내부 함수인 add5에서 baseNum(5)에 접근 가능
add5(10) // 15
add5(20) // 25
```

### Lexical scope

```jsx
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```
- 렉시컬 스코프는 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 상위 스코프가 결정된다
- 🔗 참고 : [스코프](https://poiemaweb.com/js-scope#7-%EB%A0%89%EC%8B%9C%EC%BB%AC-%EC%8A%A4%EC%BD%94%ED%94%84)


## Symbol
es6에 도입된 새로운 데이터 타입이며 `Primitive type`이다. string과 함께 객체의 key로 사용될 수 있으나 순회할 때 조회되지 않는다.
```javascript
const sym = Symbol(); // new 키워드 없이 x, 매번 유니크한 값 생성
const obj = {
    [sym]: 'symbol'
};

for(const key in obj) {
  console.log(key, obj[key]);
} // -> 조회되지 않음
```

### Symbol.iterator
객체를 순회할 때 어떤 방식으로 순회할 것인지 설정하기 위한 symbol이다. `for of`, rest 연산자로 iterable(순회가능한) 객체를 만들기 위해서는 Symbol.iterator를 key로 같는 메서드를 정의해아 한다.
메서드는 next() 메서드를 갖는 iterator 객체를 반환해야 한다. next() 메서드는 value와 done을 속성으로 갖는 객체를 반환한다.
```javascript
const obj = {
    [Symbol.iterator]() {
        let val = 0;

        return {
            next() {
                return { value: val++, done: val > obj.maxValue };
            }
        };
    },
  
    maxValue: 10
};

console.log(...obj);
// -> 0 1 2 3 4 5 6 7 8 9
```
