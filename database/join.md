### 논리적 조인
- INNER JOIN : A와 B의 교집합
- LEFT OUTER JOIN : A의 모든 데이터와 B와 매칭이 되는 레코드를 포함
- RIGHT OUTER JOIN : B의 모든 데이터와 A와 매칭이 되는 레코드를 포함
- FULL OUTER JOIN : A와 B의 합집합
- CROSS JOIN : A의 각 행과 B의 각 행을 다 조합한 결과 (cartesin product)
- SELF JOIN : 자기 자신과 조인

### 물리적 조인
- 릴레이션의 조인을 계산하는 알고리즘
- 쿼리에 조인이 포함되면 옵티마이저는 테이블 간의 조인 작업을 수행하여 결과를 얻음 (두 릴레이션 r과 s를 a라는 컬럼으로 조인)
- 조인 방법 : nested loop join, sort merge join, hash join

### nested-loop join
```
for each tuple Tr in r do begin
  for each tupel Ts in s do begin
    test pair (Tr, Ts) to see if they satisfy the join condition
    if they do, add (Tr, Ts) to the result;
  end
end
```
- 이중 for문 / r은 outer table, s는 inner table
- outer table의 row에 하나씩 접근하여 일치하는 값을 inner table에서 순차적으로 찾음
- 두 테이블의 모든 튜플 쌍을 검사하기 때문에 비용이 많이 듬 -> 좁은 범위에 유리한 성능
- 어떤 테이블을 outer table로 선택하는지가 성능을 결정
  - 크기가 작은 테이블을 outer table로 해야함
  - outer table의 행 개수만큼 join이 수행되기 때문
- inner table에 인덱스가 생성되어 있지 않다면 비효율적
- 조인 조건에 동등 연산자(=)를 사용하는 경우 옵티마이저는 NLJ를 선택


### sort-merge join
- 두 테이블을 각각 join 컬럼을 기준으로 정렬 -> 두 테이블을 동시에 스캔하면서 조인 조건에 맞는 건들을 찾아 merge함
- 많은 양의 데이터를 처리 or 적당한 인덱스가 없는 경우 NLJ보다 성능에 유리함
- 단점) sort할때 시간이 오래 걸림
- 조인 조건에 비동등 연산자(<,>)를 사용하는 경우 옵티마이저는 SMJ를 선택

### hash join
```
for each tuple Tr in Hri do begin
  for each matching tuple Ts in Hsi do begin
    add (Tr, Ts) to the result;
  end
end
```
- 아이디어) 두 튜플이 조인 조건을 만족한다면 두 튜플은 동일한 해시 값을 가짐
- 두 테이블 중 row 수가 적은 테이블을 대상으로 해시 테이블 생성
- 나머지 테이블의 row에 하나씩 접근하여 해시값을 생성 -> 해당 해시 테이블에서 일치하는 값을 찾음
- sort에 대한 부담을 줄이기 위한 방안
- 조인 조건에 동등 연사자를 사용한 경우에만 사용 가능
