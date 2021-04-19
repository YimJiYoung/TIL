## 프로퍼티 어트리뷰트

### 내부 슬롯과 메서드

- 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript에서 사용하는 의사 프로퍼티와 메서드
- 자바스크립트 엔진의 내부 로직이므로 직접 접근하거나 호출할 수 있는 방법을 제공하지 않지만 일부 직접적으로 접근할 수 있는 수단을 제공한다.
    - `[[Prototype]]` → __proto__
    - `[[GetPrototypeOf]]` → getPrototypeOf

### 프로퍼티 어트리뷰트와 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 **프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트**를 기본값으로 자동 정의한다.

- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값
    - `[[Value]` : 프로퍼티의 값
    - `[[Writable]]` : 값의 갱신 가능 여부
    - `[[Enumerable]]` : 열거 가능 여부
    - `[[Configurable]]` : 재설정 가능 여부
- `Object.getOwnPropertyDescription` 메서드를 통해 간접적으로 접근 가능
    - **프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체** 반환

```jsx
// Object.getOwnPropertyDescriptor(obj, prop) - 하나의 프로퍼티에 대해
// Object.getOwnPropertyDescriptors(obj) - 객체의 모든 프로퍼티에 대해

const person = { name: 'Lee' }

person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));

/*
{
 name: {value: 'Lee', writable: true, enumerable: true, configurable: true}
 age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

### 데이터 프로퍼티와 접근자 프로퍼티

#### 데이터 프로퍼티

키와 값으로 구성된 일반적인 프로퍼티

- `[[Value]]` : 프로퍼티의 값
- `[[Writable]]` : 값의 변경 가능 여부. false인 경우 `[[Value]]` 의 값을 변경할 수 없음
- `[[Enumerable]]` : 열거 가능 여부. false인 경우 for ..in문이나 Object.keys() 메서드 등으로 열거 불가능
- `[[Configurable]]` : 재설정 가능 여부. false인 경우 프로퍼티를 삭제하거나 프로퍼티 어트리뷰트를 변경할 수 없음. (단 `[[Writable]]` 이 true인 경우 `[[Value]]`, `[[Writable]]` 변경 가능)

#### 접근자 프로퍼티

- 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(gettter/setter 함수)로 구성된 프로퍼티
    - `[[Get]]` : 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수
    - `[[Set]]` : 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수
    - `[[Enumerable]]`
    - `[[Configurable]]`

```jsx
const person = {
	firstName: 'Ungmo',
	lastName: 'Lee',
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	},
  set fullName(name) {
		[this.firstName, this.lastName] = name.split(' ');
  }
}

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: "Ungmo", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}

// 접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 메서드가 호출된다.
console.log(person.fullName); 
// "Ungmo Lee
```

### 프로퍼티 정의

새로운 프로퍼티를 추가하면서 명시적으로 프로퍼티 어트리뷰트를 정의하거나, 기존 프로퍼티의 어트리뷰트 재정의하는 것 → Object.defineProperty()

#### Object.defineProperty()

- `writable`: 할당 연산자로 속성의 값을 바꿀 수 있는지 여부. false로 설정된 경우 해당 속성의 값을 바꾸려는 연산은 실패한다(에러가 발생하진 않음).

    ```jsx
    const obj = {};

    Object.defineProperty(obj, 'testValue', {
        value: 17,
        writable: false,
        configurable: true,
        enumerable: true
    });

    obj.testValue = 22; // fails silently
    console.log(obj.testValue); // -> 17
    ```

- `enumerable` : 대상 객체의 속성 열거 시 노출되는지 여부. false로 설정된 경우 `for...in` 반복문이나 `Object.keys` 메서드에서 조회되지 않는다. (`Object.values`에서도 해당 속성의 값은 조회되지 않음)
- `configurable`: 속성을 삭제할 수 있고 속성의 규칙을 바꿀 수 있는지(재설정 가능) 여부. false로 설정된 경우 `defineProperty`를 통해 다른 속성의 규칙을 적용하면 에러 발생. (writable true → false는 가능)

    ```jsx
    var o = {};
    Object.defineProperty(o, 'a', {
      get() { return 1; },
      configurable: false
    });

    Object.defineProperty(o, 'a', {
      configurable: true
    }); // throws a TypeError

    delete o.a; // fails silently
    ```
    

```jsx
const person = {}

Object.defineProperty(person, 'firstName', {
	value: 'Ungmo',
	writable: true,
	enumerable: true,
	configurable: true
})

Object.defineProperty(person, 'lastName', {
	value: 'Lee', 
 // writable, enumerable, configurable의 default 값은 false (value는 undefined)
})

console.log(Object.keys(person)) // ["firstName"]
// lastName은 enumerable이 false이므로 누락

person.lastName = 'Kim'
// writable이 false이므로 값을 변경할 수 없음. 에러 없이 무시된다.

delete person.lastName
// configurable이 false이므로 값을 삭제할 수 없다. 에러 없이 무시된다.

Object.definedProperty(person, 'lastName', { enumerable: true });
// TypeError 발생! configurable이 false이므로 프로퍼티를 재정의할 수 없다.
// writable이 true인 경우 writable false로 재정의하거나 value를 수정하는 것은 가능

Object.definedProperty(person, 'fullName', {
	get() {
		return `${this.firstName} ${this.lastName}`;
	},
  set() {
		[this.firstName, this.lastName] = name.split(' ');
	},
	enumerable: true,
	configurable: true
})
// get, set의 default 값은 undefined

// descriptors는 데이터, 접근자 디스크립터 중 한 가지 유형을 가져야 함
// 공통 - enumerable, configurable
// 데이터 - value, writable
// 접근자 - get, set
// value, writable, get, set 모두 갖고 있지 않으면 데이터 디스크립터로 인식
```

## Object Immutability

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

### Object.preventExtensions

- 프로퍼티 추가 X
- 프로토타입 재설정 불가능
    - Object.setPrototypeOf  메서드 사용 시 에러 발생

→ Object.isExtensible - false

### Object.seal

- 프로퍼티 추가 X
- 프로퍼티 삭제 X
- 프로퍼티 어트리뷰트 재정의 X

→ 모든 속성의 configurable false로 설정

→ Object.isSealed - true

### Object.freeze

- 읽기만 가능
- 프로퍼티 추가 X
- 프로퍼티 삭제 X
- 프로퍼티 값 쓰기 X
- 프로퍼티 어트리뷰트 재정의 X

→  모든 속성의 writable, configurable false로 설정

→ Object.isFrozen - true
