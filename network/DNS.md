### DNS (Domain Name System)
- 도메인 이름을 IP주소로 변환하는 분산형 데이터베이스 시스템
- DNS는 Domain Name Space를 저장하고 관리
- Domain Name Space는 최상위에 root 네임서버가 존재하고 그 밑에 각 DNS 서버들이 하위 도메인에 대한 정보를 관리 -> 계층적 구조
- 왜) 인터넷에서 통신을 하려면 IP주소를 알아야함 -> 모든 IP주소를 외울 수 없기 때문에 문자(도메인)를 사용
- DNS는 Application layer protocol
  - DNS의 쿼리를 application layer에서만 이해할 수 있기 때문 

### DNS 서버의 종류
- DNS resolver : iterative query를 받는 서버 / 요청받은 도메인 이름에 대한 IP정보를 다시 리졸버에게 전달
- root name server : 최상위 서버
- TLD name server : 도메인 확장자를 공유하는 모든 도메인 이름의 정보를 유지하는 서버
  - ex) .com TLD 네임서버는 '.com'으로 끝나는 모든 웹사이트의 정보를 알고있음
- authoritative name server : 실제로 DNS 리소스 레코드를 보유하고 담당하는 서버
  - DNS records : (name, value, type, ttl)

### DNS 동작 과정
- 먼저 Local DNS(컴퓨터에 미리 설정되어 있는 DNS)에 해당 도메인에 대한 IP주소가 있는지 확인
- Local DNS에 없다면 Local DNS는 다른 DNS 서버들과 통신 시작 / 다른 DNS 서버에게 DNS query를 전송함
- Recursive query : Local DNS가 (root 네임 서버 -> .com 네임 서버 -> example.com 네임 서버)에게 물어봐서 IP주소를 찾는 과정
<img src="https://user-images.githubusercontent.com/49056225/114837760-04f81f80-9e0f-11eb-805e-20d18111d89c.png" width="600" height="400">

### DNS는 UDP를 사용
- DNS는 HTTP request를 위한 준비 동작이므로 패킷 유실 가능성이 있어도 속도가 빠른 UDP 선택
- HTTP에 비해 DNS에서 주고받는 패킷의 크기는 매우 작으므로 패킷 유실되어도 손상이 작기 때문
