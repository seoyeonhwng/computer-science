### multiplexing / demultiplexing
<img src="https://user-images.githubusercontent.com/49056225/114351381-7c2a7b00-9ba5-11eb-9a1b-8c05f297895d.png" width="500" height="300"><br>
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
    
### UDP
- connectionless / unreliable delivery / unorder delivery (비교적 덜 정교한 비연결 프로토콜)
- 간단한 데이터를 빠르게 (실시간) 전송하는 서비스에 적합 ex) 영상 스트리밍, DNS
- 제공하는 서비스 (요놈과 UDP 세그먼트 헤더값을 연결해서 기억!!)
  1. **multiplexing과 demultiplexing**
  2. **error check**
- UDP 세그먼트 포맷
<img src="https://user-images.githubusercontent.com/49056225/114353116-9b2a0c80-9ba7-11eb-9318-f090107c7f66.png" width="500" height="300">
  
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
  
