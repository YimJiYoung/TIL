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

 <br>
 <br>
 

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

