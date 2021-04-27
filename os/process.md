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

### 프로세스의 상태
<img src="https://user-images.githubusercontent.com/49056225/116199452-1972db00-a772-11eb-860e-a528ff316759.png" width="500" height="300">
- Running : CPU 제어권을 가지고 명령어를 수행 중인 상태 / running 상태의 프로세스는 매 순간 하나!
- Ready : 다른 모든 조건을 만족하고 CPU만 얻으면 명령어를 수행할 수 있는 상태
- waiting(blocked) : CPU를 주어도 당장 명령어를 수행할 수 없는 상태 (ex. I/O 작업 완료를 기다리는 중)
- new : 프로세스가 생성중인 상태
- terminated : 프로세스 수행이 끝난 상태
  
### PCB (Process Control Block)
- 운영체제가 각 프로세스를 관리하기 위해 프로세스에 대한 정보를 저장하는 자료구조
- 커널 주소 공간의 data 영역에 프로세스마다 PCB가 존재 / 프로세스 생성과 동시에 PCB를 생성
- 구성 요소
  - Process state / Process ID / scheduling information / priority
  - Program counter / registers (CPU 수행 관련 하드웨어 값)
    
### 문맥 교환 (context switch)
- CPU를 한 프로세스A(스레드)에서 다른 프로세스B(스레드)로 넘겨주는 과정
- 실행 중이던 CPU 레지스터, PC의 값 -> 프로세스A의 PCB에 저장
- 프로세스B의 PCB에서 레지스터, PC의 값 -> CPU에 복원

---
### 쓰레드

  
