- 내가 다루려는 데이터에 대해 파악하고 RDB VS NoSQL을 결정해야함

### RDB (Relational DataBase)
- 테이블 간의 관계를 정의 / 스키마 정의 / 수직 확장 / 트랜잭션, 정규화
- 장점 : 데이터 무결성 보장 (ACID) -> 안정성을 추구
- 단점 : 작성된 스키마 수정이 어려움
- 용도 : 데이터의 안전성이 중요한 경우, 정형화된 데이터를 다루는 경우, 명확한 스키마를 요구하며 스키마의 구조가 변경되지 않는 경우

### NoSQL
- BASE = Basically Available Soft-state Eventually consistency
- 테이블 간의 관계를 정의하지 않음 / 유연한 데이터 저장 (스키마 X) / 수평 확장 / 트랜잭션 ACID 미보장
- 내부적으로 분산 기술을 사용하여 구현 (데이터를 여러 대의 서버에 분산하여 저장)
- 단일 인스턴스의 한계를 넘어 수평 확장을 해야 하는 대용량 데이터 저장에 용이 -> 대용량의 데이터 저장에 용이 
- 장점 : 데이터 모델링이 유연함 / 확장성 용이
- 단점 : 데이터의 일관성이 약함
- 용도 : 정확한 데이터 구조를 알 수 없거나 변경/확장 될 수 있는 경우, 다량의 데이터를 한번에 처리하는 경우 ex) 트위터

### NoSQL 저장 방식에 따른 모델
- key-value Model (ex. Redis, DynamoDB)
  - 키 하나로 데이터를 저장, 조회할 수 있는 단일 키-값 구조 / 단순한 구조
  - 복잡한 조회 연산 불가능
- document Model (ex. MongoDB)
  - key-value의 확장된 형태로 value에 document(구조화된 문서 데이터, json)을 저장하고 조회
  - 복잡한 데이터 구조 표현 가능
  - sorting, join, grouping 연산 가능
