## hooks

함수형 컴포넌트에서 상태 관리나 lifecyle API를 사용할 수 있도록 하는 기능이다.

### useState

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

- `window.addEventListener('click')` 과 같이 React에서 직접적으로 관리하지 않는 영역에서 setState 사용할 경우엔 동기적으로 실행 → ReactDOM.unstable_batchUpdate 사용

### useEffect

- 네트워크 요청, 이벤트 핸들러 등록과 같은 side effect 수행
- 의존성 배열 없을 때 렌더링 이후 매번 콜백 함수 실행
- effect 함수에서 반환하는 함수는 다음 effect 함수가 호출되기 전에 또는 언마운트 시 호출된다
- 의존성 배열에 빈 배열([]) 전달 시, 마운트 시에만 effect 함수 호출 언마운트 시 리턴 함수 호출
- effect 함수 내부에서 사용하는 컴포넌트 내의 지역 변수, 함수는 의존성 배열에 추가해야 한다. (함수는 useCallback으로 메모이제이션해야함)
- 참고: [https://ko.reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies](https://ko.reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies), [https://ko.reactjs.org/docs/hooks-effect.html](https://ko.reactjs.org/docs/hooks-effect.html)

### hook 규칙

1. 컴포넌트 내에서 hook 호출 순서는 항상 같아야 한다.
    - React가 useState 와 useEffect 가 여러 번 호출되는 중에도 Hook의 상태를 올바르게 유지할 수 있다 → React가 Hook이 호출되는 순서에 의존하여 각 useState와 useEffect를 구분하기 때문!
    - 참고: [https://ko.reactjs.org/docs/hooks-rules.html#explanation](https://ko.reactjs.org/docs/hooks-rules.html#explanation)
2. 함수형 컴포넌트나 커스텀 훅 내에서만 호출되어야 한다.
