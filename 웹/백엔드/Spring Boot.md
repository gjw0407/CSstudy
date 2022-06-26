# Spring Boot

### 정의

<spring.io/projects/spring-boot>
> Spring Boot makes it easy to create **stand-alone**, **production-grade** Spring based Applications that you can "**just run**".
스프링 부트는 단독적이고, 상용화 수준의 스프링 기반 애플리케이션을 “단지 실행”할 수 있을 정도로 쉽게 만들 수 있다.
>
> We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need **minimal Spring configuration**.
> 


<baeldung.com/spring-vs-spring-boot>
> Spring Boot is basically an **extension of the Spring framework**, which eliminates the boilerplate configurations required for setting up a Spring application.
스프링 부트는 스프링 애플리케이션을 세팅하는데 필요한 보일러플레이트 설정들을 제거한 스프링 프레임워크의 확장이다.
>
> It takes an opinionated view of the Spring platform, which paves the way for a **faster and more efficient development ecosystem.**
개발 환경을 더 빠르고 편리하게 만들어주는 스프링 플랫폼이다.
> 

### 특징

- stand-alone 애플리케이션을 만든다
- 내장 서버Tomcat, Jetty, Undertow을 가지기 때문에 WAR 파일을 배포할 필요 없다.
- 빌드 설정을 간단히 해주는 ‘starter’ 의존성을 제공한다.
- 언제든지 Spring과 3rd party 라이브러리를 자동적으로 설정해준다.
- Metrics, Health checks, and externalized configuration


## vs Spring Framework

### 1. Dependency

1. 최소한의 web application을 구동해야하는 경우의 의존성
    - Spring Framework
        ```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.3.5</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.5</version>
        </dependency>
        ```
    - Spring Boot
        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.4</version>
        </dependency>
        ```
        - 다른 의존성들은 빌드시에 자동적으로 추가된다.

2. testing libraries
    - Spring Framework
        - Spring Test, JUnit, Hamcrest, Mockito 라이브러리를 각자 모두 추가해야 한다.
    - Spring Boot
        - spring-boot-starter 의존성 하나로 자동적으로 모든 라이브러리를 추가해준다.

3. spring-boot starter dependency
    - spring-boot-starter-data-jpa
    - spring-boot-starter-security
    - spring-boot-starter-test
    - spring-boot-starter-web
    - spring-boot-starter-thymeleaf
    - 등..

### 2. Configuration

1. MVC Configuration - JSP web application
    - Spring Framework
        - dispatcher servlet, mapping 등의 설정을 web.xml이나 Initializer.class에 해주어야 한다.
        ```java
        public class MyWebAppInitializer implements WebApplicationInitializer {
         
            @Override
            public void onStartup(ServletContext container) {
                AnnotationConfigWebApplicationContext context
                  = new AnnotationConfigWebApplicationContext();
                context.setConfigLocation("com.baeldung");
         
                container.addListener(new ContextLoaderListener(context));
         
                ServletRegistration.Dynamic dispatcher = container
                  .addServlet("dispatcher", new DispatcherServlet(context));
                 
                dispatcher.setLoadOnStartup(1);
                dispatcher.addMapping("/");
            }
        }
        ```
        ```java
        @EnableWebMvc
        @Configuration
        public class ClientWebConfig implements WebMvcConfigurer { 
           @Bean
           public ViewResolver viewResolver() {
              InternalResourceViewResolver bean
                = new InternalResourceViewResolver();
              bean.setViewClass(JstlView.class);
              bean.setPrefix("/WEB-INF/view/");
              bean.setSuffix(".jsp");
              return bean;
           }
        }
        ```
    - Spring Boot
        - 아래 property만 application.properties나 application.yml에 추가
        - 스프링 부트가 애플리케이션에 있는 dependencies, properties, beans을 보고 자동적으로 **auto-configuration** 수행
        ```xml
        spring.mvc.view.prefix=/WEB-INF/jsp/
        spring.mvc.view.suffix=.jsp
        ```
        

2. Spring Security Configuration
    - 공통
        
        ```java
        @Configuration
        @EnableWebSecurity
        public class CustomWebSecurityConfigurerAdapter extends WebSecurityConfigurerAdapter {
         
            @Autowired
            public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
                auth.inMemoryAuthentication()
                  .withUser("user1")
                    .password(passwordEncoder()
                    .encode("user1Pass"))
                  .authorities("ROLE_USER");
            }
         
            @Override
            protected void configure(HttpSecurity http) throws Exception {
                http.authorizeRequests()
                  .anyRequest().authenticated()
                  .and()
                  .httpBasic();
            }
            
            @Bean
            public PasswordEncoder passwordEncoder() {
                return new BCryptPasswordEncoder();
            }
        }
        ```
    - Spring Framework
        - spring-security-web, spring-security-config 의존성 필요
    - Spring Boot
        - spring-boot-starter-security 의존성이 자동으로 모든 관련 의존성들을 클래스패스에 추가

### 3. Bootstrap

기본적인 차이는 servlet에 있다.

- Spring Framework
    - web.xml이나 SpringServletContainerInitializer이 entry point이다.
- Spring Boot
    - `public static void main()`이 내장 웹 서버를 실행시키기 위한 entry point이다.
        ```java
        @SpringBootApplication
        public class Application {
            public static void main(String[] args) {
                SpringApplication.run(Application.class, args);
            }
        }
        ```
    - application context의 Servlet, Filter, ServletContextInitializer 빈들을 내장 서블릿 컨테이너에 바인딩시켜준다.
    - Main 클래스 이하 패키지의 컴포넌트를 자동적으로 스캔한다.

### 4. Packaging and Deployment

- Spring Framework
    - war 파일을 Web Application Server에 담아 배포
- Spring Boot
    - 내장 컨테이너(WAS) 지원 → jar 파일로 간편하게 배포 가능
    - `java -jar` 명령어로 독립적으로 jar를 실행시킬 수 있다.
    - 외부 컨테이너에서 배포할 때 jar 충돌을 피하기 위해 의존성을 제외할 수 있다.
    - 배포할 때 활성화된 프로파일(active profiles)을 지정할 수 있다.
    - 통합 테스트에서 랜덤 포트 사용
