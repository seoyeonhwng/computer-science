### 네트워크 구성 요소
- 모든 복잡한 일은 edge에서 하고 단순 노동은 core가 하는 구조
1. network edge : application, hosts
2. network core : 서로 연결된 라우터들
3. links : 구성 요소들을 연결하는 링크

### 데이터 전송 방식
- 네트워크에서는 데이터가 어떤 방식으로 전달되는가?
- circuit switching
  - 출발지에서 목적지까지 가는 길에 필요한 자원들을 미리 예약해두고 전달
  - 자원을 미리 예약하기 때문에 네트워크를 사용할 수 있는 인원이 한정되어있음
- packet switching
  - 데이터를 패킷 단위로 그때 그때 올바른 방향으로 전달 (전송하는 동안만 자원을 사용)
  - 인터넷에서는 이 방식을 사용하여 데이터를 전송함
  - 자원을 미리 예약하지 않기 때문에 더 많은 사용자들이 네트워크를 사용할 수 있기 때문

### packet delay
<img src="https://user-images.githubusercontent.com/49056225/114342653-2b138a80-9b97-11eb-8152-5a4f0c7cc04c.png" width="500" height="250"><br>
- packet switching 방식에서는 delay가 발생함
- nodal processing : 받은 패킷 error check + output link 확인
- queueing : output link에서 전송을 위해 큐에서 대기하는 시간 (큐가 꽉차면 패킷은 유실됨)
- transmission delay : 패킷의 첫번째비트부터 마지막 비트까지 링크에 올라오는 시간
- propagation delay : 마지막 비트가 다음 라우터까지 전송되는 시간

### processes communicating
<img src="https://user-images.githubusercontent.com/49056225/114344192-5481e580-9b9a-11eb-81fa-790746ff6601.png" width="600" height="300"><br>
- 통신이란 클라이언트 프로세스와 서버 프로세스 사이의 통신
- 프로세스들은 **소켓**이라는 인터페이스를 통해서 메시지를 주고 받음
  - 소켓 = 프로세스간 통신을 위해 OS가 제공하는 API
  - 소켓 주소 = IP 주소 (어떤 컴퓨터인지) + port 번호 (어떤 프로세스인지)
  - transport layer로 TCP를 사용하면 TCP소켓, UDP를 사용하면 UDP소켓을 사용

### OSI 7 layer
- 네트워크에서 일어나는 통신 과정을 7단계로 나누어 정한 표준
- 통신이 일어나는 과정을 단계별로 파악할 수 있고 사람이 이해하기 쉬움
- 계층화 + 계층 간의 독립성 -> 유지 보수가 쉬움 (문제가 생긴 계층만 고치면 되기 때문)
- Physical Layer
  - 통신 케이블로 데이터를 전송 / 전송 단위 : 비트 
- Link Layer
  - 물리적으로 연결된 노드(hop)과 노드(hop) 사이의 데이터 전송 / 전송 단위 : 프레임
  - 프로토콜 : Ethernet, WIFI
- Network Layer
  - 네트워크 상에서 패킷의 이동을 다룸 (패킷을 어느 방향으로 보낼지 정하고 해당 방향으로 전송)
  - 어떤 경로를 거쳐 상대의 컴퓨터까지 패킷을 보낼지 / 전송 단위 : 패킷
  - 프로토콜 : IP, ARP
- Transport Layer
  - 프로세스간 데이터 흐름을 제공 / 전송 단위 : 세그먼트, 데이터그램
  - 프로토콜 : TCP, UDP
- Session Layer
  - 컴퓨터끼리 통신하기 위해 세션을 만드는 계층
- Presentation Layer
  - 데이터의 형식을 정의하는 계층 
- Application Layer
  - 유저에게 제공되는 애플리케이션에서 사용하는 통신의 움직임을 결정 / 전송 단위 : 메시지
  - 프로토콜 : HTTP, DNS, FTP

### 캡슐화 / 역캡슐화
- 캡슐화 (encapsulation)
  - 상위 계층에서 받은 데이터에 필요한 정보를 헤더에 담아서 하위 계층으로 전송하는 기술
- 역캡슐화 (decapsulation)
  - 하위 계층에서 받은 데이터에서 헤더를 제거하여 상위 계층으로 전송하는 기술
- 장점) 각 계층에서 수행한 정보를 헤더에 표시함으로서 필요하지 않는 부분은 숨길 수 있음


### URI vs URL
- URI는 Uniform Resource Identifier = 리소스를 식별하는 식별자
- URL은 Uniform Resource Locator = 리소스의 네트워크상 위치
- URI는 리소스를 식별하기 위한 문자열 전반을 나타내고 URL은 리소스의 위치를 나타냄 (URI가 포괄적인 개념)
- URL의 구조
  - (프로토콜) :// (도메인 이름) : (포트번호) / (웹서버에서 리소스의 경로) ? (웹서버로의 자원)
  - 프로토콜 = 브라우저가 어떤 프로토콜을 사용하는지 명시 ex) HTTP, HTTPS, FTP
  - 도메인 이름 = 어떤 웹서버인지 명시 / DNS를 통해 해당 웹 서버의 IP주소를 알아낼 수 있음
  - 포트번호 = 웹서버의 어떤 프로세스인지 명시 / 요 포트번호를 통해 TCP는 connection을 맺어 process간 통신으로 바꿔줌

### REST API
- REST라는 아키텍쳐에 기반하여 구현한 API
  - REST : 설계의 중심에는 자원이 있고, HTTP 메소드로 자원의 행위를 명시하도록 설계된 아키텍쳐
  - API : 해당 프로그램의 구현을 알지 않아도 프로그램이 제공하는 기능을 쓸 수 있도록 한 인터페이스
- HTTP URI를 통해 자원을 명시 + HTTP method를 통해 자원의 행위를 명시 + JSON이나 XML의 형태로 자원을 주고 받는 형태로 API
- 장점
  - HTTP의 인프라를 그대로 사용하기 때문에 REST API 사용을 위해 별도의 인프라를 구축할 필요가 없음 
  - 메시지가 의도하는 바를 명확하게 파악 가능
- 단점
  - HTTP 메소드(GET, POST, DELETE, PUT)가 제한적
