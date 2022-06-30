# Cookie

# 특징

- 상태 정보를 클라이언트에 저장 (사용자 PC)
- 1 cookie = 4KB(4096byte)
- 이름, 값, 만료일, 경로 정보로 구성
- 접근 속도가 빠름
- 노출이 쉬움
- 브라우저 닫아도 쿠키 저장 되어 있음 (timeout 하지 않을 시)
- 예: “오늘 다시 보지 않기”

# 동작 순서

1. 클라이언트가 페이지 요청
2. 웹 서버 쿠키 생성
3. 생성한 쿠키에 정보를 담아 HTTP 화면을 돌려줄 때, 같이 클라이언트에 응답
4. 넘겨 받은 쿠키는 클라이언트가 지니고 있고 요청 시 쿠키 전송
5. 동일 사이트 재방문 시 클라이언트의 PC에 해당 쿠키가 있을 경우, 요청 페이지와 함께 쿠키를 전송

# 코드

## 생성

```java
private void setCookieExam(HttpServletRequest req, HttpServletResponse resp) throws IOException {
  // 1. 쿠키 생성하기
  Cookie userId = new Cookie("userId", req.getParameter("userId"));

  // 2. 쿠키값에 한글을 사용시 인코딩 처리를 해준다.
  Cookie name = new Cookie("name", URLEncoder.encode(req.getParameter("name"), "utf-8"));

  // 3. 쿠키 소멸 시간 설정(초단위) => 지정하지 않으면 웹브라우저 종료할 때 쿠키를 함께 삭제
  userId.setMaxAge(60*60*24); // 1일
//name.setMaxAge(60*60*48); // 2일

  // 4. 응답헤더에 쿠키 추가하기
  resp.addCookie(userId);
  resp.addCookie(name);

  // 5. 응답헤더에 인코딩 및 Content-Type 설정
  resp.setCharacterEncoding("utf-8");
  resp.setContentType("text/html");

  // 6. 응답 헤더에 출력
  PrintWriter out = resp.getWriter();
  String title = "쿠키설정 예제";
  out.println("<!DOCTYPE html>\n"
            + "<html>\n"
            + "  <head>\n"
            + "    <title>" + title + "</title>"
            +	"  </head>\n"
            + "  <body>\n"
            + "    <h1 align=\"center\">" + title + "</h1>\n"
            + "    <ul>\n"
            + "      <li><b>ID</b>: "
            +		   req.getParameter("userId") + "</li>\n"
            +	"	   <li><b>이름</b>:"
            + 	   req.getParameter("name") + "</li>\n"
            + "    </ul>\n"
            + "  </body>\n"
            + "</html>\n");
}
```

## 읽기

```java
private void readCookieExam(HttpServletRequest req, HttpServletResponse resp) throws IOException {

  Cookie cookie = null;

  // 현재 도메인에서 사용중인 쿠키 정보 배열 가져오기
  Cookie cookies [] = req.getCookies();

  // 응답헤더에 인코딩 및 ContentType 지정
  resp.setCharacterEncoding("utf-8");
  resp.setContentType("text/html");

  PrintWriter out = resp.getWriter();
  String title = "쿠키 정보 읽기 예제";

  out.println("<!DOCTYPE html>\n"
             + "<html>\n"
             + "<head><title>" + title + "</title></head>\n"
             + "<body>\n");
  if(cookies != null) {
      out.println("<h2>" + title + "</h2>");
      for (int i = 0; i < cookies.length; i++) {
          cookie = cookies[i];
          out.print("name : " + cookie.getName() +"<br>");
          out.print("value : " + URLDecoder.decode(cookie.getValue(), "utf-8") +"<br>");
          out.print("<br>");
      }
  }else {
      out.println("<h2>쿠키 정보가 없습니다.</h2>");
      out.println("</body");
      out.println("</html");
  }
}
```

## 삭제

```java
private void deleteCookieExam(HttpServletRequest req, HttpServletResponse resp) thrwos IOException {

  Cookie cookie = null;

  // 현재 도메인에서 사용중인 쿠키 정보 배열 가져오기
  Cookies[] cookies = req.getCookies();

  // 응답헤더에 인코딩 및 Content-Type설정
  resp.setCharacterEncoding("utf-8");
  resp.setContentType("text/html");

  PrintWriter out = resp.getWriter();
  String title = "쿠키정보 삭제 예제";

  out.println("<!DOCTYPE html>\n"
            + "<html>\n"
            + "<head><title> + title + </title></head>\n"
            + "<body>\n");

  if (cookies != null) {
      out.println("<h2>" + title + "</h2>");

      for (int i = 0; i < cookies.length; i++) {
          cookie = cookies[i];

          if(cookie.getName().equals("userId") ) {
              // 쿠키 제거하기
              cookie.setMaxAge(0);
              resp.addCookie(cookie);
              out.print("삭제한 쿠키 : " + cookie.getName() + "<br>");
          }
          out.print("name : " + cookie.getName() + ", ");
          out.print("value : " + URLDecoder.decode(cookie.getValue(), "UTF-8"));
      }
  } else {
      out.println("<h2>국희정보가 업습니다.<h2>");
  }

}
```

# 출처

[https://chrisjune-13837.medium.com/web-쿠키-세션이란-aa6bcb327582](https://chrisjune-13837.medium.com/web-%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98%EC%9D%B4%EB%9E%80-aa6bcb327582)

[https://velog.io/@godkimchichi/Java-18-Servlet-쿠키-세션](https://velog.io/@godkimchichi/Java-18-Servlet-%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98)