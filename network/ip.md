### 네트워크 계층
- sender에서 라우터를 거쳐 receiver까지 각 패킷의 전달을 담당 (데이터 전송)
- 패킷을 어떤 경로로 목적지까지 전달할 것인가 -> 포워딩과 라우팅으로 경로를 결정
- 라우터는 패킷을 목적지까지 전달하는 일을 하기 때문에 네트워크 계층까지 존재
  - 라우터에는 end-to-end connection 개념없음 -> 그저 dst IP주소를 보고 적절한 경로로 패킷을 전달!
- 프로토콜 : IP, ICMP

### 라우터 vs 스위치
- 라우터 : 수신한 패킷의 정보를 통해 경로를 설정하여 패킷을 전송
- 스위치 : 내부 네트워크에서 MAC주소를 이용하여 해당 프레임을 

### IP (Internet Protocol)
- 네트워크 계층의 데이터 전송 프로토콜 
- best-effort 전송 (전송한 패킷의 도착을 보장하지 않음 = unreliable)
- 비연결형 / 패킷 분할(fragmentation) 지원

### IP 패킷의 헤더
<img src="https://user-images.githubusercontent.com/49056225/115352218-1320b400-a1f2-11eb-816c-971748caabd4.png" width="500" height="400"><br>
- source와 destination의 IP 주소
- fragmentation 관련 헤더
    - identifier : 어떤 패킷의 fragment인지 나타냄 (같은 패킷이라면 해당 필드의 값 동일)
    - flags : 뒤에 fragment가 더 있는지 여부 (마지막 조각이라면 해당 필드의 값은 0)
    - fragment offset : fragment의 데이터 부분에 저장된 첫번째 바이트
- header checksum : 헤더 필드에 대한 error detection (데이터 아님!)
- upper layer : 상위 계층의 프로토콜 종류 (TCP인지 UDP인지)
- TTL : 남은 hop의 개수 (패킷이 라우터를 지날때마다 1씩 감소) / 한정된 시간동안만 네트워크에 존재하도록 하기 위함

### 포워딩 / 라우팅
- 네트워크 계층의 핵심 기능 = 포워딩과 라우팅
- 포워딩 (forwarding)
  - 라우터의 forwarding table을 보고 특정 input으로 들어온 패킷을 적절한 output으로 내보내는 일
  - 패킷의 dst IP주소를 보고 longest prefix matching 방식으로 output link를 결정
- 라우팅 (routing)
  - forwarding table을 채우는 작업
  - routing 알고리즘을 통해 패킷의 최소 비용 경로를 결정
    - link state 알고리즘 : 모든 라우터가 topology와 link cost를 알기 때문에 Dijkstra로 해결
    - distance vector 알고리즘 : 각 라우터는 자신과 연결된 라우터의 link cost만 알기 때문에 bellman-Ford로 해결
      - 각 노드는 자신의 distance vector를 이웃에게 전송
      - 이웃으로부터 새로운 distance vector를 받으면 자신의 distance vector를 새로 계산

### Fragmentation
- 라우터에서 다른 라우터로 패킷을 전송할때 연결된 링크마다 MTU(Maximum Transmission Unit)이 존재
- 패킷이 MTU보다 크다면 해당 패킷을 잘라서 전송 (fragmentation)
- receiver가 IP 헤더 (identifier, flags, fragment offset)를 보고 잘라진 패킷들을 재조립
  
### IP Address (IPv4)
- 32bit / 호스트나 라우터의 네트워크 인터페이스를 지칭
- IP주소는 network와 host 부분으로 나눠져있음 (hierarchical addressing)
  - IP주소 = network ID (=subnet ID = prefix) + host ID
  - 계층적으로 IP주소를 할당함으로써 forwarding이 간단해지고, 새로운 호스트 추가도 쉬움
- IP주소에서 어디까지 network ID인지 나타내는 방법
  - subnet mask 표기법 : 12.34.158.5 (255.255.255.0)
  - CIDR 표기법 : 12.34.158.5/24
  
### 서브넷
<img src="https://user-images.githubusercontent.com/49056225/115199843-92e54a80-a12e-11eb-9e27-faa4b86eb86b.png" width="400" height="400"><br>
- 같은 network ID를 가진 네트워크 인터페이스들의 집합
- 라우터를 거치지 않고 서로 접근 가능한 집합 -> 라우터는 서브넷들의 교집합

### NAT (Network Address Translation)
<img src="https://user-images.githubusercontent.com/49056225/115200711-78f83780-a12f-11eb-8224-a23183f159e3.png" width="600" height="400"><br>
- IP주소가 부족해짐 → IP주소를 재사용해서 다른 사람과 같이 쓰자!!
  - 같은 네트워크 안의 사용자들은 각자 고유한 IP주소를 가짐
  - 같은 IP주소를 서로 다른 네트워크 사용자도 사용할 수 있음
  - 해결 방법) 라우터에서 제공하는 제공하는 주소 변환 서비스 NAT
- outbound
  - 로컬 네트워크 밖으로 나가는 패킷의 src IP주소를 호스트의 IP주소 -> 라우터의 IP주소로 변환
  - 호스트마다 포트 번호를 다르게 하여 NAT Translation table에 (호스트 IP주소, 포트 번호)를 적어둠
- inbound
  - NAT Translation table을 보고 dst IP주소를 라우터의 IP주소 -> 호스트의 IP주소로 변환
  - dst port번호로 로컬 네트워크의 호스트를 구분
- network layer에서 포트 번호(transport layer)를 수정하는 것은 layer violation
- IP주소를 변경하면서 헤더 체크섬을 다시 해야함 -> 오버헤드

### DHCP (Dynamic Host Configuration Protocol)
- 호스트의 IP주소와 TCP/IP 프로토콜의 기본 설정을 동적으로 할당해주는 프로토콜
- DHCP 서버는 호스트에게 일정 시간동안 IP주소를 빌려주고 다시 회수함 (IP 자동 할당과 분배 기능)
- 장점) DHCP 서버에서 IP주소를 자동으로 할당해주기 때문에 IP주소 관리가 쉽고 충돌을 예방할 수 있음
- 단점) 초기 부팅시 broadcast 트래픽을 유발 / DHCP 서버에 전적으로 
- 동작 방식
  - DHCP discover : 호스트가 자신의 존재를 broadcast로 알림
  - DHCP offer : IP주소를 빌려줄 수 있는 DHCP 서버가 응답 + 메시지에 사용 가능한 IP주소 보냄
  - DHCP request : 호스트가 해당 IP주소 사용을 요청
  - DHCP ack : DHCP 서버가 요청에 대해 응답
- 위의 네가지 메시지 모두 broadcast !!
  - broadcast로 통신함으로써 여러 대의 DHCP 서버가 있을 때 선택받지 못한 다른 DHCP 서버들이 현재 상황을 간접적으로 알 수 있기 때문

### ICMP (Internet Control Message Protocol)
- 호스트와 라우터가 네트워크 상황을 진단하기 위해 사용
- sender가 보낸 패킷이 목적지에 도달하지 못했다면 라우터가 ICMP 패킷을 
