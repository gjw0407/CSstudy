# Spring Framework

### Framework

> 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것
> _ Ralph Johnson

1. 구조 품질을 보장한다.
    - Design Pattern + Class Library
        - 설계자가 의도하는 여러 디자인 패턴의 집합으로 구성되어 있다.
    - 구체적이며 확장 가능한 기반 코드를 가진다.
2. 반제품
    - 애플리케이션의 틀과 구조를 결정한다.
    - Framework의 코드가 그 위에 개발된 개발자의 Application 코드를 호출하고 제어한다.
    - 개발자는 핵심 비즈니스 로직에만 집중할 수 있다.
3. 생산성과 코드 품질 향상

## Spring (Framework)

### 정의

> 자바 엔터프라이즈 개발을 편하게 해주는 / 경량급  / 오픈소스 / 애플리케이션 프레임워크
> 
- 오픈소스: Apache License 2.0을 따르는 오픈소스 프로젝트
- 경량급: 단순한 개발툴과 가볍고 기본적인 개발환경으로도 엔터프라이즈 개발에서 필요로 하는 주요 기능을 갖춘 애플리케이션을 개발하기에 충분하다.
    - 작은 규모의 코드로 이루어졌다는 뜻이 아니다.
- 자바 엔터프라이즈 개발을 편하게 해주는: 대규모 데이터 처리와 트랜잭션이 동시에 행해지는 매우 큰 규모의 자바 애플리케이션을 개발할 때 low level 코드에 신경쓸 필요 없이 핵심 로직에 집중하게 도와준다.
- 애플리케이션 프레임워크: 개발하는데 필요한 구조를 포괄적으로 제공한다.

### POJO (Plain Old Java Object)

- 오래된 방식의 간단한 자바 오브젝트
- 특정 규약, 특정 환경에 종속되지 않고 객체지향 원리에 충실하다.
- 단순한 자바 객체에 도메인 로직을 넣어서 사용

### Spring Bean

- Spring IoC Container가 관리하는 **자바 객체**
    - `new` 키워드로 생성한 객체는 bean이 아니다
    - ApplicationContext가 만들어서 그 안에 담고 있는 객체
- bean scope
    - bean이 관리되는 범위
    - 기본 전략 = singleton
    - 애플리케이션 전반에 걸쳐 빈의 인스턴스를 오직 하나만 생성해서 사용
- `@Component` 애노테이션이 붙은 객체

**bean 등록 방법**

1. `@Component`가 붙은 클래스들을 Component Scan으로 자동 등록
2. Configuration 클래스에서 `@Bean`으로 직접 빈 등록

### Spring Container

- 개발자 대신해서 스프링 빈을 생성하고 관리하는 컨테이너 = Inversion of Control (IoC)
- 객체의 제어권이 개발자에게서 스프링으로 넘어갔다.
- 각 객체의 생성, 소멸과 같은 라이프 사이클을 관리하며 필요한 객체를 컨테이너로부터 얻어올 수 있다.
- Component Scan으로 bean을 `@Component`가 붙은 클래스들의 객체를 스프링 빈으로 컨테이너에 넣고 관리


## Spring 7 Modules

아래 모듈들은 엔터프라이즈 애플리케이션을 개발하기 위해 다른 플랫폼을 제공한다.

![spring-modules](https://user-images.githubusercontent.com/55528172/174765998-2b9fd445-5c18-4584-8efd-50b71b98a25b.png)

- Spring Core
    - IoC 컨테이너를 제공하는 spring framework이 핵심 컴포넌트
    - BeanFactory은 빈 객체 관리, 의존성 주입 등의 역할을 하는 컨테이너이다.
- Spring Context
    - Core 모듈을 기반으로 하며 추가적인 기능들과 더 쉬운 개발이 가능하도록 지원한다.
        - Email, JNDI접근, EJB 연계등과 같은 다수의 엔터프라이즈 서비스 제공
    - Spring을 container로 만든 것이 Core 모듈의 BeanFactory이고, Spring을 framework로 만든 것이 Context 모듈이다.
- Spring AOP
    - 프로그램을 aspect(관점)이나 concern(관심사)로 나눈다.
- Spring ORM
    - Hibernate, Apache Ibatis 등의 ORM 프레임워크와의 매우 우수한 통합 환경 제공
    - 데이터베이스를 접근, 조작할 수 있는 API를 제공한다.
- Spring Web MVC
    - web application을 만들기 위한 MVC 구조
    - JSP, Velocity, Excel, PDF 같은 다양한 UI 기술들을 사용하기 위한 API 제공
    - form controller: SimpleFormController.class, AbstractWizardFormController.class
- Spring Web Flow
    - Spring Web MVC 모듈의 확장
    - web application의 다른 페이지들간의 workflow를 관리한다.
- Spring DAO(Data Access Object)
    - Spring JDBC DAO 추상 레이어는 다른 데이터베이스 vendor들의 예외 핸들링과 오류 메세지를 관리하는 예외 계층을 제공한다.
    - 일반적으로 많이 사용해왔던 JDBC 기반 DAO 개발을 적은 코드와 쉽고 일관된 방법으로 개발하는 것이 가능하다.
