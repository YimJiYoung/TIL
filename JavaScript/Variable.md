# Variable
- 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름. 
- 값의 위치를 가리키는 상징적인 이름
- 식별자 = 변수 이름

> 모든 식별자는 실행 컨텍스트에 등록된다.

## Variable declaration
변수 선언이란 변수를 생성하는 것
- 값을 저장하기 위한 메모리 공간 확보 + 변수 이름과 확보된 메모리 공간의 주소 연결
- `let`, `const`, `var`와 같은 키워드 사용하여 선언
- 선언하지 않은 식별자 접근 -> `ReferenceError`

# Hoisting

## Variable hoisting

변수 선언이 scope의 위쪽으로 옮겨지는 것을 의미한다. 

변수 및 함수 선언이 물리적으로 작성한 코드의 상단으로 옮겨지는 것은 아니고, 컴파일(런타임 이전) 단계에서 메모리에 저장된다.

```jsx
console.log(x); // undefined
var x = 5;
console.log(x); // 5
```

 `var` 키워드로 선언하면 변수 호이스팅이 일어난다.

첫번째 줄의 변수  `x`는 선언되기 전이므로 `Reference Error`가 떠야 하지만 변수 호이스팅으로 선언이 먼저 되었기 때문에 (선언은 되었지만 할당되지 않은) `undefined` 가 된다.

> 호이스팅은 변수 선언이 소스코드가 한 줄씩 실행되는 시점인 런타임이 아니라 그 이전 단계에서 먼저 실행되기 때문에 발생한다.

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


