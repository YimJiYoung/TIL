## 실행 컨텍스트

실행할 코드에 제공할 환경 정보들을 모아놓은 객체

- VariableEnvironment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보. 선언 시점의 LexicalEnvironment의 스냅샷으로 변경 사항이 반영되지 않음.
- LexicalEnvironment: 처음에는 VariableEnvironment와 같지만 변경 사항이 실시간 반영
- ThisBinding: this가 가리키는 대상 객체

### Lexical Environment

### **environmentRecord**

  현재 컨텍스트와 관련된 코드의 식별자 정보 저장. 컨텍스트 구성 시 매개변수 식별자, 선언된 함수 자체, var로 선언된 변수의 식별자를 코드 훑어가며 순서대로 수집 → 코드 실행되기 전에 변수명을 이미 알고 있으며 해당 변수에 대한 메모리 공간 미리 확보 **(호이스팅 발생)**

### outerEnvironmentReference

호출된 함수가 선언되는 시점의 Lexical Environment를 참조한다

- 스코프 체인 : 코드 상에서 어떤 변수에 접근하려고 하면 현재 컨텍스트의  Lexical Environment 탐색 → (없을 시) outerEnvironmentRecord에 담긴  Lexical Environment 탐색 → (반복) → 전역 컨텍스트의 Lexical Environment에도 없을 시 undefined 반환

### this

실행 컨텍스트의 thisBinding에는 this로 지정된 객체가 저장된다. 실행 컨텍스트 활성화 당시에 this가 지정되지 않은 경우는 전역 객체로 설정된다. 그 밖에 함수를 호출하는 방법에 따라 this에 저장되는 대상이 달라진다. ([this문서](https://github.com/YimJiYoung/TIL/blob/master/JavaScript/this.md) 참고)


## 참고
- 코어 자바스크립트
