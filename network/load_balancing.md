### 트래픽 증가에 대처하는 방법
- scale-up / 수직 확장 : 기존 서버의 성능을 올리는 방법
- scale-out / 수평 확장 : 서버의 개수를 추가하여 여러 대의 서버가 일을 나눠서 하도록 하는 방법
  - scale-up보다 더 좋음!
  - 하드웨어 향상 비용보다 서버를 추가하는 비용이 더 저렴하기 때문
  - 서버가 여러 대 있으면 서버 1대의 장애가 발생하여도 계속 서비스를 제공할 수 있기 때문
  - 여러 대의 서버가 요청을 균등하게 나눠서 처리할 수 있도록 로드밸런싱을 해줘야함

### 로드밸런서
- 여러 대의 서버가 분산 처리할 수 있도록 요청을 나누어주는 서비스
- LB을 사용하면 여러대의 서버가 요청을 나눠서 처리하기 때문에 사용자 입장에서 응답이 빨라지고 가용성이 높아짐
- 로드밸런서는 Virtual IP와 함께 구성됨
  - 로드밸런싱의 대상이 되는 여러 서버들을 대표하는 가상 IP주소
  - 클라이언트는 서버의 IP가 아닌 로드밸런서의 VIP로 요청함

### 로드밸런서 동작 방식
- 로드밸런서가 중간에서 NAT를 통해 IP주소를 변환하여 요청/응답을 처리
- 요청  
  - 클라이언트가 로드밸런서의 VIP로 요청을 전송 (DNS가 VIP로 알려줌) 
  - 로드밸런서는 NAT를 통해 dst, src IP주소를 변환하여 실제 서버로 전송
    - dst IP주소 : VIP -> 실제 서버의 IP주소 / src IP주소 : 클라이언트의 IP주소 -> VIP
- 응답
  - 실제 서버가 로드밸런서에게 받은 응답을 전달
  - 로드밸런서는 NAT를 통해 dst, src IP주소를 변환하여 클라이언트에게 전송
    - dst IP주소 : VIP -> 클라이언트의 IP주소 / src IP주소 : 실제 서버의 IP주소 -> VIP 

### 로드밸런서의 종류
- OSI 7계층 중에서 어떤 계층을 기준으로 분산 작업을 하느냐에 따라 L2, L3, L4, L7로 나뉨
- 상위 계층일수록 섬세한 로드밸런싱 + 가격이 비쌈 / 하위 계층일수록 간단한 로드밸런싱 + 가격이 저렴
- L4 로드 밸런서
  - transport 계층의 정보(IP주소와 port번호)를 바탕으로 로드밸런싱
  - ex) www.google.com:80 으로 접속시 서버A,B로 로드밸런싱
  - 장점) HTTP message의 내용과 상관없이 로드밸런싱하기 때문에 속도가 빠르고 효율이 좋음
  - 단점) L7보다 섬세하지 못한 로드밸런싱 / 사용자의 IP주소가 수시로 바뀌는 경우라면 연속적인 서비스 제공이 불가능
- L7 로드 밸런서 
  - 사용자의 요청(URL, HTTP 헤더, 쿠키)을 바탕으로 로드밸런싱
  - L4보다 클라이언트의 요청을 보다 세분화해서 서버에 전달할 수 있음
  - ex) www.google.com으로 접속시 /search와 /map을 서버A,B로 로드밸런싱 (사용자의 요청 URL을 사용)
  - 장점) 섬세한 로드밸런싱 / 캐싱 기능 제공 / 비정상적인 트래픽 차단
  
### 로드밸런서가 서버를 선택하는 방법 (로드 밸런싱 알고리즘)
- 여러 대의 서버에 트래픽을 어떤 기준으로 분산시키는가
1. 라운드로빈
    - 서버에 들어온 요청을 순서대로 돌아가면서 배정 (첫번째 요청은 첫번째 서버가!)
    - 여러 대의 서버의 성능이 동일하고 처리 시간이 짧은 어플리케이션인 경우 균일하게 분산이 이루어짐
    - 서버의 처리와 관계없이 연결이 할당되기 때문에 세션 유지 기능이 필요한 어플리케이션은 적절하지 않음
2. 가중 라운드로빈
    - 각 서버에 가중치를 매기고 가중치가 높은 서버에게 요청을 우선적으로 배정
    - 서버의 트래픽 처리 능력이 서로 다른 경우 적합
3. IP 해시
    - 클라이언트의 IP주소로 해시값을 생성하여 해시값을 기반으로 배정
    - 사용자가 항상 동일한 서버에 연결되는 것을 보장 -> 인증같이 세션을 유지해야 하는 사이트에서 사용
4. 최소 연결 방식 
    - 요청이 들어온 시점에 가장 적은 연결 상태를 보이는 서버에게 배정
    - 서버에 분배된 트래픽들이 일정하지 않은 경우 적합

### 로드밸런서 장애 대비
- 로드밸런서를 여러개 두어 장애에 대비할 수 있음
- 메인 로드밸런서가 동작하지 않으면 VIP는 여분의 로드밸런서로 변경됨
