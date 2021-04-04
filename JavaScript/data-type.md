# Data Type

## Number

## String

## Boolean

## undefined

## null

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
객체를 순회할 때 어떤 방식으로 순회할 것인지 설정하기 위한 symbol이다. `for of`, rest 연산자로 iterable(순회가능한) 객체를 만들기 위해서는 Symbol.iterator를 key로 갖는 메서드를 정의해아 한다.
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
<br>
<br>

## Reference vs Value

- `Primitive type` :  Number, String, Boolean, undefined, null, Symbol ( **Pass by Value**)
- `Object type` : Array, Function, Object (**Pass by Refernece**)

primitive type을 담고 있는 변수의 경우 `=` 연산자를 통해 다른 변수에 할당했을 때 value 자체가 복사된다. 

그러나 Object type의 경우는 Object value 자체가 아니라 Object value에 대한 reference가 복사된다.

### Primitive value = immutable value

원시값은 변경 불가능한 값, 즉 read only 값이다 → 데이터의 신뢰성 보장 
원시값을 할당한 변수에 새로운 원시값을 재할당하면 메모리 공간에 저장되어 있는 재할당 이전의 원시값을 변경하는 것이 아니라 새로운 메모리 공간을 확보하고 재할당한 원시값을 저장한 후 변수는 새롭게 재할당한 원시값을 가리킨다.

#### Why Immutable?
- 모던 자바스크립트 Deep Dive에서는 원시 값의 불변성을 통해 예기치 못한 변경으로부터 자유로울 수 있고, 데이터의 신뢰성을 보장할 수 있다고 설명한다.
   - 여기서 말하는 데이터의 신뢰성 ? (내 생각) 어떤 변수가 참조하던 원시값이 같은 값을 가리키는 다른 변수에 의해 값이 변경될 수 있다면 예상치 못한 경우가 많아지고 프로그램의 복잡도가 증가할 것이다.
   - 또한 자바스크립트는 동적 언어이므로 다른 타입의 원시값을 재할당할 수도 있는데 타입마다 필요한 메모리 공간의 크기가 다르므로 기존의 원시값을 저장하던 메모리 공간을 그대로 사용하기 어려울 수 있을 것이다.


### Object value = mutable value

객체는 변경 가능한 값이다. 객체를 할당한 변수에는 생성된 객체가 실제로 저장된 메모리 공간의 주소(참조값)이 저장되어 있다. 객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있다.


#### Why mutable?
- 객체를 변경할 때마다 원시값처럼 이전 값을 복사해서 새롭게 생성한다면 신뢰성은 확보되겠지만 객체는 크기가 매우 클 수 있고, 원시값처럼 크기가 일정하지도 않으며, 프로퍼티 값이 객체일 수도 있어서 깊이 중첩된 객체의 경우 복사해서 생성하는 비용이 매우 많이 든다 → 메모리의 효율적 소비가 어렵고 성능이 나빠짐

### Shallow copy vs Deep copy

- `=` 연산을 통해 객체를 복사했을 때 새로운 객체를 생성하는 것이 아니라 원본 객체의 레퍼런스를 공유하게 된다! (주소값을 공유하게 된다) → Pass by Reference
- **shallow copy** : 새로운 객체를 생성하여 원본 객체 내부의 자식 객체에 대한 레퍼런스 값을 가짐

    → 복사된 객체의 변화가 원본 객체에 반영될 수 있음

- **deep copy** : 새로운 객체를 생성하여 원본 객체 내부의 자식 객체까지 재귀적으로 복사 과정을 진행한다

    →복사된 객체의 변화가 원본 객체에 반영되지 않음

#### Pure function

외부 scope에 영향을 미치지 않는 함수 → 외부 변수의 값을 바꿔서는 안된다 ! (side effect 방지)

primitive type을 parameter로 받는 경우(외부 변수도 또한 사용하지 않을 경우) pure function인 것을 보장할 수 있지만 object의 경우 reference로 전달받기 때문에 함수 내에서 이 object를 조작하면 원본 객체에도 영향을 미친다. 따라서 원본 객체에 영향을 미치지 않기 위해서 복사 객체를 만들고 복사된 객체에 조작을 가하는 방식으로 pure function을 만들어야 한다 ! 이때 deep copy가 필요 !

가장 간단한 deep copy :  `JSON.parse(JSON.stringify(object))`

### Immutable update
Whenever your object would be mutated, don’t do it. Instead, create a changed copy of it. <br>
**Shallow equality check** (Reference equality check)

- 참조형 객체의 변경을 비교할때 쉽게 비교 가능 ( value equality의 경우 nested structure 비교할 때 재귀 작업 필요 → 시간복잡도 증가 )
- 리액트에서는 리렌더링 필요성을 판단할 때 얕은 비교를 활용
