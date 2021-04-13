### UDP
- connectionless / unreliable delivery / unorder delivery (비교적 덜 정교한 비연결 프로토콜)
- 간단한 데이터를 빠르게 (실시간) 전송하는 서비스에 적합 ex) 영상 스트리밍, DNS
- 제공하는 서비스 (요놈과 UDP 세그먼트 헤더값을 연결해서 기억!!)
  1. **multiplexing과 demultiplexing**
  2. **error check**
- UDP 세그먼트 포맷
<img src="https://user-images.githubusercontent.com/49056225/114353116-9b2a0c80-9ba7-11eb-9318-f090107c7f66.png" width="500" height="300">

 ### TCP 특징
 <img src="https://user-images.githubusercontent.com/49056225/114558361-8fbd0b00-9ca5-11eb-8abd-22d79599b9dc.png" width="500" height="300"><br>
 - point-to-point : 소켓 한 쌍의 통신
 - reliable, in-order byte streams : byte 스트림을 에러, 손실없이 순서대로 전송
 - connection-oriented : 데이터를 주고받기 전에 handshaking
 - flow / congestion control : receiver가 받을 수 있는 만큼 전송
 - full duplex data (같은 TCP 연결에서 데이터가 양방향으로 전송 가능)
 
### TCP 헤더 (헤더를 보고 TCP의 모든 것을 설명할 수 있어야함!!)
<img src="https://user-images.githubusercontent.com/49056225/114547013-d3f5de80-9c98-11eb-8028-ef23c6748277.png" width="600" height="450"><br>
- source와 destination의 port번호
  - multiplexing과 demultiplexing을 위해 필요
- sequece number
  - TCP는 byte 스트림을 MSS(Maximum Segment Size)단위로 분할하고 TCP 헤더를 붙여 세그먼트를 생성
  - 해당 필드에 **data에 저장되는 byte 스트림의 첫번째 바이트**를 명시
  - SYN 필드가 1이면 초기, 0이면 누적 sequence number
  - receiver가 받은 세그먼트를 재조립 + duplicate segment를 구분하기 위해 필요
- ACK number : receiver 입장에서 sender로부터 받아야할 sequece number (= next byte)
  - ACK 필드에 1일때만 유효함
- Flags (Control bits)
  - ACK : ACK number 필드가 유효함을 나타냄
  - SYN : 양쪽이 보낸 최초의 패킷에만 해당 필드가 설정되어있음
  - FIN : TCP 연결 종료 (보낼 데이터 없음)
- window size
  - receiver 입장에서 받을 수 있는 크기 (byte 단위) 
  - 해당 필드로 TCP의 flow control을 할 수 있음
- checksum
  - error detection을 위해 필요 

### TCP의 reliable data transfer
- TCP는 IP의 unreliable 서비스 위에서 신뢰성 있는 전송을 제공 (TCP의 가장 큰 특징)
- TCP sender events
  - application layer로 부터 데이터를 받은 경우
    - byte stream을 MSS 크기로 분할하고 TCP 헤더와 seq#를 붙여서 세그먼트를 만듬
    - oldest unacked segment에 타이머를 설정 (재전송을 위한 타이머는 1개!)
  - timeout 발생한 경우
    - 타임아웃이 난 해당 세그먼트를 재전송
  - ACK를 받은 경우
    - ACK 번호가 SendBase(send buffer의 시작 위치)보다 크다면 SendBase를 ACK번호로 갱신
    - ex) ACK 10을 기다리고 있는데 ACK 11이 오면 10, 11 모두 받았다는 뜻 -> SendBase를 11로 갱신
    - 현재 동작중인 타이머가 없다면 oldest unacked segment에 타이머를 설정 
- TCP가 재전송하는 경우
  1. timeout
      - 해당 세그먼트의 ACK가 timeout 이내에 오지 않은 경우 해당 세그먼트를 재전송
  2. 3 Duplicate ACK 
      - 같은 번호의 ACK를 중복해서 3번 받으면 (총 4번) 해당 번호의 패킷을 유실로 판단하고 재전송
      - ex) 10번 패킷이 유실 -> receiver는 11, 12번 패킷이 와도 계속 ACK 10을 전송 -> sender는 10번 패킷 유실로 판단!
      - timeout전에 패킷 유실을 알 수 있으므로 fast transmission이라고 부름 
