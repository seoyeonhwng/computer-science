### 동시성 제어
- 트랜잭션은 ACID 특징처럼 하나의 트랜잭션이 수행중일때 다른 트랜잭션은 해당 데이터에 접근할 수 없음
- 수많은 트랜잭션을 이렇게 순서대로 처리한다면 DBMS의 성능은 떨어짐
- DBMS의 성능을 높이기 위해 여러 사용자의 쿼리나 프로그램들을 동시에 수행해야함
- 여러 개의 트랜잭션이 충돌없이 동시에 수행될 수 있도록 concurrency control이 필요함
- isolation level을 두어 응용 프로그램별로 최대한 효율적인 locking을 하고자 함
  - 동시성을 높이면 데이터 무결성에 문제가 발생하고, 데이터 무결성을 유지하면 동시성이 떨어짐

### serializable(직렬성)
- serial schedule : 트랜잭션 하나씩 차례대로 수행
- non-serial schedule : 여러 트랜잭션들을 동시에 수행
- serializable : serial schedule과 non-serial schedule의 수행 결과가 같음
  - 여러 트랜잭션을 동시에 실행하여도 차례대로 수행한 것과 결과가 같도록, serializable하게 만들어야함

### 동시성 제어를 하지 않으면 발생할 수 있는 문제
- lost update : 트랜잭션A가 업데이트한 내용을 트랜잭션B가 덮어씀 (무효화)
- dirty read : 완료되지 않은 트랜잭션이 갱신한 데이터를 읽음
- uprepeatable read : 하나의 트랜잭션에서 같은 select문을 수행할때 서로 다른 값을 읽음
- phantom read : 하나의 트랜잭션에서 레코드를 두번 읽었을때, 첫번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현상
  - 트랜잭션 실행 도중 새로운 레코드 삽입을 허용하기 때문에 나타나는 현상
  
### Lock의 종류
- S-lock (Shared Lock) : 트랜잭션에서 READ를 목적으로 데이터에 접근할때 사용 -> 다른 트랜잭션과 공유 가능
- X-lock (Exclusive Lock) : 트랜잭션에서 UPDATE를 목적으로 데이터에 접근할때 사용 -> 다른 트랜잭션과 공유 불가능
- lock -> 데드락 문제 발생 가능!

### Isolation Level
- 트랜잭션에서 일관성 없는 데이터를 허용하는 수준
- SERIALIZABLE
  - 모든 작업을 하나의 트랜잭션에서 처리하는 것과 같음 (가장 높은 고립 수준)
- REPEATABLE READ
  - 한 트랜잭션에서 반복해서 select를 수행하여도 값이 변하지 않음을 보장
  - Phantom Read 발생 가능
- READ COMMITED
  - 다른 트랜잭션에서 commit이 완료된 데이터만 읽을 수 있음
  - non-repeatable read 발생 가능 
- READ UNCOMMITED
  - 다른 트랜잭션에서 commit되지 않은 데이터를 읽을 수 있음
  - dirty read 발생 가능
