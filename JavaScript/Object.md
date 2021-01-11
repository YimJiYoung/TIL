## Object immutability

### Object.defineProperty

객체의 속성과 그 속성에 적용될 규칙(writable, configurable, enumerable ⇒ 기본값 false)을 정의한다.

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

### Object.preventExtensions

- 객체에 속성 추가 불가능
- 프로토타입 설정 불가능
- `Object.isExtensible` false로 설정

### Object.seal

- ...Object.preventExtensions
- 모든 속성의 `configurable` false로 설정
- `Object.isSealed` true로 설정

### Object.freeze

- ...Object.seal
- 모든 속성의 `writable` false로 설정
- `Object.isFrozen`  true로 설정
