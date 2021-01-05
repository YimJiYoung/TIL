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
