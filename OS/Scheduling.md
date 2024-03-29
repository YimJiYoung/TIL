## CPU Scheduler

Ready 상태의 프로세스 중에서 다음에 CPU에서 실행할 프로세스를 선택한다.

### Dispatcher

선택된 프로세스에게 CPU의 제어권을 넘긴다 (context swtich 발생)

### Non-Preemptive vs Preemptive

non-preemptive(비선점): CPU를 할당 받아 실행 중인 프로세스가 종료되거나 waiting 상태로 변할 때까지 다른 프로세스가 뺏을 수 없는 방식

preemptive(선점): CPU를 할당 받아 실행 중인 프로세스를 중지시키고 다른 프로세스가 뺏을 수 있는 방식

### Scheduling Criertia (성능 척도)

- CPU Utilization : CPU 이용률. CPU가 놀지 않도록 최대한 사용하자 !
- Throughput : 단위 시간 동안 처리된 프로세스의 수
- Turnaround Time : 프로세스가 ready queue에 들어와서 종료될 때까지의 시간 ( Burst time + Waiting time)
- Waiting Time : 프로세스가 ready queue에서 기다린 시간의 합
- Response Time : 프로세스가 ready queue에 들어와서 처음으로 실행되기까지의 시간

### FCFS(First Come First Served)

- 온 순서대로 처리
- 단점: 오래 걸리는 process가 먼저 와서 처리되면 응답 시간과 대기 시간이 길어진다.

### SJF(Shortest Job First)

- 예측 CPU burst time이 가장 짧은 순서대로 처리
- 장점: 응답 시간이 짧아진다. preemptive 방식의 경우 최소 평균 대기 시간 보장.
- 단점: Starvation 발생(burst time이 긴 프로세스의 경우, 짧은 프로세스들이 계속 생기면 실행되지 않는 문제)

### Priority Scheduling

- 우선순위가 높은 프로세스에게 CPU 할당
- 단점: Starvation 발생(우선순위가 낮은 프로세스가 실행되지 않는 문제) → 시간이 지날수록 프로세스의 우선순위를 증가시키는 aging으로 해결 가능

### Round Robin

- 각 프로세스는 주어진 시간(time quantum)만큼 돌아가면서 실행된다 (preemtive 방식)
- 장점: 응답시간 빨라짐
- 단점: quantum이 작으면 context switch 빈번하게 발생 → 오버헤드

### MultiLevel Queue

![](https://media.geeksforgeeks.org/wp-content/uploads/multilevel-queue-schedueling-1-300x217.png)
- 우선순위가 낮은 큐에서 작업 실행 중이더라도 상위 단계의 큐에 프로세스가 도착하면 CPU를 빼앗는 선점형 스케줄링
- 레디 큐를 각 성격에 따라 여러 개로 분리한다.
- 각 큐는 각자의 우선순위와 스케쥴링 알고리즘을 가진다. (각 큐는 라운드 로빈이나 FCFS등 독자적 스케줄링 사용 가능)
- 큐에 대한 스케쥴링이 필요하다.
  - Fixed Priority:  우선순위가 낮은 큐의 프로세스가 오랫동안 CPU 할당을 기다리는 Starvation 이 발생할 수 있음
  - Time Slice: 각 큐에 적절한 비율로 CPU 시간 할당

### MultiLevel Feedback Queue

![](https://media.geeksforgeeks.org/wp-content/uploads/Multilevel-Feedback-Queue-Scheduling-300x269.png)
- Multilevel Queue의 방식에서 프로세스의 큐 간 이동이 가능해졌다.
  - 우선순위 낮은 레디큐에는 큰 Time Quantum을 줌

### Real-time Scheduling
정해진 시간 내에 task를 완료해야 한다.
- Hard real-time system: real-time task가 정해진 시간 내에 반드시 완료해야 한다.
- Soft real-time system: real-time task가 정해진 시간 내에 완료되는 것을 보장하지 않음. 그러나 일반 task에 비해 높은 우선순위를 갖고 수행된다.

### Thread Scheduling
- user level thread: 사용자 수준의 thread library에 의해 스케쥴링된다.
- kernel level thread: 커널에 의해 스케쥴링된다.
