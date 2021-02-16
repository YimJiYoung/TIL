## React
UI를 만들기 위한 JS 라이브러리

- html 처럼 선언적으로 UI 정의(JSX) → React가 알아서 렌더링
- view에 집중하여 개발 가능
   - 전역 상태 관리, 라우팅을 위해서는 직접 구현하거나 별도의 라이브러리 필요
   - UI 코드와 비지니스 로직 코드를 분리 가능 -> 비지니스 로직에 집중하여 개발 가능
- virtual DOM 
   - UI 상태 메모리에 저장, 변경이 필요한 부분 찾아서(diff 알고리즘) DOM에 업데이트 → 불필요한 업데이트 감소, 효율성 증가

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

```jsx
// React.createElement(component, props, ...children)

React.createElement(
   'button',
   { onClick: () => this.setState({ liked: true }) },
   '버튼',
);
```

**루트 DOM 노드에 element 렌더링**

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### Component

- 재사용 가능한 UI 조각
- “props”라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환

### SPA
사용자는 네비게이션 기능(브라우저의 뒤로가기, 앞으로가기)을 활용해서 SPAs에서도 편리하게 여러가지 화면을 접근해서 볼 수 있어야 한다.
- JS에서 브라우저 페이지 전환 요청 (서버로 요청X)
    - pushState, replaceState 함수 사용
- 브라우저 뒤로가기와 같은 사용자 페이지 전환 요청 → JS에서 처리 (서버로 요청 X)
    - popState 이벤트
```jsx
import React, { Component } from "react";

class App extends Component {
  state = {
    pageName: ""
  };
  componentDidMount() {
    window.onpopstate = (event) => {
      this.onChangePage(event.state);
    };
  }
  onChangePage = (pageName) => {
    this.setState({ pageName });
  };
  onClick1 = () => {
    const pageName = "page1";
    window.history.pushState(pageName, "", "/page1");
    this.onChangePage(pageName);
  };
  onClick2 = () => {
    const pageName = "page2";
    window.history.pushState(pageName, "", "/page2");
    this.onChangePage(pageName);
  };
  render() {
    const { pageName } = this.state;
    return (
      <div>
        <button onClick={this.onClick1}>page1</button>
        <button onClick={this.onClick2}>page2</button>
        {!pageName && <Home />}
        {pageName === "page1" && <Page1 />}
        {pageName === "page2" && <Page2 />}
      </div>
    );
  }
}

function Home() {
  return <h2>여기는 홈페이지입니다. 원하는 페이지 버튼을 클릭하세요.</h2>;
}
function Page1() {
  return <h2>여기는 Page1입니다.</h2>;
}
function Page2() {
  return <h2>여기는 Page2입니다.</h2>;
}

export default App;

// 출처 : https://github.com/landvibe/book-react/blob/master/1-chapter/7-router-test/src/App-2.js
```

### **React-router에서의 Routing**

React에서 사용되는 라우팅 라이브러리 중 react-router가 대표적이다. react-router는 Link, Route 컴포넌트와 같은 유용한 컴포넌트를 제공한다.
- pushState를 활용해 history를 저장하는 컴포넌트 => `<Link>`
- popstate를 통해 navigation 변화가 발생하면 event.state값을 받아 다시 렌더링 하는 컴포넌트 => `<Route>`

### Router 컴포넌트

라우터를 사용하기 위해선 상위 컴포넌트에서 정의해야 한다.

```jsx
import React from 'react';
import { HashRouter as Router } from 'react-router-dom';
import Routes from './routes';

function App() {
  return (
    <Router>
      <Routes />
    </Router>
  );
```

1. **BrowserRouter**
    - Link 컴포넌트 to속성에 이동할 경로 기술
    - Route 컴포넌트 path속성을 Link의 to속성과 매핑 component에 컴포넌트 경로 기술
    - 새로고침 하면 경로 못찾아서 에러남 → 추가 설정 필요
2. **HashRouter**
    - 주소에 해쉬(#)이 붙음
    - 새로 고침해도 그대로 나옴 -> #뒤에는 화면에서 읽는 경로이기 때문
    - 검색엔진으로 못읽는 단점때문에 거의 안씀
