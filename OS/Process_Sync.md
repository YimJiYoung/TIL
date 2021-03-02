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
- 공유 데이터에 접근하는 코드 영역을 의미한다. 

## Ciritical Section Problem Solution
critical section에는 한 번에 오직 하나의 프로세스만이 접근할 수 있어야 한다. 이런 해결하기 위해서는 다음의 3가지 조건을 만족해야 한다.
- **Mutual Exclusion**: 한 프로세스가 critical section에 있을 때 다른 프로세스는 critical section에 접근할 수 없다.
- **Progresss**: critical section에 있는 프로세스가 없고 critical section에 접근하고자 하는 프로세스가 있을 경우 해당 프로세스가 critical section에 접근할 수 있어야 한다.
- **Bounded Waiting**: critical sescion에 들어가고자 대기하는 프로세스가 무한정 기다리지 않아야 한다. 


### Peterson’s Solution
```
// shared valriable: flag, turn

do {
  flag[i] = true
  turn = j
  while(flag[j]&& turn == j);
  
  // critical section
  
  flag[i] = false
  
  // remainder section
} while(true);
```
- 세가지 조건 모두 만족
- 단점: busy waiting(조건 만족할 때까지 while문 돌면서 기다림)


### TestAndSet
동기화 문제 하드웨어적으로 해결. test&modify하는 연산을 atomic하게 수행한다.
- TestAndSet(a) : a의 값을 읽고 true로 설정, 읽은 값 반환.
```
// shared valriable: lock

do {
  while(TestAndSet(lock));
  
  // critical section
  
  lock = false;
  
  // remainder section
} while(true);
```

## Semaphore
앞선 방식과 같이 critical section 문제를 해결하기 위한 추상 자료형. 정수값 S을 가지고 P, V의 Atomic 연산을 할 수 있다.
P는 critical section에 들어가기 전에 수행되고 V는 나올 때 수행된다. 
- Binary Semaphore: S는 0 또는 1의 값을 가진다 -> mutex, lock
- Count Semaphore: S는 가능한 자원의 수를 의미한다.
```
// busy waiting 방식

P(S) 
// while(S <= 0);
// S-- 

V(S)
// S++

semaphore mutex; // 1로 초기화 - 한 번에 하나의 프로세스만이 접근할 수 있다

do {
  P(mutex)
  // critical section
  V(mutex)
  
  // remainder section
} while(true);
```
### Producer-Consumer Problem (using Semaphore)
- Semaphore: full(0으로 초기화), empty(n으로 초기화), mutex(1로 초기화)
```
Producer:
1. empty buffer 확인
2. lock -> shared data 추가 -> unlock
3. full buffer 증가

 do {
     ...
     아이템을 생산한다.
     ...
     P(empty);  //버퍼에 빈 공간이 생길 때까지 기다린다.
     P(mutex); //임계 구역에 진입할 수 있을 때까지 기다린다.
     ...
     아이템을 버퍼에 추가한다.
     ...
     V(mutex); //임계 구역을 빠져나왔다고 알려준다.
     V(full);  //버퍼에 아이템이 있다고 알려준다.
 } while (1);

Consumer:
1. full buffer 확인
2. lock -> shared data 감소 -> unlock
3. empty buffer 

do {
     P(full);    //버퍼에 아이템이 생길 때까지 기다린다.
     P(mutex);
     ...
     버퍼로부터 아이템을 가져온다.
     ...
     V(mutex);
     V(empty); //버퍼에 빈 공간이 생겼다고 알려준다.
     ...
     아이템을 소비한다.
     ...
 } while (1);
```

## Monitor
![monitor](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTTGpXqwFgovBDjM0btchO6oYXGuZSVE2MK3Q&usqp=CAU)
- 동시 접근을 막기 위한 높은 추상화 수준의 자료형이다.
- 한번에 하나의 프로세스(스레드)만이 monitor의 operation을 수행할 수 있다 -> semaphore처럼 critical section에 들어가기 전에 lock 걸어줄 필요 없음 ‼
- condition variable: 값을 가지지 않고 조건에 따라 wait/signal 연산을 통해 프로세스를 sleep/awake 시키는 용도로 사용한다.
