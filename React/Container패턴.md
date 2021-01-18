## Container Pattern
view를 담당하는 **presentaional component**와 data fetching, 상태 관리를 담당하는 **container component**를 분리하여 presentational component의 재사용성을 높이려는 디자인 패턴
#### 장점
- presentational component의 재사용성 증가
#### 단점
- 상태 관리가 필요한 부분은 container component로 분리해야 하기 때문에 불필요한 컴포넌트 중첩이 깊어지는 문제 발생
- 상태 관련 로직은 함수형 컴포넌트에서 hook으로 분리하여 대체 가능하므로 container pattern이 더이상 유용하지 않을 수 있다.

### React hook
함수형 컴포넌트에서 상태 관리나 lifecyle API를 사용할 수 있도록 하는 기능이다.
#### 장점
- 컴포넌트 사이에서 상태와 관련된 로직을 재사용하기 어렵다는 문제 해결 (기존에는 HOC와 같은 패턴 사용 -> wrapper 지옥 ..)
  - **Hook은 계층 변화 없이 상태 관련 로직을 재사용 가능**
- 간결한 코드 작성

## 참고
- https://blog.bitsrc.io/implementing-the-container-pattern-using-react-hooks-f490a8492d05
- https://ko.reactjs.org/docs/hooks-intro.html
