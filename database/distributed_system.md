### 분산 시스템 (Distributed System)
- 웹 어플리케이션의 크기가 커지면서 요청이 많아짐 -> DB 1대로 처리 불가능
- DB를 여러 대 만들고 요청을 나눠서 처리하자 = 분산 시스템!
- 분산 시스템을 관리(배포 + 유지 + 디버깅)하는 것은 복잡하지만 수평 확장을 할 수 있기 때문에 사용
- 수평 확장의 장점
  - easy scaling : 기계 1대를 추가하면 되기 때문에 성능 향상이 쉽고 한계가 없음
  - fault tolerance : 하나가 고장이 나도 여러 대가 존재하기 때문에 서비스가 중단되지 않음
  - low laterency (낮은 지연시간) : 물리적으로 가장 가까운 노드가 요청을 처리 -> 지연시간 감소 
  
### Replication
<img src="https://user-images.githubusercontent.com/49056225/116812039-82e65580-ab87-11eb-8a35-10f56f6cfd18.png" width="600" height="500"><br>
- DB를 Master/Slave로 나눠서 동일한 데이터를 저장하는 방식 / 수직적 구조
- Master DB는 쓰기 작업만 / Slave DB는 읽기 작업만 처리
- 수정사항을 master DB가 slave DB에게 비동기로 알려줌 -> 수정사항이 즉시 반영되지 않아 일관성있는 데이터를 얻지 못할 수 있음
- 장점
  - DB 요청의 대부분이 읽기 작업이기 때문에 Replication만으로도 성능을 높일 수 있음
  - 비동기 방식으로 운영되어 지연 시간이 거의 없음
- 단점
  - 노드들 간의 데이터 동기화가 보장되지 않아 일관성있는 데이터를 얻지 못할 수 있음
  - Master 노드가 다운되면 복구 및 대처가 까다로움
  - 쓰기 작업만 엄청 많다면 성능을 높일 수 없음!!
  
### Partitioning
- 하나의 테이블이 너무 커졌으니 쪼개자!
- 모든 노드가 같은 스키마를 가지고 있지만 각각 데이터를 일부분씩 나눠가지고있음 ex) x테이블의 일부분은 A디비, x테이블의 일부분은 B디비
- 파티셔닝의 종류 (테이블을 어떻게 나누느냐)
  - 수평 단편화 (horizontal partitioning = Sharing)
    - 테이블을 수평으로 분할해서 나눠서 저장 
  - 수직 단편화 (vertical partitioning)
    - 컬럼을 나눠 새로운 테이블로 나눠서 저장 (정규화처럼)
    - 특정 컬럼이 빈번하게 참조될 때 사용
    - 한 row의 크기가 작아지면서 해당 컬럼을 저장한 여러개의 row가 캐시에 올라갈 수 있음!!

### Sharding (horiziontal partitioning)
<img src="https://user-images.githubusercontent.com/49056225/116812140-29325b00-ab88-11eb-810a-125b646e8c86.png" width="600" height="500"><br>
- 서버를 여러 개의 작은 서버(=shard)로 분할 / shard에는 모두 다른 레코드를 저장함
- 어떤 레코드를 어떤 샤드에 저장하는지 rule을 정해야함 -> 데이터를 균일하게 분산할 수 있어야 좋은 rule!
- sharding의 여러 가지 방법 (샤딩 전략)
  - hash sharding (modular sharding)
    - shard key를 해싱/모듈러 연산한 결과로 어느 샤드로 갈지 결정
    - range sharding에 비해 데이터가 균일하게 분산 / DB를 증설하면 이미 적재된 데이터의 재정렬이 필요함
    - 데이터양이 일정수준에서 유지되는 서비스에 적합
  - range sharding
    - shard key 값의 범위에 따라 어느 샤드로 갈지 결정
    - DB를 증설해도 재정렬이 필요하지 않음 / 일부 DB에 데이터가 몰릴 수 있음
- 샤딩은 프로그래밍/운영적 복잡도가 높아지기 때문에 그전에 다른 해결방안을 충분히 고민해야함
  - 어떤 연산이 자주 사용되는지/언제 어떻게 트래픽이 몰리는지 먼저 파악해야함!!! 
  - scale-in은 어떨까?
  - Read 연산이 많다면 cache(Redis)나 replication는 어떨까?
  - 일부 컬럼만 자주 사용된다면 vertical partitioning는 어떨까?

### decentralized VS distributed
- decentralized system은 하나에 의해 통제되지 않음
- 즉, 우리가 원하는 시스템은 distributed centralized system!

---
### Clustering
<img src="https://user-images.githubusercontent.com/49056225/116812592-6f88b980-ab8a-11eb-9055-a102dfa673a4.png" width="600" height="500"><br>
- 하나의 DB를 여러개의 서버상에 구축하는 방식 / 수평적인 구조
- Fail over 시스템을 구축하기 위해 사용 (서버 하나가 고장나면 나머지가 대신 처리)
- 동기화 방식 -> 데이터 누락이 발생하지 않음
- 장점 
  - 노드들 간의 데이터를 동기화하여 항상 일관성있는 데이터를 얻을 수 있음
  - 1개의 노드가 죽어도 다른 노드가 살아있기 때문에 시스템을 계속 운영할 수 있음 (장애X)
- 단점
  - 여러 노드들 간의 데이터를 동기화하는 시간이 필요하므로 Replication에 비해 쓰기 성능이 떨어짐
  
