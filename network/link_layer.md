### 데이터링크 계층
- 물리적인 링크로 연결된 두 노드 사이의 전송을 담당
- framing / flow control / error check

### MAC 주소

### ARP

### MAC(Multiple Access Control) protocol
<img src="https://user-images.githubusercontent.com/49056225/115511628-023c7500-a2bc-11eb-8875-615c844b68a2.png" width="500" height="400"><br>
- broadcast medium으로 물리적 링크를 사용하는 경우 충돌이 나지 않도록 조절해야함
- 동시에 데이터를 전송하면 파동(신호)가 섞여서 데이터를 알아들을 수 없음
- MAC 프로토콜의 종륲
  - channel partitioning : 시간/주파수 등을 쪼개서 지정된 시간/주파수에만 전송 가능
    - load가 많을때 채널을 효율적이고 공정하게 나눠서 사용 가능
    - load가 적을때는 비효율적 (idle한 채널이 많기 때문)
  - random access : 전송하고 싶을때 전송 + 충돌 발생시 탐지/처리
    - load가 적을때 채널을 독점할 수 있기 때문에 효율적
    - load가 많을때 충돌이 많이 발생함
    
### CSMA/CD (carrier sense multiple access)
- Ethernet에서 사용하는 MAC 프로토콜
- listen befroe transmit = 채널이 비어있을때만 프레임을 전송 / 누군가 전송중이면 기다림
- carrier sense를 해도 propagation delay때문에 충돌이 발생할 수 있음
- 충돌을 감지하면 전송을 중단하고 jam signal을 전송
- 랜덤한 시간만큼 기다렸다가 재전송 (exponential backoff / 기다리는 시간은 충돌 횟수와 비례)
<img src="https://user-images.githubusercontent.com/49056225/115513022-94914880-a2bd-11eb-96ff-39acbfd7dff6.png" width="500" height="400"><br>

### Ethernet 프레임
<img src="https://user-images.githubusercontent.com/49056225/115515050-b25fad00-a2bf-11eb-810c-4a5d40aeabb3.png" width="400" height="100">
- header
  - preamble : receiver에게 프레임 도착을 알림 (synchronize)
  - dst Addr / src Addr : destination의 MAC주소 / source의 MAC주소
  - type : 상위 layer의 종류
- tailer
  - CRC : error check (CRC를 통해 error control이 가능함)
