# Deadlock
- 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태 
- 자원(Resource): I/O device, CPU cycle, memory space, sempahore 등 하드웨어, 소프트웨어 포함

## 발생 조건
1. **Mutual Exclusion**
- 한 시점에 하나의 프로세스만이 자원을 사용할 수 있다.
2. **No Preemption**
- 프로세스가 자원을 스스로 내놓을 때까지 강제로 자원을 빼앗을 수 없다.
3. **Hold and Wait**
- 자원을 가진 프로세스가 다른 자원을 기다릴 때 가진 자원을 놓지 않고 계속 가지고 있음
4. **Circular Wait**
- 자원을 기다리는 프로세스 간에 사이클 발생.

## DeadLock Prevention
데드락 발생 조건인 네 가지 조건 중 하나를 차단하여 데드락이 발생하지 않게 하는 방법.

- Hold and Wait
  - 방법1 : 필요한 모든 자원 프로세스 시작 시 할당 → 비효율적
  - 방법2 : 자원이 필요한 경우 보유 자원 모두 놓고 다시 요청
- No Preemption
  - preemption 방식 사용 : state를 쉽게 save하고 restore할 수 있는 자원(CPU, memory)에서 주로 사용
- Circular Wait
  - 모든 자원 유형에 할당 순서를 정하여 순서대로만 자원을 할당한다.

## Deadlock Avoidance
자원 요청에 대한 추가적인 정보를 이용하여 자원 요청 시 deadlock이 발생할 가능성이 있는지 확인(safe/unsafe) 후에 자원을 할당한다.

- 자원이 하나의 인스턴스만을 가지는 경우 →  Resource Allocate Graph Algorithm
- 자원이 여러 인스턴스를 가지는 경우 →  Banker's Algorithm

## Deadlock Ignorance
데드락은 드물게 발생하므로 이를 막기 위한 방법이 더 큰 오버헤드가 될 수 있다. 따라서 데드락이 발생하더라도 OS에서 별도의 처리를 하지 않고 사용자가 직접 처리하도록 하는 방법이다.
UNIX, Window를 포함한 대부분의 OS가 이 방법을 채택하고 있다.
