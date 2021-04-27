### logical address VS physical address
- logical address (virtual address)
  - 프로세스마다 독립적으로 가지는 주소 공간 / 각 프로세스마다 0번지부터 시작
  - CPU가 보는 주소는 logical address
- physical address
  - 메인 메모리에 올라가는 실제 위치
  - 아래 주소에는 커널, 상위 주소에는 여러 프로그램들이 섞여서 올라가있음

### MMU(Memory-Management Unit)
- logical address를 physical address로 맵핑해주는 하드웨어
- 사용자 프로그램, CPU 둘다 logical address를 사용하고 MMU가 그때그때 주소 변환을 해줌
- logical address + base register = physical address
  - base register : 물리적 메모리에서 해당 프로세스의 시작 위치를 저장
  - limit register : 프로그램의 크기를 저장 (논리적 주소의 범위를 결정)

### swapping
- 한정된 메모리를 관리하기 위해 사용하는 기법
- 프로세스 전체를 메모리에서 디스크로, 디스크에서 메모리로 내쫓는 것 (swap in/swap out)
- 중기 스케쥴러에 의해 swap out 시킬 프로세스를 선정

### fragmentation (단편화)
- 메모리에 프로세스들을 적재하고 제거하다보면 메모리에 생기는 작은 공간들
- 외부 단편화 (external fragmentation) : 메모리 공간 중 사용하지 못하는 부분
- 내부 단편화 (internal fragmentation) : 파티션 내부에서 사용되지 않는 부분

### contiguous allocation
- 각 프로그램를 메모리에 연속적인 공간에 적재하는 방식 (프로세스 통째로!)
- 고정 분할 방식(fixed partition)
  - 물리적 메모리를 몇 개의 파티션으로 나누고 각 파티션에 하나의 프로그램를 적재
  - 외부 단편화 / 내부 단편화 발생
- 가변 분할 방식(variable partition allocation)
  - 프로그램이 실행될때마다 프로그램의 크기를 고려해서 비어있는 hole (가용 메모리 공간)에 할당
  - 파티션의 크기와 개수는 동적으로 변함
  - 외부 단편화 발생

### noncontiguous allocation
- 하나의 프로세스를 잘라서 메모리에 산발적으로 적재
- paging
  - 프로그램의 주소 공간 (논리적 메모리)를 동일한 사이즈의 page 단위로 분할 / 물리적 메모리도 frame 단위로 분할
  - page table : 각 page가 어떤 frame에 올라가있는지 저장 ex) 0번 페이지 - 1번 프레임
  - 내부 단편화 발생
- segmentation
  - 프로그램의 주소 공간 (논리적 메모리)를 의미 단위로 분할 / 서로 다른 크기
  - 외부 단편화 발생

### Demand paging
- 대부분의 시스템은 페이징 기법을 사용 -> 실제 필요할 때 해당 Page를 메모리에 적재하자
- 필요한 양만 올리기 때문에 더 많은 프로그램을 동시에 실행시킬 수 있음
- page table의 valid/invalid bit 사용
  - invalid : 해당 페이지가 물리적 메모리에 없는 경우, 사용되지 않는 주소 영역인 경우
  - 주소 변환시 valid bit = 0 -> 요청한 페이지가 메인 메모리에 없음 = page fault

### page fault
- 인터럽트를 통해 디스크에 접근하여 해당 페이지를 메인 메모리로 읽어옴 
- 오래 걸리는 작업 -> page fault rate을 최소화하자
- replacement algorithm
  - 물리적 메모리에서 내쫓을 frame을 선정하는 알고리즘
  - FIFO, LRU, LFU ...

### Thrashing
- 프로세스의 원활한 수행을 위해서는 최소한의 page가 메인 메모리에 적재되어 있어야함
- 최소한의 page frame을 할당 받지 못하는 경우 프로세스가 수행되는 시간보다 페이지 교체 시간이 더 많아지는 현상
- page fault rate이 매우  높아짐 / CPU utilization이 낮아짐 (요청한 페이지를 계속 I/O하기 때문)
- 해결방안) degree of multiprogramming을 조절 (동시에 너무 많은 프로그램을 실행하지 않음) -> working set

### working set
- locality에 기반하여 프로세스가 일정 시간동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야하는 페이지들의 집합
- working set 전체가 메모리에 올라오고 그렇지 않으면 모든 frame을 반납
- degree of multiprogramming을 조절하고 thrashing을 방지할 수 있음

