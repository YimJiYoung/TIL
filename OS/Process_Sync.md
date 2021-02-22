# Process Synchronization 

프로그램 내에서 어떤 데이터에 접근하여 처리하려 할 때 다음과 같은 과정을 거친다.

1. 저장소에서 데이터를 읽는다. (Read)
2. 읽어온 데이터에 연산을 한다. (Operation)
3. 처리된 데이터를 저장소에 저장한다. (Write)

이때 여러 프로세스가 동시에 같은 데이터(공유 데이터)에 접근하려 할 때 Race Condition이 발생하여 데이터 불일치 문제가 발생할 수 있다.
이런 문제를 해결하고 데이터의 일관성(consistency)을 유지하기 위해 프로세스 간의 실행 순서를 적절하게 정해주는 매커니즘이 필요하다.

### Race Condition
여러 프로세스가 동시에 공유 데이터에 접근하려 할 때 발생하는 현상. 
최종 연산 결과가 마지막 프로세스의 결과가 되는 등 프로세스의 실행 순서에 따라 결과값이 영향을 받고 데이터의 일관성을 해칠 수 있다.

## Critical Section
![critical section](https://www.studytonight.com/operating-system/images/critical-section-problem.png)
공유 데이터에 접근하는 코드 영역
