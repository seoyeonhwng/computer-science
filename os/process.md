<img src="https://user-images.githubusercontent.com/49056225/116198048-72da0a80-a770-11eb-999d-eb74055c200e.png" width="600" height="400"><br>
- 프로그램(실행 가능한 파일)은 하드디스크 파일 시스템에 실행파일 형태로 저장
- 이 파일을 실행시키면 가상 메모리를 거쳐서 물리적 메모리로 올라감 -> 프로세스(실행 중인 프로그램)가 됨!!
- 프로세스마다 주소 공간(Address space)를 가짐
  - 커널 영역은 컴퓨터를 키면 항상 메인 메모리에 올라가있음 / 사용자 프로그램은 프로그램이 종료되면 해당 프로세스의 주소 공간이 사라짐
  - 주소 공간에서 당장 필요한 부분은 물리적 메모리에, 그렇지 않은 부분은 디스크의 swap area에 저장됨
- file system과 swap area은 둘 다 하드디스크지만 사용 용도가 다름
  - file system : 비휘발성의 용도 (전원이 나가도 파일의 내용이 유지되어야하기 때문에 사용)
  - swap area : 메모리 연장 공간의 용도 
  
### 프로세스
- 프로세스 : 실행 중인 프로그램
- 프로세스의 문맥 (context) : 프로세스가 어느 지점까지 실행했는지 나타냄
  - 프로세스들이 CPU를 번갈아 사용하기 때문에 프로세스가 어느 지점까지 실행되었는지 정확하게 알아야함
  - CPU 수행 상태를 나타내는 하드웨어 문맥 (프로세스가 명령어를 어디까지 실행했는가)
    - Program Counter (PC가 어딜 가리키고 있는가) / 각종 register (레지스터에 어떤 값이 저장되었는가)
  - 프로세스의 주소 공간
    - code, data, stack, heap (프로세스의 주소 공간에 어떤 값이 저장되었는가)

### 프로세스의 상태
<img src="https://user-images.githubusercontent.com/49056225/116199452-1972db00-a772-11eb-860e-a528ff316759.png" width="500" height="300"><br>
- Running : CPU 제어권을 가지고 명령어를 수행 중인 상태 / running 상태의 프로세스는 매 순간 하나!
- Ready : 다른 모든 조건을 만족하고 CPU만 얻으면 명령어를 수행할 수 있는 상태
- waiting(blocked) : CPU를 주어도 당장 명령어를 수행할 수 없는 상태 (ex. I/O 작업 완료를 기다리는 중)
- new : 프로세스가 생성중인 상태
- terminated : 프로세스 수행이 끝난 상태
  
### PCB (Process Control Block)
- 운영체제가 각 프로세스를 관리하기 위해 프로세스에 대한 정보를 저장하는 자료구조
- 프로세스가 한나 생길때마다 운영체제는 커널 주소 공간의 data영역에 PCB를 생성
- 구성 요소
  - Process state / Process ID / scheduling information / priority
  - Program counter / registers (CPU 수행 관련 하드웨어 값)
    
### 문맥 교환 (context switch)
- CPU를 한 프로세스A(스레드)에서 다른 프로세스B(스레드)로 넘겨주는 과정
- 실행 중이던 CPU 레지스터, PC의 값 -> 프로세스A의 PCB에 저장
- 프로세스B의 PCB에서 레지스터, PC의 값 -> CPU에 복원

### 스케쥴러
- long-term scheduler (장기 스케줄러 = job scheduler)
  - 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정 (new -> ready)
  - degree of multiprogramming을 제어 (메모리에 몇 개의 프로그램이 동시에 올라가는지) 
- short-term scheduler (단기 스케줄러 = CPU scheduler)
  - 어떤 프로세스에게 CPU를 줄지 결정 (ready -> running) 
- medium-term scheduler (중기 스케쥴러 = swapper)
  - 어떤 프로세스에게서 메모리를 뺏아가 디스크로 내쫓을지 결정
  - degree of multiprogramming을 제어
   
### 쓰레드
- 프로세스 내에서 CPU 수행 단위
- 프로세스 내에서 공유할 수 있는 것은 최대한 공유하고 CPU 수행과 관련있는 것은 따로 관리
- 구성 요소
  - 쓰레드 ID
  - program counter, registers (스레드는 CPU를 할당받았다가 스케쥴러에 의해 다시 선점 당함 -> 문맥을 저장해야함)
  - stack (독립적인 함수 호출을 위해)
- 다른 thread와 공유하는 부분
  - code / data / heap / 다른 운영체제 자원들 

### multi-thread VS multi-process
- multi-process
  - 여러 개의 프로세스가 작업을 동시에 처리
  - 단점) context switching 비용이 크다 (각각 독립적인 메모리를 가지고 있기 때문) / 프로세스 간 통신을 하려면 IPC
  - 자식 프로세스 중 하나가 문제가 생겨도 다른 프로세스에 영향이 없음 (독립적으로 동작)
- multi-thread
  - 하나의 프로세스 안에서 여러개의 쓰레드가 처리
  - context switching 비용이 적음 (프로세스 자원을 공유하기 때문)
  - 단점) 하나의 스레드가 종료되면 전체 스레드가 종료 / 자원을 공유하는 만큼 충돌 주의 (thread-safe 하게) -> 동기화!!
- multi-thread가 multi-process보다 좋은 이유
  - context switching시 시간과 자원 손실이 적다
  - stack을 제외한 모든 메모리를 공유하기 때문에 자원을 효율적으로 사용 가능
  - 동일한 메모리 공간을 공유하기 때문에 동기화, 데드락 문제 발생 가능

  
