# Session

# 특징

- 웹 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장
- 클라이언트 구분하기 위해 Session ID 부여
- 상태 정보를 서버에 저장
- 브라우저 닫으면 자동으로 세션 삭제
- 과도한 사용은 서버 과부하 → 속도 느려짐

# 동작 순서

1. 클라이언트가 페이지에 요청
2. 클라이언트의 Request-Header 필드인 Cookie를 확인 후 클라이언트가 session-id를 보냈는지 확인
3. Session-id가 없으면 생성 후 응답
    1. 서버에 session-id 저장, 클라이언트에게 쿠키로 전달
4. 클라이언트 재접속 시, 이 쿠키를 이용해 session-id 값을 서버에 전달

# 코드

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  // 세션을 가져오는 데 없으면 새로 생성한다
  HttpSession session = req.getSession(true);

  // 세션 삭제
//session.invalidate(); // 1번 방법
//session.setMaxInactiveInterval(20); // 2번 방법: 20초
  // 생성시간 가져오기
  Date createTime = new Date(session.getCreationTime());

  // 마지막 접근시간 가져오기
  Date lastAccessTime = new Date(session.getLastAccessedTime());

  String title = "재방문을 환영하니다.";
  int visitCount = 0; // 방문횟수
  String userId = "chichi";

  if (session.isNew()) {
    title = "처음 방문을 환영합니다.";
    session.setAttribute("userId", userId); // 키값, 밸류값
  }else {
    visitCount = (Integer)session.getAttribute("visitCount"); // 세션에 있는 기존 값
    visitCount++;
    userId = (String) session.getAttribute("userId");
  }

  System.out.println("방문횟수 : " + visitCount); // 서버단에서 한번 콘솔에 찍어본것 화면과 상관x
  session.setAttribute("visitCount", visitCount); // 세션에 ++된 방문수 저장

  // 응답헤더 html 코드 출력
  // 생략
}
```

# 출처

[https://chrisjune-13837.medium.com/web-쿠키-세션이란-aa6bcb327582](https://chrisjune-13837.medium.com/web-%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98%EC%9D%B4%EB%9E%80-aa6bcb327582)

[https://dev-coco.tistory.com/61](https://dev-coco.tistory.com/61)