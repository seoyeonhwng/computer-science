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
 - point-to-point : 소켓 한 쌍의 통신 (일대일 통신)
 - reliable, in-order byte streams : byte 스트림을 에러, 손실없이 순서대로 전송
 - connection-oriented : 데이터를 주고받기 전에 handshaking
 - flow / congestion control : receiver가 받을 수 있는 만큼 전송
 - full duplex data (같은 TCP 연결에서 데이터가 양방향으로 전송 가능)
 - 높은 신뢰성을 요구하는 서비스에 적합
 - 단점) 매번 connection 연결 → 시간 손실(느림) / 패킷을 조금만 손실해도 재전송
 
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
    - timeout 기준 : estimated RTT (Round Trip Time, 세그먼트가 전송되고 다시 돌아오는 시간)
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

### TCP의 connection
- Connection / Three way handshake (클라이언트와 서버측에 각각 send/receive 버퍼를 할당하는 과정)
<br><img src="https://user-images.githubusercontent.com/49056225/114818126-32849f00-9df6-11eb-9831-0c6e9370d96e.png" width="400" height="300"><br>
    1. 클라이언트는 SYN 세그먼트를 서버에게 전송
        - 해당 세그먼트 헤더의 sequence number에는 클라이언트의 초기 seq#를 명시 + no data
    2. 서버는 SYNACK 세그먼트를 클라이언트에게 전송
        - 해당 세그먼트 헤더의 sequence number에는 서버의 초기 seq#를 명시 + no data
        - 서버측에 send/receive 버퍼를 할당
    3. 클라이언트는 ACK 세그먼트를 서버에게 전송
        - 해당 세그먼트부터 데이터 포함됨
    - two way handshake만 한다면 서버는 클라이언트가 살아있음에 확신이 없음!!
 - Termination / Four way handshake
 <br><img src="https://user-images.githubusercontent.com/49056225/114818619-0cabca00-9df7-11eb-9611-9adeec5bec5e.png" width="400" height="300"><br>
     1. 클라이언트는 FIN 세그먼트를 서버에게 전송
         - FIN = 1은 보낼 데이터가 더이상 없다는 의미 + no data
     2. 서버는 ACK 세그먼트를 클라이언트에게 전송
         - 클라이언트가 보낸 ACK를 받았다는 의미
     3. 서버는 보낼 데이터가 더이상 없을때 FIN 세그먼트를 전송 (connection을 바로 종료하지 않음)
         - 4번에서 클라이언트각 보낸 ACK가 유실되면 서버는 FIN을 재전송해야 하기 때문
     4. 클라이언트는 ACK를 서버에게 전송하고 "timed wait" 상태가 됨 (connection을 바로 종료하지 않음)
         - 보낸 ACK가 유실될 수 있기 때문!
         - ACK가 유실되면 서버는 FIN을 재전송 -> 만약 connection을 바로 종료하면 클라이언트는 ACK를 재전송할 수 없음 

### TCP의 Flow Control
- sender는 receive buffer(=spare room + TCP data)의 여유 공간보다 더 많은 데이터를 전송하지 않음
- receiver가 받을 수 있는 만큼만 전송
- 동작 과정
  - receiver는 세그먼트의 window size에 현재 버퍼의 여유 공간을 담아서 전송
  - sender는 해당 여유 공간만큼만 세그먼트를 전송
- receiver가 window size를 0으로 설정한 세그먼트를 전송하는 경우
  - sender는 더이상 데이터를 전송하지 않음 -> 언제까지?
  - sender는 데이터가 없는 세그먼트를 주기적으로 보내고 이에 대한 ACK를 받아 ACK의 window size를 확인하여 receiver의 상태를 확인

### TCP의 Congestion Control
- TCP는 loss event가 발생하면 네트워크가 혼잡하다고 판단 (네트워크의 직접적인 피드백 없음)
- TCP sender의 window size는 네트워크 상황에 맞게 동적으로 바뀜
- 3 main phases
    1. Slow Start : CongWin(Congestion Window Size)를 1MSS, 2MSS, 4MSS ... 더블링 구조로 증가시킴
    2. Additive increase : CongWin이 정해둔 threshold를 지나면 window size를 1MSS씩 증가시킴
    3. Multiplicative decrease : 패킷 유실이 일어나면 threshold을 절반으로 줄이고 slow start부터 시작
- loss event 판단 기준
    - timeout과 3 Duplicate ACK
    - 특정 패킷만 유실되었다면 timeout보다 3 Duplicate ACK가 먼저 발생
    - timeout이 발생한 경우 네트워크 상황이 더 좋지 않음
- Refinement (Tahoe & Reno)
  - timeout에 의한 loss event라면 이전과 똑같이 행동 (threshold를 절반으로 줄이고 slow start부터 시작)
  - 3 Dup ACK에 의한 loss event라면 threshold와 CongWin을 절반으로 줄이고 linear increase부터 시작 
