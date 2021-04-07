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

### 쿠키와 세션

- request/response 메시지에 쿠키 정보를 추가해서 클라이언트의 상태를 파악
- 쿠키의 동작 방식
    - 클라이언트가 쿠키를 가지지 않은 상태에서 서버에게 request를 전송
    - 서버는 쿠키를 발행하고 기억해둠 + response 헤더의 set-cookie에 쿠키를 붙여서 응답
    - 클라이언트는 해당 쿠키를 자신의 웹브라우저에 저장
    - 같은 요청을 하는 경우 request 헤더의 cookie에 해당 쿠키를 붙여서 전송      
    (서버에서 클라이언트가 보낸 쿠키를 확인하여 클라이언트의 이전 상태를 알 수 있음)
- Set-Cookie 속성
    - Expires=DATE : 쿠키 유효 기한 (지정되지 않은 경우는 브라우저를 닫을 때까지)
    - Secure : HTTPS로 통신하고 있는 경우에만 쿠키를 송신
    - HttpOnly : 쿠키를 자바스크립트에서 액세스하지 못하도록 제한
- 세션
    - 쿠키에는 민감한 정보를 담을 수 없기 때문에 세션을 사용함
    - 일정 시간동안 같은 브라우저로부터 들어오는 요구를 하나의 상태로 보고 그 상태를 유지함
    - 서버는 클라이언트를 식별할 수 있는 session ID를 response 헤더의 set-Cookie에 붙여서 전송
    - 클라이언트는 reqeust 헤더에 cookie에 session ID를 붙여서 전송
- 쿠키 VS 세션
    - 저장 위치 : 쿠키는 클라이언트, 세션은 서버의 메모리에 저장
    - 보안 : 쿠키는 클라이언트 로컬에 저장 + HTTP 메시지 도청 때문에 보안에 취약
    - 라이프 사이클 : 쿠키는 브라우저가 종료되어도 저장할 수 있지만 세션ID는 브라우저 단위
    
### HTTP 메시지
- Request 메시지 : 메소드 + URI + 프로토콜 버젼 + 헤더 필드 + 엔티티
    - 메소드 : 리소스가 어떤 행동을 하기 원하는지 지시
    - URI(Uniform Resource Identifiers) : 인터넷 상의 리소스를 지정
```
GET /index.html HTTP/1.1
Host: www.hackr.jp
```

- Response 메시지 : 프로토콜 버젼 + 상태 코드 + 상태 설명 + 헤더 필드 + 바디
```
HTTP/1.1 200 OK
Date: Tue, 10 Jul 2020
Content-Length: 362
Content-Type: text/html

<html>
...
```

### GET 방식 VS POST 방식
