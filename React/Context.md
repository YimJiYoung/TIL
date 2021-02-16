### Context API

어떤 컴포넌트에서 하위 컴포넌트로 데이터를 전달하고자 하는데 하위 컴포넌트가 몇단계 떨어져 있는 경우 중간에 있는 컴포넌트에 불필요하게 props 넘겨주지 않기 위해 사용할 수 있는 것이 context!

```jsx
const MyContext = React.createContext(defaultValue);

// MyContext.Provider, MyContext.Consumer

<MyContext.Provider value={/* 어떤 값 */}>
```

- Consumer 컴포넌트는 해당 context의 가장 가까운 부모 Provider를 찾아 값을 받아온다. 부모 Provider가 없는 경우는 defaultValue로 값이 설정된다.
- Provider 하위에서 context를 구독하는 모든 컴포넌트는 Provider의 value prop가 바뀔 때마다 다시 렌더링 된다. context 값의 바뀌었는지 여부는 Object.is와 동일한 알고리즘을 사용해 이전 값과 새로운 값을 비교해 측정된다.
- 주의할 점 ! Provider에서 value 넘겨줄 때 객체리터럴 사용할 시 렌더링 될때마다 새로운 객체를 생성하여 넘겨주게 되므로 value가 바뀌었다고 인식되어 Consumer 컴포넌트에서의 불필요한 렌더링이 발생한다.
