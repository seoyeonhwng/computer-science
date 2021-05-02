### 프로세스의 종류
- I/O bound process : CPU burst는 적고 I/O burst가 많은 작업 ex) 사용자와 인터렉티브한 프로그램
- CPU bound process : CPU burst가 많고 I/O burst가 적은 작업 ex) 과학 계산용 프로그램
- 컴퓨터 안에는 이러한 여러 종류의 프로세스가 섞여있기 때문에 CPU 스케쥴링이 필요함
- I/O bound process는 사용자와 인터렉티브하기 때문에 적절한 시간 내에 응답을 해야함 -> CPU를 우선적으로 할당!

### CPU 스케쥴링
- ready queue에 있는 프로세스들 중에서 어떤 프로세스에게 CPU를 줄지 결정하는 매커니즘
- 이슈1) 어떤 프로세스에게 CPU를 줄 것인가
- 이슈2) CPU를 할당하고 나면 해당 프로세스가 계속 쓰게할 것인가(non-preemptive) 중간에 뺏어올 수도 있는가(preemptive)

### CPU scheduler & Dispatcher
- CPU scheduler
  - ready 상태의 프로세스 중에서 CPU 제어권을 넘거줄 프로세스를 선택
  - 커널 안에서 스케쥴링을 하는 코드
- Dispatcher
  - 스케쥴러에 의해 선택된 프로세스에게 CPU 제어권을 넘겨줌 (context switching!)
  - 커널 안에서 CPU 제어권을 넘겨주는 코드

### CPU 스케쥴링 알고리즘
- FCFS (First Come First Serve)
  - 먼저 온 순서대로 CPU를 할당 / non preemptive
  - 단점) convoy effect : CPU burst time이 긴 프로세스가 먼저 온다면 전체적인 waiting time이 길어짐
- SJF (Shortest Job First)
  - CPU burst time이 가장 짧은 프로세스에게 CPU를 할당 / non preemptive
  - 단점) starvation : CPU burst time이 짧은 프로세스를 선호하기 때문에 긴 프로세스는 무한정 기다림
- SRTF (Shortest Remain Time First)
  - 남은 CPU burst time이 가장 짧은 프로세스에게 CPU를 할당 / preemptive
  - 단점) starvation : CPU burst time이 짧은 프로세스를 선호하기 때문에 긴 프로세스는 무한정 기다림
- Priority Scheduling
  - 우선순위가 제일 높은 프로세스에게 CPU를 할당 / preemptive, non preemptive
  - 단점) starvation : 우선순위가 낮은 프로세스는 무한정 기다림
  - 해결책) aging : 시간이 흐를때마다 프로세스의 우선순위를 증가시킴
- Round Robin
  - 모든 프로세스에게 동일한 크기의 할당 시간(time quantum, q)만큼 CPU를 할당
  - 할당 시간이 지나면 프로세스는 CPU를 뺏기고 ready queue로 이동
  - 장점) responsse time이 짧음 / waiting time과 CPU burst time이 비례
  - q가 커지면 FCFS, q가 작아지면 context switch 오버헤드가 큼
  - CPU burst time이 섞여있을때 효율적
