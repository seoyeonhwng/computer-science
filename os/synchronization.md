- 공유 자원 -> 동기화 문제 발생 -> lock으로 해결 -> 데드락 발생 가능

### Process synchronization
- 멀티 프로세스(스레드) 환경에서 공유 데이터를 동시에 접근하는 경우 데이터 불일치가 발생할 수 있음
- ex) system call을 하는 동안 커널 주소공간의 데이터를 접근 + 이 작업 중간에 CPU를 선점하면 race condition 발생
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

---
### 데드락
- 프로세스가 자원을 얻지 못해서 다음 처리를 하지 못하고 무한 대기하는 상태
- 자원 : 하드웨어, 소프트웨어를 포함하는 개념 ex) I/O device, CPU cycle, memory space, semaphore
- 프로세스가 자원을 사용하는 절차 : request -> allocate -> use -> release

### 데드락 발생 조건 (4가지 조건이 동시에 성립할때 발생)
- 4가지 조건을 다 만족해야 데드락이 발생함
- mutual exclusion : 자원은 하나의 프로세스만 사용할 수 있음
- hold and wait : 본인이 가진 자원은 내어놓지 않고 다른 자원을 기다림
- No preemption : 프로세스는 사용중인 자원을 빼앗기지 않음
- circular wait : 자원을 기다리는 프로세스들 간에 사이클이 형성됨

### 데드락 해결 방법
- prevention : 데드락이 생기지 않도록 미연에 방지 (강한 처리)
  - 자원 할당시 데드락 발생 조건 중 하나라도 만족되지 않도록 함
- avoidance : 데드락이 생기지 않도록 미연에 방지
  - 자원 요청에 대한 부가적인 정보를 이용해서 데드락의 가능성이 없는 경우에만 자원 할당 
- detection and recovery : 데드락 발생을 허용하고 데드락 발견시 처리
  - reovery : 데드락에 연루된 프로세스들을 한번에 죽임 / 데드락 사이클이 없어질 때까지 프로세스를 한번에 하나씩 죽임 
- ignorance : 데드락이 발생해도 아무런 처리하지않고 무시함

### 데드락 prevention
- 데드락 발생 조건 4가지 중에서 하나를 없애서 데드락 발생을 방지
- 데드락을 원천적으로 막을 수 있지만 자원 이용률이 저하됨 
- mutual exlusion : 배제할 수 있는 조건이 아님
- hold and wait : 자원을 요청하는 상태에서는 자원을 보유하고 있지 않음
  - 방법1) 시작시 모든 필요한 자원을 프로세스에게 할당 -> 비효율적
  - 방법2) 자원이 필요한 경우 보유 자원을 모두 내어놓고 다시 요청
- no preemption : 자원이 필요할때 빼앗을 수 있게 함
- circular wait : 사이클이 생기지 않도록 함
  - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로 자원을 할당 ex) 낮은 순서 자원 먼저 할당

### 데드락 avoidance
- 프로세스들이 필요한 각 자원별 최대 사용량을 미리 선언
- 이 정보를 바탕으로 프로세스가 자원 요청을 했을때 데드락 발생 가능성이 있으면 자원이 있어도 할당하지 않음
- 데드락 발생 가능성이 없는 경우만 자원을 할당
  - safe state : 시스템 내의 프로세스들에 대한 safe sequence가 존재하는 상태
  - 현재 가용 자원과 다른 프로세스들이 반환한 자원으로 각 프로세스의 최대 요청을 처리할 수 있는 순서가 존재하는지 확인
  - 시스템이 safe state라면 데드락 발생 가능성이 없음!!
  - 즉, avoidance는 시스템이 unsafe state가 되지 않는ㄴ 것을 보장함
- banker's alogrithm
  - 프로세스가 자원을 요청했을때 그 요청을 받아들일 것인지 아닌지 결정
  - 프로세스가 최대 요청을 할 것이라 가정하고 그 요청이 현재 가용 자원으로 가능한지 확인  
