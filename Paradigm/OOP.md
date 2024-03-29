# Object Oriented programming
프로그램을 명령어의 목록으로 보는 기존의 절차 지향적인 시각으로 벗어나 **독립적인 단위인 객체들의 협력**으로 보는 것
- 객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법이다.
- 자율적인 객체란 상태와 행위를 함게 지니며 자신의 행동을 스스로 결정하고 책임지는 객체를 의미한다.
- 객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력하며 각 객체는 협력 내에서 정해진 역할(관려된 책임의 집합)을 수행한다.
- 객체는 다른 객체와 협력하기 위해 메시지를 전송하고, 메시지를 수신한 객체는 메시지를 처리하는데 적합한 메서드를 자율적으로 선택한다. (객체의 자율성)


## 타입과 추상화

### 추상화
캡슐화와 비슷한 의미. 내부의 복잡한 로직이나 구조는 숨기고 외부에 제공된 인터페이스를 통해 필요한 기능만을 사용할 수 있도록 한다.

### 상속
공통적인 부분을 분리하여 상위 클래스로 만들고 여러 클래스에서 이를 상속받아 재사용하는 것
- 부모 클래스의 구현이 자식 클래스에게 노출되기 때문에 캡슐화가 약해질 수 있다.
- 실행 시점에 객체의 종류를 변경하는 것이 불가능하다.

### 합성
다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 재사용하는 방법

### 다형성
하나의 요소가 다양한 성질로 동작하는 것
- 오버라이딩: 같은 이름의 메소드가 여러 클래스에서 다른 기능을 하는 것
- 오버로딩: 같은 이름의 메소드가 인자의 개수나 자료형에 따라서 다른 기능을 하는 것

## SOLID
바람직한 OOP는 높은 응집도와 낮은 결합도를 가져야 한다.
- 높은 응집도: 밀접한 연관성이 있는 애들 모아두기
- 낮은 결합도: 의존성이 있는 애들은 의존성을 최대한 낮추기

SOLID는 좋은 객체 지향 설계를 위한 규칙이며 가독성 좋고, 유연하며, 유지보수용이한 코드를 목적으로 한다.

### SRP(Single Response Priciple)
- 하나의 클래스는 하나의 명확한 책임만 가져야 한다.
- 클래스가 비대해지면 명확한 책임을 정의하고, 그외 것들은 별도 클래스로 분리한다. (응집도 있는 클래스 만들기)

### OCP(Open/Closed Principle)
- 클래스, 모듈, 함수는 확장에는 열려 있으나 변경에는 닫혀 있어야 함
- 확장은 유연하게 가능해야 하지만 변경은 최소화되어야 함

### LSP(Liskov Substitution Principle/ 리스코프 치환 원칙)
- 하위 클래스는 상위 클래스를 대체할 수 있어야 한다. 즉 부모 클래스가 들어갈 자리에 자식 클래스를 넣어도 계획대로 잘 작동해야 한다는 것.(잘못된 상속 관계를 만들지 말 것)
- 참고 : http://stg-tud.github.io/sedc/Lecture/ws13-14/3.3-LSP.html#mode=document

### ISP(Interface Segregation Principle/ 인터페이스 분리 원칙)
- 큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 한다.

### DIP(Dependency Inversion Principle/ 의존성 역전 원칙)
- 추상성이 높고 안정적인 고수준의 클래스는 구체적이고 불안정한 저수준의 클래스에 의존해서는 안된다.
- 상위레벨 모듈이 하위레벨의 구체적인 구현 방식을 알 필요가 없음.
- 의존성 주입
```
// HighLevelModule 내에서 new로 구현체를 직접 만들어 사용하면 안된다! 
const highLevel = new HighLevelModule('data', new lowLevel());
```
