### 네트워크 계층
- sender에서 라우터를 거쳐 receiver까지 각 패킷의 전달을 담당
- 패킷을 어떤 경로로 목적지까지 전달할 것인가 -> 포워딩과 라우팅으로 경로를 결정
- 라우터는 패킷을 목적지까지 전달하는 일을 하기 때문에 네트워크 계층까지 존재
  - 라우터에는 end-to-end connection 개념없음 -> 그저 dst IP주소를 보고 적절한 경로로 패킷을 전달!

### 포워딩 / 라우팅
- 네트워크 계층의 핵심 기능 = 포워딩과 라우팅
- 포워딩 (forwarding)
  - 라우터의 forwarding table을 보고 특정 input으로 들어온 패킷을 적절한 output으로 내보내는 일
  - 패킷의 dst IP주소를 보고 longest prefix matching 방식으로 output link를 결정
- 라우팅 (routing)
  - routing 알고리즘을 통해 패킷의 경로를 결정
  - forwarding table을 채우는 작업

### IP Address (IPv4)
- 32bit / 호스트나 라우터의 네트워크 인터페이스를 고유하게 식별 (주민번호처럼!)
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
- IP주소가 부족해짐 → IP주소를 재사용해서 다른 사람과 같이 쓰자!!

<img src="https://user-images.githubusercontent.com/49056225/115200711-78f83780-a12f-11eb-8224-a23183f159e3.png" width="600" height="400">
