# Spring Triangle

![image](https://user-images.githubusercontent.com/55528172/174726226-b77b383a-4619-4be0-ae6a-49926a26e086.png)

1. Inversion of Control / Dependency Injection (제어의 역전 / 의존성 주입)
2. Aspect Oriented Programming (관점 지향 프로그래밍)
3. Portable Service Abstraction (일관성 있는 서비스 추상화)


## 1. Inversion of Control/Dependency Injection (IoC/DI)

Spring Framework는 개발자 대신 직접 객체인 스프링 빈을 관리하고(제어의 역전), 클래스들간의 의존 관계가 맺어지는 것을 도와준다(의존성 주입).

### 예시

`PostService`가 `PostRepository`를 의존(사용)하는 경우

1. 개발자가 객체를 생성하고 사용한다

```java
@Service
public class PostService {
	private final PostRepository postRepository = new PostRepository();

	public void test() {
		postRepository.findById(1L);
	}
}
```

1. 스프링이 객체를 생성/관리하고, `@Autowired`를 통해 객체를 주입받아 의존성을 만든다.

```java
@Repository
public class PostRepository {
}
```

```java
@Service
public class PostService {
	private final PostRepository postRepository;

	@Autowired
	public PostService(PostRepository postRepository) {
		this.postRepository = postRepository;
	}
}
```

### 의존성 주입 방법

1. field 주입
    
    ```java
    @Service
    public class PostService {
    	@Autowired
    	private final PostRepository postRepository;
    }
    ```
    
2. setter 주입
    
    ```java
    @Service
    public class PostService {
    	private final PostRepository postRepository;
    
    	public PostService() {
    	}
    
    	@Autowired
    	public void setPostRepository(PostRepository postRepository) {
    		this.postRepository = postRepository;
    	}
    }
    ```
    
3. constructor 주입 → 권장
    - 처음 애플리케이션이 실행되는 시점에 의존성이 한 번에 주입되고 끝난다.
    - 생성자가 1개만 있으면 `@Autowired`는 생략 가능하다.


## 2. Aspect Oriented Programming (AOP)

- 로직이나 기능을 하나의 **aspect(관점)**으로 보고 이 관심사을 **각각 모듈화**하여 프로그래밍한다.
- 부가 기능을 핵심 비즈니스 로직으로부터 분리하여 하나의 별도의 모듈로 만들고 필요한 곳에 조립하여 사용한다.
- Transaction, Logging, Security 같은 여러 모듈에서 공통적으로 사용하는 기능
- 코드를 단순하고 깔끔하게 작성할 수 있다.

![image](https://user-images.githubusercontent.com/55528172/174726314-56b134f5-c5d6-4704-9ca3-eff7c28c2c3e.png)

### 예시

1. 트랜잭션 `@Transactional` 어노테이션

```java
Connection connection = dataSource.getConnection();

public sendMoney() {
	try (connection) {

		connection.setAutoCommit(false);

		// business logic

		connection.commit();

	} catch (SQLException e) {
		connection.rollback();
	}
}
```

```java
@Transactional
public sendMoney() {
	// business logic
}
```

1. 메서드 실행시간 재기
    모든 메서드의 시작과 끝에 시간, 로깅 관련 코드를 삽입해주어야하지만 별도의 애너테이션으로 분리하여 각 메서드가 중복 코드 없이 핵심 로직만을 가지고 할 수 있다.
    

## 3. Portable Service Abstraction (PSA)

- Service Abstraction: 추상화 계층을 사용해 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공하는 것
- PSA: service abstraction으로 제공되는 기술을 **다른 기술 스택으로 간편하게 바꿀 수 있는 확장성**을 가졌다.
- 특정 기술에 직접적 영향을 받지 않도록 스프링에서 동작하는 라이브러리들은 **POJO 원칙**을 기반으로 추상화된 layer를 가진다.
    - 서로 다른 외부 라이브러리들에 대해 스프링은 **동일한 인터페이스**로 동일한 구동이 가능하게끔 설계되어서 의존성을 고려할 필요가 없다.

![image](https://user-images.githubusercontent.com/55528172/174726369-99c3777f-9ea8-4c63-a87d-98912934f672.png)
### 예시

1. Spring Web MVC
    - servlet을 로우 레벨에서 직접 구현할 필요가 없다. HttpServlet의 `doGet()`, `doPost()` 대신 `@GettMapping`, `@PostMapping`을 통해 요청을 처리한다.
    - 로직 변경 없이 web에서 webflux로 의존성만 바꿈으로써 Tomcat이 아닌 netty 로 서버를 실행할 수 있다.
2. Spring Transaction
    - `commit()`, `rollback()`을 명시적으로 호출하지 않아도 어노테이션을 붙이면 트랜잭션 처리가 이루어진다.
    - 기존 코드 변경 없이 트랜잭션을 처리하는 구현체를 JDBC, JPA, Hibernate 등으로 유연하게 바꿀 수 있다.
3. Spring Cache
    - `@Cacheable`
