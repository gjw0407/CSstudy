# Web Server vs WAS

# Static Page vs Dynamic Page

## Static Page

> 언제 접속해도 같은 응답을 보내주는 페이지
> 
- html, css, js, 이미지 → 즉시 응답 가능한 컨텐츠

## Dynamic Page

> 요청을 받은 이후 서버가 추가적인 처리 과정을 거친 이후 인자에 따라 바뀌는 페이지
> 
- jsp, php, asp → 동적인 데이터들을 정립해서 html 문서로 재조립 후 리턴

# Web Server

> 웹 브라우저 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램
> 

## 기능

- 하드웨어 : Web 서버가 설치 되어 있는 컴퓨터
- 소프트웨어 : 웹 브라우저 클라이언트로부터 HTTP 요청을 받고, 정적인 컨텐츠를 제공하는 컴퓨터 프로그램
- 정적인 파일을 애플리케이션 서버까지 가지 않고 앞단에서 처리 가능
- 동적인 컨텐츠 제공을 위한 요청 전달
    - to WAS & from WAS

## 종류

- Apache, Nginx, IIS

# WAS (Web Application Server)

> 인터넷 상에서 HTTP 프로토콜을 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어로서, 주로 동적 서버 컨텐츠를 수행하는 것으로 웹 서버와 구별이 되면, 주로 데이터베이스 서버와 같이 수행
> 

→ 웹 서버 + 웹 컨테이너

## 웹 컨테이너 or Servlet Container

- JSP, Servlet을 실행시킬 수 있는 SW
- JSP 요청 → Tomcat에서 JSP 파일을 서블릿으로 변환해 컴파일 수행 → 수행 결과 웹서버에 전달
- 서블릿의 생명주기를 관리
- 멀티스레딩 지원

### Apache의 CGI

- Common Gateway Interface
- 웹 서버와 외부 프로그램 사이에서 정보를 주고 받는 방법이나 규약
- 두 개 이상의 컴퓨터 간의 자료들을 주고 받는 프로그램
- JAVA는 CGI를 지원하지 않아 컨테이너가 필요

## 기능

- DB 조회 및 다양한 로직을 처리하는데 집중
- 서버 부하를 방지 (트랜잭션 관리)

## 종류

- Tomcat, JBoss, Jeus, Web Sphere

# 분리 이유

1. 서버 부하 방지
    1. 동적 + 정적 = 페이지 노출 시간이 늘어남
2. 보안 강화
    1. SSL에 대한 암복호화 처리에 Web Server 사용
    2. 웹 서버와 WAS에 접근하는 포트가 다르기 때문에 WAS에 들어오는 포트에는 방화벽을 쳐서 보안을 강화할 수도 있다
    3. WAS의 환경 설정 파일을 외부에 노출 시키지 않도록 하기 위해
3. 여러 대의 WAS 연결
    1. Load Balancing
    2. Fail over (장애 극복), fail back 처리에 유리
        1. fail over : 정지된 WAS 대신 다른 WAS를 사용해 장애 극복
        2. fail back : 정지된 WAS를 재동작
    3. 무중단 운영을 위한 장애 극복에 쉽게 대응
4. 기타
    1. 접근 허용 IP 관리

![Untitled](https://user-images.githubusercontent.com/61227459/176675511-a10933fa-e242-484c-aa4b-bcca4ed26e50.png)

<aside>
💡 Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.

</aside>

# Web Service Architecture

## 구조

1. Client -> Web Server -> DB
2. Client -> WAS -> DB
3. Client -> Web Server -> WAS -> DB

→ 3번이 common

![Untitled 1](https://user-images.githubusercontent.com/61227459/176675784-27718416-ccaa-40a2-a813-826b4d41d58b.png)

1. 클라이언트 요청
2. 웹 서버가 받아서 WAS에게 보내 Servlet을 메모리에 올림
3. WAS는 web.xml을 참조해 Servlet에 대한 스레드를 생성 (스레드 풀 이용)
4. HttpServletRequest & HttpServletResponse 생성해 Servlet에 전달
    1. 스레드는 Servlet의 service() 호출 → doGet(), doPost() 호출
5. 생성된 동적 페이지를 Response에 담아 WAS에 전달
6. WAS는 Response를 HttpResponse로 바꿔 웹 서버로 전달
7. 스레드 종료, HttpServletRequest & HttpServletResponse 제거

<aside>
💡 Apache Tomcat : Tomcat이 Apache의 기능을 포함하고 있기 때문
→ Tomcat 5.5부터 정적 컨텐츠 처리 기능 추가

</aside>

# 출처

[https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

[https://codechasseur.tistory.com/25](https://codechasseur.tistory.com/25)

[https://helloworld-88.tistory.com/71](https://helloworld-88.tistory.com/71)

[https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_web_server](https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_web_server)

[https://velog.io/@bky373/Web-웹-서버와-WAS](https://velog.io/@bky373/Web-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%99%80-WAS)

[https://coding-factory.tistory.com/741](https://coding-factory.tistory.com/741)

[https://devmoony.tistory.com/113](https://devmoony.tistory.com/113)