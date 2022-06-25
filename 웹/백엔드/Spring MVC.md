# Spring MVC

## MVC (Model-View Controller)

- 사용자 인터페이스, 데이터 및 논리 제어를 구현하는데 널리 사용되는 소프트웨어 디자인 패턴
- 비즈니스 로직과 화면을 구분한다. (관심사 분리)

![mvc](https://user-images.githubusercontent.com/55528172/174748624-87e70490-3c36-4cfa-886b-df1d864b1c4c.png)

1. Model: 데이터와 비즈니스 로직 관리
2. View: 레이아웃, 화면 처리
3. Controller: 명령을 model과 view 부분으로 라우팅
    - 입력에 대한 응답으로 모델 또는 뷰를 업데이트하는 로직

### MVC 1

![mvc1](https://user-images.githubusercontent.com/55528172/174754676-4a4ecaca-e59c-4807-99d7-bd22b70dfcdf.png)

- JSP가 웹 브라우저의 요청을 받고(control), 빈(model)에서 비즈니스 로직을 처리한 후, 결과 페이지(view)를 반환하는 CV 코드를 모두 가지고 있음
- 가독성, 재사용성, 유지보수성이 모두 떨어진다.

### MVC 2

![mvc2](https://user-images.githubusercontent.com/55528172/174754688-3fb07de2-607d-46dd-89a5-694844b1fb2e.png)

- MVC 1 모델에 Servlet(Controller) 추가
- 브라우저의 요청을 servlet이 받는다.
- 서블릿은 요청을 처리한 후 결과를 JSP 페이지로 포워딩
- 요청, 응답, 비즈니스 로직 처리를 분리하여 유지보수와 확장이 용이하다.

## Spring MVC

MVC 2 모델 기반으로 만들어짐

![spring mvc](https://user-images.githubusercontent.com/55528172/174758623-86580c62-d887-4fa9-95c9-073a4e0a9422.png)

```
동작 순서: 
요청 → 프론트 컨트롤러 → 핸들러 매핑 → 핸들러 어댑터 → 컨트롤러 → 로직 수행 (서비스) → 컨트롤러 → 뷰 리졸버 → 응답(jsp, html)
```

1. Model
    - POJO로 구성
    - Java Beans
2. View
    - Model data의 렌더링, HTML output
    - JSP(Java Server Pages), Thymeleaf, Groovy, Freemarker 등 template engine
3. Controller
    - 사용자 입력 및 요청을 수신하여 적절한 결과를 Model에 담아 View에 전달

### 흐름도

![spring mvc flow](https://user-images.githubusercontent.com/55528172/174766612-eb89eb0b-8b26-4f73-9633-085b26d26e2e.png)

1. `DispatcherServlet`
    - Front Controller의 역할
    - Spring Framework가 제공하는 Servlet 클래스
    - 사용자의 모든 요청을 받아서 `HandlerMapping`으로 넘긴다.
    - 요청에 대한 선처리 작업 수행: 지역 정보 결정, 멀티파트 파일 업로드 처리
    - web.xml이나 org.springframework.web.WebApplicationInitializer 인터페이스를 구현하여 디스패처서블릿을 프론트 컨트롤러로 설정할 수 있다.
2. `HandlerMapping`
    - 요청을 처리할 `Controller`를 탐색한다. (controller url mapping)
        - 요청 url에 해당하는 controller 정보를 저장한 table을 가진다.
        - url 요청이 들어왔을 경우 테이블에서 `@Controller`가 붙은 해당 클래스와 `@RequestMapping("/url")`이 붙은 메서드를 찾아 mapping한다.
3. `HandlerAdapter`
    - 매핑된 컨트롤러를 실제로 실행시킨다.
4. `Controller`
    - 해당 요청을 처리하는 로직
    - view name과 관련 model 데이터를 포함한 처리 결과 `ModelAndView` 객체를 `DispatcherServlet`에게 반환한다.
5. `ViewResolver`
    - 컨트롤러가 반환한 view name(logical name)에 prefix, suffix를 적용하여 View Object(physical view file)를 반환한다.
        - view name = home → file = /WEB-INF/view/home.jsp
6. `View`
    - 로직 처리 결과를 반영한 최종 화면을 생성한다.
