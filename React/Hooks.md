# hooks

함수형 컴포넌트에서 상태 관리나 lifecyle API를 사용할 수 있도록 하는 기능이다.

### hook 규칙

1. 컴포넌트 내에서 hook 호출 순서는 항상 같아야 한다.
    - React가 useState 와 useEffect 가 여러 번 호출되는 중에도 Hook의 상태를 올바르게 유지할 수 있다 → React가 Hook이 호출되는 순서에 의존하여 각 useState와 useEffect를 구분하기 때문!
    - 참고: [https://ko.reactjs.org/docs/hooks-rules.html#explanation](https://ko.reactjs.org/docs/hooks-rules.html#explanation)
2. 함수형 컴포넌트나 커스텀 훅 내에서만 호출되어야 한다.


## useState

- 상태값 사용
- (React가 관리하는 범위에서는) setState 비동기적으로 배치 처리된다.

```jsx
function onClick() {
	setCount(count + 1);
	setCount(count + 1);
}
// render 한번 발생 / count 1만 증가
// count 2번 증가 시키고 싶다면 setCount(count => count + 1); 콜백 함수 사용하기
```

- `window.addEventListener('click')` 과 같이 React에서 직접적으로 관리하지 않는 영역에서 setState 사용할 경우엔 동기적으로 실행하기 때문에 배치로 처리하고 싶다면 ReactDOM.unstable_batchUpdate 함수를 사용해야 한다.

## useEffect

- 네트워크 요청, 이벤트 핸들러 등록과 같은 side effect 수행
- 의존성 배열 없을 때 렌더링 이후 매번 콜백 함수 실행
- effect 함수에서 반환하는 함수는 다음 effect 함수가 호출되기 전에 또는 언마운트 시 호출된다
- 의존성 배열에 빈 배열([]) 전달 시, 마운트 시에만 effect 함수 호출 언마운트 시 리턴 함수 호출
- effect 함수 내부에서 사용하는 컴포넌트 내의 지역 변수, 함수는 의존성 배열에 추가해야 한다. (함수는 useCallback으로 메모이제이션해야함)
- 참고: [https://ko.reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies](https://ko.reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies), [https://ko.reactjs.org/docs/hooks-effect.html](https://ko.reactjs.org/docs/hooks-effect.html)

## useRef

```jsx
const refContainer = useRef(initialValue);
```

initialValue로 전달한 값으로 초기화된 current 속성을 갖는 ref 객체를 반환한다. ref 객체는 전 생애주기 동안 같은 객체를 가르킨다.

- 주로 DOM 요소에 직접 접근하고 싶을 때 사용. (focus 주거나, 스크롤 위치, 크기를 얻어오기 위해)

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

- 컴포넌트 요소에게 ref 속성으로 전달할 경우 ref.current는 해당 컴포넌트의 인스턴스를 가리킴 → 해당 클래스의 메서드 호출 가능

```jsx
<Button ref={buttonRef} />

// 함수형 컴포넌트에게 ref 속성 전달하기 위해서는 React.forwardRef로 감싸주어야 한다.
```

- ref에 콜백 함수를 넘겨주면 해당 요소가 생성되거나 사라질 때 실행된다. 이때 새로운 함수가 입력될 때마다 다시 실행하므로 콜백 함수는 useCallback으로 감싸줘야 한다.

```jsx
function MeasureExample() {
  const [height, setHeight] = useState(0);

  const measuredRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []);

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  );
}
```

## useLayoutEffect

useEffect와 유사하지만 effect 함수가 동기적으로 실행된다는 점에서 다르다. 렌더링 후 DOM 요소의 레이아웃 정보를 얻거나 조건에 따라 리렌더링하고 싶을 때 사용할 수 있다. useLayoutEffect가 호출되는 시점은 렌더링이 DOM에 반영되고 브라우저 화면에 그리기 전이다.
