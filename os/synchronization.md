- 공유 자원 -> 동기화 문제 발생 -> lock으로 해결 -> 데드락 발생 가능

### Process synchronization
- 멀티 프로세스(스레드) 환경에서 공유 데이터를 동시에 접근하는 경우 데이터 불일치가 발생할 수 있음
- 일관성을 유지하기 위해 프로세스간 실행 순서를 정해주어야 함
- race condition : 여러 프로세스(스레드)들이 동시에 공유 데이터에 접근하는 상황
  - race condition을 막기 위해서 동기화가 필요함!!

### critical section problem
- critical section : 공유 데이터를 접근하는 코드
- critical section problem 기본 조건
  - mutual exclusion : 프로세스가 crtitical section을 수행중이면 다른 프로세스는 실행할 수 없음
  - progress : critical section에 실행 중인 프로세스가 없다면 진입하려는 프로세스를 적절히 선택하여 진입시킴
  - bounded waiting : critical section을 수행하고자 하는 프로세스는 무한정 기다리면 안됨
- 해결책) lock과 세마포어!

### lock
- critical section에 들어가고자 하는 프로세스는 lock을 획득하고 빠져나올때 lock을 해제 -> 동시 접근 X
- 단점) busy waiting : critical section 진입을 위해 while문을 계속 실행하면서 CPU를 낭비

### 세마포어
- 사용 가능한 자원의 수로 초기화 된 후 자원을 사용하면 세마포어 감소, 방출하면 증가
- 카운팅 세마포어 / 이진 세마포어 (mutex)
- 단점) busy waiting

### 데드락
- 프로세스가 자원을 얻지 못해서 다음 처리를 하지 못하고 무한 대기하는 상태
- 발생 조건 (4가지 조건이 동시에 성립할때 발생)
  - mutual exclusion : 자원은 하나의 프로세스만 사용할 수 있음
  - hold and wait : 본인이 가진 자원은 내어놓지 않고 다른 자원을 기다림
  - No preemption : 프로세스는 사용중인 자원을 빼앗기지 않음
  - circular wait : 자원을 기다리는 프로세스들 간에 사이클이 형성됨

### 데드락 해결 방법
- prevention : 데드락이 생기지 않도록 미연에 방지 (강한 처리)
  - 자원 할당시 데드락 발생 조건 중 하나라도 만족되지 않도록 함 -> 자원 낭비가 심함
- avoidance : 데드락 발생 가능성이 없는 경우에만 자원을 할당
- detection and recovery : 데드락이 발생하면 모든 프로세스를 죽이거나 사이클이 없어질때까지 하나씩 죽임
- ignorance
