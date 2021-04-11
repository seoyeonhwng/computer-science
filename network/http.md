## HTTP (Hyper Text Transfer Protocol)

- 인터넷에서 데이터를 주고 받을 수 있는 프로토콜
- 클라이언트와 서버가 request/response 메시지를 교환
- TCP/IP를 이용하는 응용 프로토콜

### 단방향성
- 클라이언트가 서버로 요청을 보내면 이에 대한 응답을 받는 단방향 통신

### 비연결성 (connectionless)
- 한 번의 TCP 연결로 한 번의 HTTP 요청과 응답을 하고나면 TCP 연결을 끊음
- 단점) TCP 연결/종료로 인한 오버헤드
- 해결책) keep-alive 헤더 사용 (Persistent connection)

### 무상태성 (stateless)
- 이전에 주고 받았던 request/response를 기억하지 않음
- 장점) 데이터를 매우 빠르고 확실하게 처리 + 서버의 리소스 절약
- 단점) 상태를 기억해야하는 상황도 존재 (ex. 로그인)
- 이를 보완하고 상태를 기억하기 위해 cookie, seesion, token 등을 사용

### persistent connection

- TCP 연결을 계속 유지함 (connection을 재사용)
- request/response 메시지의 헤더에 "Connection: keep-alive" 를 추가해서 구현
- HTTP1.1에서는 기본적으로 지원하기 때문에 "Connection: close"으로 사용하지 않을 수 있음
- 장점) TCP 연결/종료로 인한 오버헤드 감소 + 응답 속도 빨라짐
- 단점) 서버에 연결된 모든 클라이언트의 TCP 연결이 계속 늘어나면 서버의 자원이 고갈됨 → 클라이언트의 접속이 잦은 메인 페이지는 persistent connection을 고려해야함
- HTTP pipelineing을 가능하게 함
    - response를 받을 때까지 기다리지 않고 다음 request를 보낼 수 있음
    - 문제점) 순차적으로 request를 처리하므로 먼저 온 request가 끝날때까지 기다려야함 = Head Of Line Blocking 문제
    
### HTTP 메시지
- Request 메시지 : (메소드 + URI + 프로토콜 버젼) + 헤더 필드 + 엔티티
    - 메소드 : 리소스가 어떤 행동을 하기 원하는지 지시
    - URI(Uniform Resource Identifiers) : 인터넷 상의 리소스를 지정
```
GET /index.html HTTP/1.1
Host: www.hackr.jp
```

- Response 메시지 : (프로토콜 버젼 + 상태 코드 + 상태 설명) + 헤더 필드 + 바디
```
HTTP/1.1 200 OK
Date: Tue, 10 Jul 2020
Content-Length: 362
Content-Type: text/html

<html>
...
```

### GET 방식 vs POST 방식
- GET 방식 (정보 조회할때 사용하는 메소드)
    - 필요한 데이터를 HTTP 메세지 header의 url에 담아서 전송 (쿼리 스트링)
    - 전송 데이터의 한계 (길이의 한계)
    - 데이터가 그대로 보여지기 때문에 보안성이 떨어짐
    - POST 방식보다 전송 속도가 빠르다
- POST 방식 (정보 생성, 변경할때 사용하는 메소드)
    - 필요한 데이터를 HTTP 메세지의 body에 담아서 전송
    - 대용량 데이터 전송 가능
    - 데이터가 보여지기 않기 때문헤 GET보다 보안성이 높다
    - GET 방식보다 전송 속도가 느리다

### HTTP 응답 코드
- 2XX : 성공, 요청이 정상적으로 이루어졌음
    - 200 OK : 응답 성공
    - 204 No Content : response에 body는 없고 헤더만 존재 (새로운 정보를 보낼 필요가 없는 경우)
- 3XX : 리다이렉션, 요청을 완료하기 위해 다른 주소로 이동해야 함
    - 304 Not Modified : 조건부 리퀘스트를 했을때 응답이 수정되지 않음 / 캐시 목적
- 4XX : 클라이언트 오류, 올바르지 않은 요청
    - 400 Bad Request : 리퀘스트 문법이 잘못됨
    - 403 Forbidden : 리퀘스트된 리소스의 액세스가 거부됨 (접근 권한 없음)
    - 404 Not Found : 리퀘스트한 리소스가 서버상에 없음
- 5XX : 서버 오류, 올바른 요청에 대해 서버의 문제로 응답 불가능함
    - 500 Internal Server Error : 서버에서 리퀘스트를 처리하는 도중에 에러 발생

---
## 웹 서버
### 가상 호스트
- 웹 서버에서 기본적으로 존재하는 호스트 외에 여러 개의 호스트를 생성할 수 있음
- 기본적으로 존재하는 메인 호스트 외의 모든 호스트를 가상 호스트라고 함
- 이름 기반의 가상 호스트
    - 하나의 IP주소에 도메인이 다른 여러 개의 가상 호스트가 존재
    - 도메인명은 DNS에 의해서 IP주소로 변환 -> 가상 호스트들은 IP주소가 같음 -> request 메시지의 Host 헤더 필드로 구분
- 포트 기반의 가상 호스트
    - 동일한 호스트에 포트 번호가 다른 여러 개의 가상 호스트가 존재

### 프록시
- 클라이언트와 서버 사이에 통신을 중계함 (리퀘스트를 서버에 전송하고, 리스폰스를 클라이언트에게 전송)
- 클라이언트와 오리진 서버 사이에서 오리진 서버의 개입없이 클라이언트의 요청을 만족시킴
- 프록시 서버를 경유할때마다 HTTP message의 via 헤더 필드에 추가
- 캐시와 보안 목적으로 사용함

### 게이트웨이
 - 다른 서버를 중계하는 서버 (다음에 있는 서버가 HTTP 서버 이외인 경우)
 - 쇼핑 사이트에서 신용 카드 결제 시스템과 연계할때 사용됨
