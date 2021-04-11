### OSI 7 layer
- 네트워크에서 일어나는 통신 과정을 7단계로 나누어 정한 표준
- 계층화 + 계층 간의 독립성 -> 유지보수가 쉬움 (문제가 생긴 계층만 고치면 되기 때문)
- Application Layer
  - 유저에게 제공되는 애플리케이션에서 사용하는 통신의 움직임을 결정
  - 프로토콜 : HTTP, DNS, FTP
- Transport Layer
  - 2대의 컴퓨터 사이의 데이터 흐름을 제공
  - 프로토콜 : TCP, UDP
- Network Layer
  - 네트워크 상에서 패킷의 이동을 다룸 (패킷을 어느 방향으로 보낼지 정하고 해당 방향으로 전송)
  - 어떤 경로를 거쳐 상대의 컴퓨터까지 패킷을 보낼지
  - 프로토콜 : IP, ARP
- Link Layer
  - 물리적으로 연결된 노드(hop)과 노드(hop) 사이의 데이터 전송
  - 프로토콜 : Ethernet, WIFI
- Physical Layer

### 캡슐화

### URI vs URL
- URI는 Uniform Resource Identifier = 리소스를 식별하는 식별자
- URL은 Uniform Resource Locator = 리소스의 네트워크상 위치
- URI는 리소스를 식별하기 위한 문자열 전반을 나타내고 URL은 리소스의 위치를 나타냄 (URI가 포괄적인 개념)
