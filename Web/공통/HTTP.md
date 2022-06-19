# HTTP

- HyperText Transfer Protocol
- 인터넷에서 문서를 주고받기 위한 텍스트 기반 프로토콜
- Hypertext: 참조(Hyper Link)를 통해서 한 문서에서 다른 문서로 접근할 수 있는 텍스트
- 80번 포트 사용

### 특징

1. 대부분의 파일 형식 전송이 가능하다.
    - HTML, JSON, XML, Text, Image, 음성, 영상 파일 등 모두 HTTP를 통해 전송 가능
2. 서버/클라이언트 모델을 따른다.
    - 클라이언트가 요청을 하면 서버는 응답만 하는 단방향 통신
3. application layer 프로토콜
    - TCP/IP layer 위에서 동작한다.
4. connectionless
    - 응답 후 TCP/IP 연결 상태를 유지하지 않는다.
    - 서버의 자원을 효율적으로 관리하고, 수많은 요청에도 대응할 수 있다.
    - 대안: HTTP Persistence Connection(지속 연결) - HTTP/2, HTTP/3, 요청에 따라 일정 시간 연결 유지하거나 여러 개 요청에 대한 응답이 다 올 때까지 기다린 후 연결 종료
5. stateless
    - 서버가 클라이언트의 이전 상태를 저장하지 않는다.
    - 어느 서버가 응답해도 상관없다. 서버간 정보 공유를 할 필요가 없다.
    - 대안: cookie, session

## Method

http metod, 클라이언트가 서버에게 요청의 목적/종류를 알리는 수단

### 종류

1. GET
    - 리소스 조회
    - query parameter를 통해서 데이터 전달
        - body로 데이터를 전달하는 방법은 권장하지 않음
2. POST
    - 리소스 생성
    - message body를 통해서 데이터 전달
3. PUT
    - 리소스 대체(전체 수정), 없으면 생성
4. PATCH
    - 리소스 부분 업데이트
5. DELETE
    - 리소스 삭제
6. HEAD, OPTIONS, CONNECT, TRACE

### 속성

1. 안전성 Safe
    - 서버에 영향을 줄 수 있는 안전하지 않은 메서드가 사용될 때 사용자들에게 알려준다.
    - 읽기 전용(리소스 수정 X) 메서드: GET, HEAD, OPTIONS
2. 멱등성 Idempotent
    - 특정 메서드를 여러번 호출해도 같은 결과를 가진다
    - GET, PUT, DELETE, HEAD, OPTIONS, TRACE
3. 캐시가능성 Cacheable
    - 재사용을 위해 응답을 저장할 수 있다.
    - GET, HEAD

## Status Code

서버의 요청 처리 결과에 따른 응답 상태 코드가 다르다.

### 1xx (Informational)

요청이 수신되어 처리 중이다

### 2xx (Successful)

요청이 정상적으로 처리되었다

1. 200 OK: 요청 성공
2. 201 Created: 새로운 리소스 생성
3. 202 Accepted: 요청 접수되었으나 처리가 완료되지 않았음
4. 204 No Content: 성공했지만, 응답 payload 데이터가 없음

### 3xx (Redirection)

location 헤더가 있다면 요청을 완료하기 위해 location 위치로 자동 이동한다

1. 301 Moved Permanently: 리다이렉트시 GET으로 변하고 본문이 제거될 수 있음
2. 302 Found: 리다이렉트시 GET으로 변하고 본문이 제거될 수 있음
3. 303 See Other: 리다이렉트시 GET으로 변경
4. 304 Not Modified: 캐시 목적
5. 307 Temporary Redirect: 리다이렉트시 요청 메서드와 본문 유지
6. 308 Permanent Redirect: 리다이렉트시 요청 메서드와 본문 유지

### 4xx (Client Error)

잘못된 문법 등으로 서버가 요청을 수행할 수 없다

1. 400 Bad Request: 클라이언트의 잘못된 요청으로 서버가 처리할 수 없음
2. 401 Unauthorized: 리소스에 유효한 인증 자격 증명이 없음. 403과 다르게 인증 가능
    - 엄연히 authorized가 아니라 authentication이 정확하다
    - missing or bad authentication
3. 403 Forbidden: 서버가 요청이 전달됐지만 권한 때문에 거절됨. 재인증(re-authenticating)을 하더라도 접속 거절.
    - know who you are but don’t have permission
    - user is authenticated but is not authorized
4. 404 Not Found: 요청 리소스를 찾을 수 없음

### 5xx (Server Error)

서버가 정상 요청을 처리하지 못했다

1. 500 Internal Server Error: 서버 문제로 오류 발생
2. 503 Service Unavailable: 서비스 이용 불가

## 1.1 vs 2.0

### HTTP 1.1

- 한 번의 연결에 하나의 요청과 응답 처리
- 동시 전송이 어렵고, 여러 리소스를 처리하기에 속도와 성능이 느릴 수 있다.

### HTTP 2.0

- 1.1보다 성능, 속도 면에서 우수
- Multiplexed Streams: 한 connection에 여러 메세지를 동시에 주고 받을 수 있음
- Stream Prioritization: 요청 리소스간의 의존관계를 설정
- Server Push: HTML 문서에 필요한 리소스를 클라이언트 요청 없이 보내줄 수 있음
- Header Compression: Header 정보를 HPACK 압축 방식을 이용해서 압축 전송

## HTTPS

- HyperText Transfer Protocol Secure (HyperText Transfer Protocol over Secure Socket Layer, HTTP over TLS, HTTP over SSL, HTTP Secure 등)
- HTTP에 데이터 암호화가 추가된 프로토콜
    - 네트워크 상에서 제3자가 중간에 정보를 볼 수 없도록 암호화 지원
- 인증서를 발급하고 유지하기 위한 추가 비용 발생
- 443번 포트 사용

### 연결 과정

대칭키 암호화, 비대칭키(공개키, 개인키) 암호화를 모두 사용하여 빠른 연산 속도와 안정성을 얻는다.

1. 클라이언트(브라우저)가 서버와 최초 연결 시도를 한다.
2. 서버는 공개키(인증서)를 브라우저에게 넘겨준다.
3. 브라우저는 인증서의 유효성을 검사하고 세션키(대칭키)를 발급한다.
4. 브라우저는 세션키를 보관하고, 서버의 공개키로 세션키를 암호화하여 서버로 전송한다.
5. 서버는 개인키로 암호화된 세션키를 복호화해서 세션키를 얻는다.
6. 클라이언트와 서버는 데이터를 전달할 때 공유한 세션키로 암호/복호화를 진행한다.
