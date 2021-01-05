## Process

### Process란?

- a program in execution (실행 중인 프로그램)
- 디스크에 저장되어 있던 실행 가능한 프로그램이 메모리에 적재되어 운영체제가 관리하는 상태

### Process address space
 프로그램이 실행되면 메모리에 그 프로세스만의 독자적인 주소 공간을 갖게 된다.
![process](https://woovictory.github.io/img/process_os.png)

- `Code`: 프로세서가 실행할 바이너리 코드(명령어)를 저장해 놓은 영역
- `Data`: 전역 변수 또는 static 변수가 저장되는 영역
- `Stack`: 함수를 호출하고 리턴할 때의 복귀 주소나 지역 변수와 같은 일시적인 데이터를 저장하는 영역
- `Heap`: 프로그램 실행 중에 동적으로 메모리를 할당할 수 있는 자유로운 영역

### Process State
![process state](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_02_ProcessState.jpg)
![process state2](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile28.uf.tistory.com%2Fimage%2F1260995050E18C8C24C034)
- `Running` : CPU에서 실행 중인 상태
- `Ready`: CPU에서 실행되기를 기다리는 상태
- `Wait`: 요청한 event(ex. I/O)를 기다리는 상태
- `Suspended`: 외부적인 이유로 프로세스의 수행이 정지된 상태(ex. Swap out)

### Process Control Block

![pcb](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_03_PCB.jpg)

OS에서 각 프로세스를 관리하기 위해 필요한 정보를 저장하기 위한 자료구조

1. OS 관리 상 필요 정보
    - Process state, Process ID
    - scheduling information, priority
2. CPU 수행 관련 레지스터 값
    - Program counter, registers
3. 메모리 관련
    - Process address space(code, stack, data)의 위치 정보
4. 파일 관련
    - Open file descriptors

### Context Switch

![context_switch](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_04_ProcessSwitch.jpg)

- 실행 중인 프로세스가 바뀌면 context switch가 발생한다.
- 실행 중이던 프로세스의 PC, registers 등의 정보 PCB에 저장 → 실행할 프로세스의 PCB에서 정보 불러옴
- user mode → kernel mode로 바뀌는 것은 context switch가 아님 (이 경우도 CPU 수행 정보를 PCB에 저장하게 되지만 context swtich보다는 오버헤드가 적다)

### Thread
- 경량 프로세스(Light weight process)라고 불리기도 하며, 프로세스 내의 CPU 수행 단위를 의미한다.
- 각 thread마다 cpu 관련 정보(PC, registers)와 stack 별도로 가진다.
![thread](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_01_ThreadDiagram.jpg)
#### 장점 :
- 스레드를 사용하면 사용자에 대한 응답성을 증가시킬 수 있다. (ex. 하나의 thread에서 waiting 상태인 경우에 다른 thread를 실행하여 빠른 처리를 할 수 있다.)
- 프로세스 자원과 메모리를 공유할 수 있다  → 자원을 공유하기 때문에 경제적이다.

### User-level thread vs Kernel-level thread

**user level thread (supported by library)**

- user application에서 사용되는 thread
- kernel은 user level thread의 존재를 알지 못함 -> 실제로 thread가 실행되기 위해서는 kernel level thread에 매핑되어야 함

**kernel level thread (supported by kernel)**

- cpu에 할당되는 스케쥴링의 대상
- user level thread 실행

### Process Creation

부모 프로세스가 자식 프로세스를 복제 생성

- fork() : 새로운 프로세스 복제 생성, 주소 공간 할당.
- exec(): 기존의 프로세스에서 새로운 프로그램 실행.
- wait(): 자식 프로세스가 종료될 때까지 기다린다 (waiting 상태) / ex. 입력 프롬프트
- exit(): 프로세스 자발적으로 종료
    - 비자발적인 종료: 부모 프로세스가 자식 프로세스 강제 종료 (ex. 한계치 넘어서는 자원 요청), kill, 부모 프로세스가 먼저 종료되는 경우(부모 프로세스가 종료되기 전에 자식 프로세스 종료)

## IPC (Interprocess Communication)

프로세스는 각자의 주소 공간을 가지고 독립적으로 수행되므로 원칙적으로 다른 프로세스의 수행에 영향을 미치지 못함

![IPC](https://networkencyclopedia.com/wp-content/uploads/2019/09/inter-process-communications-models.png)

### Message passing

커널을 통해 메시지 전달

방식
- Direct: 통신하려는 프로세스의 이름 명시적으로 표시
- Indirect: mailbox(또는 port)를 통해 메시지 간접 전달

### Shared memory

다른 프로세스 간에도 일부 주소 공간 공유

- 장점: 커널을 거치지 않고 통신하므로 Message passing보다 빠르다.
- 단점: 여러 프로세스에서 동시에 같은 데이터에 접근할 경우에 대한 처리 필요
