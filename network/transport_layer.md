### Transport Layer
- 서로 다른 호스트에서 동작하는 어플리케이션 프로세스간의 논리적 통신을 제공하는 계층
- 즉, client의 프로세스A와 server의 프로세스B 사이의 메시지 전달 (processs-process)
- 논리적 통신 = 실제로 두 프로세스가 물리적으로 연결되어있지 않지만 애플리케이션들은 서로 연결되어있다고 느낌
  - 소켓이라는 인터페이스를 사용하여 논리적 통신을 구현
  - 소켓 주소는 (IP주소 + 포트번호)이므로 서로 다른 호스트의 프로세스를 구분할 수 있음
  - 서로 다른 소켓 주소를 맵핑함으로써 논리적 연결을 구현

### multiplexing / demultiplexing
<img src="https://user-images.githubusercontent.com/49056225/114351381-7c2a7b00-9ba5-11eb-9a1b-8c05f297895d.png" width="500" height="300"><br>
- 'host-to-host delivery'를 'process-to-process delivery'로 확장 
- multiplexing (다중화)
  - 여러 소스(소켓)의 입력을 하나의 출력(세그먼트)으로 묶는 작업
  - application layer의 어떤 소켓으로 내려온 데이터라도 잘라서 세그먼트로 만들고 
  - 각 세그먼트에 헤더 정보를 넣고 network layer로 전송하는 과정
- demultiplexing (비다중화)
  - 단일 소스(세그먼트)의 입력을 다중 출력(소켓)으로 운반하는 작업
  - 전송된 세그먼트의 헤더를 보고 올바른 소켓으로 데이터를 전송하는 과정
  - demultiplexing 동작 방식
    - UDP는 **dst IP + dst port번호**로 어떤 소켓으로 전송할지 결정 (connection 개념이 없기 때문에 src 상관없음)
    - TCP는 **dst IP + dst port번호 + src IP + src port번호**로 어떤 소켓으로 전송할지 결정 (connection 했기 때문에 정해진 놈만!)
    
### Reliable Data Transfer
- 네트워크가 unreliable channel이기 때문에 신뢰성 있는 전송 프로토콜이 필요함
- unreliable channel에서 일어날 수 있는 일 : message error + message loss
- message error를 제어하기 위해 필요한 매커니즘
  1. **error detection** : checksum으로 가능
  2. **Feedback** : ACK를 전송함으로써 에러없이 패킷을 잘 받았는지 상대방에게 알려줌
  3. **Retransmission** : 에러난 메시지를 재전송 
  4. **sequence #** : 재전송을 하기 때문에 duplicate message를 구별하기 위해 사용
- message loss를 제어하기 위해 필요한 매커니즘
  1. **timer** : sender는 패킷을 전송하고 일정 시간동안 ACK가 오지 않으면 loss로 판단
  
### Pipelined protocols
- 신뢰성있는 데이터 전송을 위해 패킷 하나 보낼때마다 ACK를 받는 것은 비효율적
- 패킷 여러개를 한번에 보내고 한번에 피드백을 받자 (utilization이 높아짐)
- 대표적인 프로토콜 : Go-Back-N과 Selective Repeat 프로토콜
  - window size인 N만큼 패킷을 한번에 전송하고 ACK를 받으면 window를 오른쪽으로 이동
  - cumulative ACK 사용 ex)ACK 11 (11번까지 잘 받았어. 12번 줘)
  - go-Back-N 프로토콜
    - sender는 window안에서 ACK를 받지 못해서 타임아웃 난 패킷의 seq#보다 큰 패킷을 모두 재전송
    - receiver는 자신이 기다리는 패킷이 아니면 버리고 어디까지 잘 받았는지 나타내는 ACK를 전송 
    - 장점) 구현하기 간단 / 단점) 유실되지 않은 패킷까지 재전송하는 것은 비효율적
  - Selective Repeat 프로토콜
    - sender는 ACK를 받지 못해서 타임아웃이 난 패킷만 선택적으로 재전송
    - receiver는 자신이 기다리는 패킷이 아니면 버퍼에 저장하고 해당 패킷을 받았다는 ACK를 전송 -> receiver window도 존재!
    - 단점) 구현이 복잡 / 유실된 패킷만 재전송하므로 효율적
