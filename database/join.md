### 논리적 조인
- INNER JOIN : A와 B의 교집합
- LEFT OUTER JOIN : A의 모든 데이터와 B와 매칭이 되는 레코드를 포함
- RIGHT OUTER JOIN : B의 모든 데이터와 A와 매칭이 되는 레코드를 포함
- FULL OUTER JOIN : A와 B의 합집합
- CROSS JOIN : A의 각 행과 B의 각 행을 다 조합한 결과 (cartesin product)
- SELF JOIN : 자기 자신과 조인

### 물리적 조인
- 릴레이션의 조인을 계산하는 알고리즘
- 두 릴레이션 r과 s를 a라는 컬럼으로 조인하는 경우 내부적으로 조인 결과를 어떻게 얻는가

### nested-loop join
```
for each tuple Tr in r do begin
  for each tupel Ts in s do begin
    test pair (Tr, Ts) to see if they satisfy the join condition
    if they do, add (Tr, Ts) to the result;
  end
end
```
- 이중 for문 / r은 outer relation, s는 inner relation
- 두 릴레이션의 모든 튜플 쌍을 검사하기 때문에 비용이 많이 듬
- 대량의 테이블이거나 inner relation에 인덱스가 생성되어있지 않은 경우 비효율적
- 소량의 데이터를 가진 테이블이 outer relation으로 설정하는 것이 성능이 유리함

### sort-merge join
- nested-loop join처럼 이중 for문으로 동작
- outer relation과 inner relation을 각각 join 컬럼을 기준으로 정렬 후 join하는 방식
- inner table에 인덱스가 없는 경우 nested-loop join 대신 사용 가능

### hash join
```
for each tupe Tr in Hri do begin
  for each matching tupe Ts in Hsi do begin
    add (Tr, Ts) to the result;
  end
end
```
- 아이디어) 두 튜플이 조인 조건을 만족한다면 두 튜플은 동일한 해시 값을 가짐
- outer/inner relation의 조언 컬럼에 대해 각각 동일한 해시 값을 가지는 집합(파티션)으로 분할
- 각 파티션에 대해서 nested loop join처럼 이중 for문으로 조인
- 대용량의 데이터를 조인할 때 적절함
