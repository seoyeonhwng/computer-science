### DNS (Domain Name Server)
- 도메인 이름을 IP주소로 변환하는 시스템
- 인터넷에서 통신을 하려면 IP주소를 알아야함 -> 모든 IP주소를 외울 수 없기 때문에 문자(도메인)를 사용
- DNS를 여러 서버로 나누고 계층 구조로 구성 (분산 계층 데이터베이스)

### DNS 서버의 종류
- DNS recursor : iterative query를 받는 서버 
- root name server : 최상위 서버
- TLD name server : 도메인 확장자를 공유하는 모든 도메인 이름의 정보를 유지하는 서버
  - ex) .com TLD 네임서버는 '.com'으로 끝나는 모든 웹사이트의 정보를 알고있음
- authoritative name server : 실제로 DNS 리소스 레코드를 보유하고 담당하는 서버
  - DNS records : (name, value, type, ttl)

### DNS 동작 과정
- 먼저 Local DNS(컴퓨터에 미리 설정되어 있는 DNS)에 해당 도메인에 대한 IP주소가 있는지 확인
- Local DNS에 없다면 여러 DNS 서버에게 물어봐서 IP주소를 알아냄 (Recursive query)
<img src="https://user-images.githubusercontent.com/49056225/114833433-8a2d0580-9e0a-11eb-8504-3447a0587b9e.png" width="600" height="400">

### DNS는 UDP를 사용
- DNS는 HTTP request를 위한 준비 동작이므로 패킷 유실 가능성이 있어도 속도가 빠른 UDP 선택
- HTTP에 비해 DNS에서 주고받는 패킷의 크기는 매우 작으므로 패킷 유실되어도 손상이 작기 때문
