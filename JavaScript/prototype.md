## Prototype

JavaScript는 프로토타입 기반의 언어이다. 프로토타입 기반 언어에서는 어떤 객체를 원형(prototype)으로 삼고 이를 참조함으로써 상속을 구현한다.

- 모든 객체는 자신의 부모 역할을 하는 객체를 가르키는 `[[prototype]]` 속성을 갖는다.
- `[[prototype]]` 의 값은 객체 또는 `null`이다.

### Property Lookup

객체의 프로퍼티에 접근할 때, 자바스크립트 엔진은 먼저 해당 객체에 찾는 프로퍼티가 있는지 확인한다. 찾지 못하면 객체의 `[[prototype]]` 체인을 따라서 프로퍼티가 존재하는지 찾게 된다. 만약 `[[prototype]]` 이 `null` 이 될 때까지 해당 프로퍼티를 찾지 못하면 `undefined` 를 반환한다.

```jsx
// [객체 리터럴의 prototype chain]
// 객체 리터럴의 __proto__는 Object.prototype
// Object.prototype의 __proto__는 null

__proto__ === null
|
|
__proto__ === Object.prototype
|
|
{ object literal }
```

### 함수의 prototype

모든 함수는 `[[prototype]]` 과 별도의 `prototype` 객체를 갖는다. `prototype` 또한 객체이기 때문에 `prototyped` 의 `[[prototype]]` 은 `Object.prototype`이다. 여기서 가장 중요한 점!

- **new 키워드로 만들어진 객체의 `[[prototype]]` 은 생성자 함수의 `prototype` 으로 설정된다.**

```jsx
function Fn() {}
const obj = new Fn();

// new Fn()으로 생성된 객체 obj의 prototype chain
__proto__ === null
|
|             
__proto__ === Object.prototype
|
|
__proto__ === Fn.prototype
|
|
obj
```

### 상속 구현

```jsx
function Fn() {}

Fn.prototype.print = function() {
    console.log("Calling Fn.prototype's print method");
};

const obj = new Fn();
obj.print(); 
```

위와 같이 생성자 함수의 `prototype` 에 메서드를 정의하여 상속을 구현할 수 있다. 해당 함수를 통해 만들어진 객체(obj)는 생성자 함수의 프로토타입에 정의되었던 함수 `print`에 접근하여 사용할 수 있다. 이는 생성자 함수 내부에서 `this.print`로 메서드를 정의하는 것보다 메모리 면에서 효율적이다. (`prototype`은 생성된 모든 객체에서 공유되지만 `this.print`로 정의하면 생성된 객체마다 자신의 `print` 함수를 갖게 되기 때문에 비효율적임 )

### Object.create()

객체의 프로토타입을 수동으로 설정하여 생성할 수 있도록 하는 함수. 파라미터로 넘겨진 객체를 프로토타입으로 설정한 새로운 객체를 반환한다. 상위 클래스를 확장하는 하위 클래스를 구현할 때 사용된다.

```jsx
// Shape - 상위클래스
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 상위클래스 메서드
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - 하위클래스
function Rectangle() {
  Shape.call(this); // super 생성자 호출.
}

// 하위클래스는 상위클래스를 확장
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();
```

참고 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
