# Hoisting
## Variable hoisting

변수 선언이 scope의 위쪽으로 옮겨지는 것을 의미한다. 

변수 및 함수 선언이 물리적으로 작성한 코드의 상단으로 옮겨지는 것은 아니고, 변수 및 함수 선언이 컴파일 단계에서 메모리에 저장된다.

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

- `Primitive type` :  Number, String, Boolean, undefined, null ( **Pass by Value**)
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
