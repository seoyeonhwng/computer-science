### Symmetric key cryptography (공통키 암호화)
- Alice와 Bob은 같은 키를 공유하여 데이터를 암호화/복호화함
- 장점) 처리 속도가 빠르고 효율적
- 단점) 키를 안전하게 주고 받을 수 없음

### Public key cryptography (공개키 암호화)
- Alice와 Bob은 각각 서로 다른 2개의 키를 가지고 있음
- public key : 모두가 다 알고있음 / private key : 나만 알고있음
- Alice가 Bob에게 메시지를 전송할때 B의 public key로 암호화해서 보냄 
- 해당 내용을 복호화할 수 있는 사람은 B의 private key를 가진 Bob뿐!

### HTTP의 약점
- 암호화하지 않은 통신이기 때문에 도청 가능
  - 하위 프로토콜인 TCP/IP에서 보안 기능을 제공하지 않기 때문에 HTTP를 사용한 통신을 암호화할 수 없음
- 통신 상대를 확인하지 않기 때문에 위장 가능
- 완전성 (정보의 정확성)을 증명할 수 없기 떄문에 변조 가능

### HTTPS의 통신과 데이터 암호화
- SSL(Secure Socket Layer)를 사용하여 통신을 암호화 / HTTP + SSL + TCP + IP 
- 하이브리드 암호화 방식으로 데이터를 암호화
  - 공통키 암호로 사용하는 키(대칭키)를 공개키 암호화 방식으로 안전하게 교환하고 그 뒤로는 공통키 암호화 방식으로 통신
  - 클라이언트와 서버는 대칭키로 암호화/복호화하여 데이터를 안전하게 교환 = 통신을 암호화할 수 있음

### HTTPS의 통신 상대 확인
- 문제 상황) 클라이언트는 서버의 public key로 공통키를 암호화해서 전송
  - 서버의 public key라는 것을 어떻게 증명할 수 있을까?
- 해결책) 인증 기관(Certificate Authority)에서 발행한 public key 증명서
  - 인증서에는 '이것은 누구의 public key야'라고 적혀있는 내용을 인증 기관의 private key로 암호화되어있음
  - 인증 기관의 public key로 복호화하면 서버의 public key를 얻을 수 있음
  - 인증 기관의 public key는 브라우저에 내장되어있어 믿을 수 있음
  
### HTTPS의 통신 과정
1. 클라이언트가 Client Hello 메시지를 송신하면서 SSL 통신을 시작
  - 메시지 안에는 랜덤값, 클라이언트 측에서 사용 가능한 암호화 기법을 전송
2. 서버가 SSL 통신이 가능한 경우 Server Hello 메시지로 응답
  - 메시지 안에는 랜덤값, 서버 측에서 사용 가능한 암호화 기법을 전송
3. 서버가 Certificate 메시지를 송신 (공개키 증명서)
4. 클라이언트는 브라우저에 내장된 인증기관의 public key로 공개키 증명서를 암호화하여 서버의 public key를 획득
5. 클라이언트는 주고받은 랜덤값을 이용해서 symmetric key를 생성 후 서버의 public key로 암호화해서 전송
6. 서버는 자신의 private key로 복호화하여 symmetric key를 얻음
7. 이후부터 클라이언트와 서버는 symmetric key로 메시지를 암호화/복호화해서 전송

### HTTPS의 단점
- HTTP보다 속도 처리가 느림
  - SSL에 필요한 통신이 추가되기 때문에 전체적으로 처리해야할 통신이 증가하기 
  - 서버와 클라이언트 모두 암호화와 복호화 처리가 필요하기 때문에 CPU, 메모리 등 리소스를 소비하기 때문
- 처리할 수 있는 리퀘스트의 수가 줄어들게 됨
- 그렇기 때문에 민감한 정보를 다룰때만 HTTPS를 사용하여 통신함

<img src = 'https://user-images.githubusercontent.com/49056225/114295394-f2fd4080-9adf-11eb-9c78-0ad1c1066e6c.png' width="400" height="500">
