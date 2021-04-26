### 인덱스
- 데이터베이스에서 원하는 데이터를 빠르게 검색하기 위해서 사용
- 데이터 파일과 인덱스 파일을 따로, 부가적으로 관리
- 인덱스 파일의 레코드 : <search key 값, 레코드들을 포함한 블록에 대한 포인터> 

### B+-tree
- 인덱스 파일을 저장하는 자료구조
- 루트부터 단말 노드까지 모든 경로의 길이가 같은 balanced tree
- 단말 노드 : search key값과 포인터를 저장 (이러한 단말 노드들이 정렬된 순서로 저장)
- 비단말 노드 : 단말노드를 향한 포인터

### 순서 인덱스
- 인덱스 파일이 특정 컬럼 (search key)값을 기준으로 정렬된 순서로 저장
- 인덱스 파일의 레코드는 <search key 값, 레코드들을 포함한 블록에 대한 포인터> 
- clustering index (= primary index)
  - search key가 primary key인 경우
- nonclustering index (= secondary index)
