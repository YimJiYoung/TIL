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
- `Running` : CPU에서 실행 중인 상태
- `Ready`: CPU에서 실행되기를 기다리는 상태
- `Wait`: 요청한 event(ex. I/O)를 기다리는 상태
- `Suspended`: 외부적인 이유로 프로세스의 수행이 정지된 상태(ex. Swap out)
