## React
UI를 만들기 위한 JS 라이브러리

- html 처럼 선언적으로 UI 정의(JSX) → React가 알아서 렌더링
- view에 대한 부분에 집중.
- 상태 관리 라이브러리를 사용하여 비지니스 로직 집중
- virtual DOM - 변경 사항 찾아서(diff 알고리즘) DOM에 업데이트

### JSX

- UI를 기술하기 위해서 사용하는 HTML과 유사한,  JavaScript를 확장한 문법.
- React Element를 생성한다.
- JSX은 표현식 이기 때문에 변수 할당, return 값으로 사용 가능.
- JSX는 Babel을 통해 React.createElement() 호출로 컴파일된다.
```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

// after transpiling
 
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);

// -> 따라서 JSX 문법을 사용할 땐 React import 해줘야 함 !! 

// createElement를 통해 생성된 element 예시(단순화)
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

### element

- 브라우저 DOM 엘리먼트와 달리 React 엘리먼트는 일반 객체이며(plain object) 쉽게 생성 가능
- React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트

**루트 DOM 노드에 element 렌더링**

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### Component

- 재사용 가능한 UI 조각
- “props”라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환
