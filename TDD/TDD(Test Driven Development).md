# TDD(Test Driven Development)

# 개념

> 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복해 구현
> 

협력 + 피드백

- ‘어떻게 해야 피드백을 더 자주 받을까’
- ‘어떻게 해야 내가 하는 작업에 대해 협력이 잘 일어나게 할까’

eXtream Programming(XP)의 Test-First 개념에 기반을 둔 단순한 설계를 중요시

- eXtream Programming(XP) : 미래에 대한 예측을 하지 않고 지속적으로 프로토타입을 완성하는 애자일 기반방법

# 개발 주기

![Untitled](https://user-images.githubusercontent.com/61227459/176461918-77e6071b-a8e0-4bf7-afb4-d36c3b1a1647.png)

1. 실패하는 테스트 코드 작성 (RED)
    1. 실패 테스트가 없을 경우에만 성공 테스트를 작성
2. 성공 시 실제 코드 작성 (GREEN)
3. 중복 코드 제거, 일반화, 리팩토링 (BLUE)

# 왜

- 소비자의 요구 사항이 명확하지 않을 수 있음 → 설계에 문제가 생김
- 깔끔한 코드 작성
- 개발 비용 절감
    - 개발을 처음부터 다시 시작하는 경우가 줄어듦
- 개발 이후 테스트 코드 작성은 어려움(귀찮음)

# 비교

## 일단 개발 방식

요구사항 분석 → 설계 → 개발 → 테스트 → 배포

![Untitled 1](https://user-images.githubusercontent.com/61227459/176461806-147315fb-4525-4037-b509-3781de31f375.png)

## TDD 기반 개발 방식

![Untitled 2](https://user-images.githubusercontent.com/61227459/176461853-9c2a082b-3e70-4911-9d9e-37037caa79ab.png)

# 장점

1. 객체지향적인 코드 개발
2. 설계 수정 시간의 단축
3. 디버깅 시간 단축
4. 유지 보수 용이
    1. 내가 짠 코드를 남이 본다고 생각
5. 테스트 문서의 대체 가능
    1. SI 프로젝트 진행 과정에서 테스트 정의서를 만듦 (통합 테스트 문서)

# JAVA

## Tool

- JUnit
- AssertJ

## Spring

Repository → Service → Controller 

- Repository : H2와 같은 인메모리 DB 기반의 통합 테스트로 진행
- Service : Mockito를 사용해 Repository 계층을 Mock하여 진행
- Controller : SpringTest의 MockMvc
- Repository가 다른 계층에 대한 의존성이 거의 없기 때문에 편리 (개인적 의견)

Controller → Service → Repository 순으로 하는 경우도 있음

### Mockito

> 개발자가 동작을 직접 제어할 수 있는 가짜(Mock) 객체를 지원하는 테스트 프레임
> 

Spring에서는 의존성 때문에 단위 테스트 하는데 어려움 → Mock(가짜) 객체를 주입

### 예시

### Controller Test

```java
@RestController
@RequiredArgsConstructor
public class UserController {

   private final UserService userService;

   @PostMapping("/users/signUp")
   public ResponseEntity<UserResponse> signUp(@RequestBody SignUpRequest request) {
       return ResponseEntity.status(HttpStatus.CREATED)
          .body(userService.signUp(request));
   }

   @GetMapping("/users")
   public ResponseEntity<List<UserResponse>> findAll() {
       return ResponseEntity.ok(userService.findAll());
   }
}
```

```java
// JUnit5와 Mockito 연동
@ExtendWith(MockitoExtension.class)
class UserControllerTest {
	// 가짜 객체 주입
   @InjectMocks
   private UserController userController;
	// 가짜 객체 생성 (@Autowired 대체)
   @Mock
   private UserService userService;
	// HTTP(Controller 테스트 할때 필요) 호출 대신
   private MockMvc mockMvc;
	// 실제 메서드 사용할 시
	@Spy
  private BCryptPasswordEncoder passwordEncoder;

   @BeforeEach
   public void init() {
       mockMvc = MockMvcBuilders.standaloneSetup(userController).build();
   }
}
```

```java
@DisplayName("회원 가입 성공")
@Test
void signUpSuccess() throws Exception {
   // given
   SignUpRequest request = signUpRequest();
   UserResponse response = userResponse();
  
   doReturn(response).when(userService)
      .signUp(any(SignUpRequest.class));

	// when
   ResultActions resultActions = mockMvc.perform(
       MockMvcRequestBuilders.post("/users/signUp")
               .contentType(MediaType.APPLICATION_JSON)
               .content(new Gson().toJson(request))
   );

	// then
   MvcResult mvcResult = resultActions.andExpect(status().isOk())
      .andExpect(jsonPath("email", response.getEmail()).exists())
      .andExpect(jsonPath("pw", response.getPw()).exists())
      .andExpect(jsonPath("role", response.getRole()).exists())
}

private SignUpRequest signUpRequest() {
  return SignUpRequest.builder()
      .email("test@test.test")
       .pw("test")
      .build();
}

private UserResponse userResponse() {
  return UserResponse.builder()
      .email("test@test.test")
       .pw("test")
      .role(UserRole.ROLE_USER)
      .build();
}
```

### Repository Test

```java
@DataJpaTest
class UserRepositoryTest {
  @Autowired
  private UserRepository userRepository;

  @DisplayName("사용자 추가")
  @Test
  void addUser() {
      // given
      User user = user();
      
      // when
      User savedUser = userRepository.save(user);

      // then
      assertThat(savedUser.getEmail()).isEqualTo(user.getEmail());
      assertThat(savedUser.getPw()).isEqualTo(user.getPw());
      assertThat(savedUser.getRole()).isEqualTo(user.getRole());
  }

  private User user() {
      return User.builder()
              .email("email")
              .pw("pw")
              .role(UserRole.ROLE_USER).build();
  }
}
```

# JavaScript

## Tool

- Jest
- Mocha

## 함수

- 진실성(Truthiness)
    - `toBeNull` → null
    - `toBeUndefined` → undefined
    - `toBDefined` → `toBeUndefined` 반대
    - `toBeTruthy` → if is true
    - `toBeFalsy` → if is false
    - `toBeNull` → null
    - `toBeUndefined` → undefined
    - `toBDefined` → `toBeUndefined` 반대
    - `toBeTruthy` → if is true
    - `toBeFalsy` → if is false
- 숫자
    - `toBeGreaterThan`
    - `toBeGreaterThanOrEqual`
    - `toBeLessThan`
    - `toBeLessThanOrEqual`
    - `toBeCloseTo`
- String
    - `toMatch`
- Array
    - `toContain`
- Exception
    - `toThrow`

## 예시

```java
function sum(a, b) {
    return a + b;
}

function sumOf(numbers){
    let result = 0;
    numbers.forEach(n => {
        result += n;
    });
    return result;
}

// 내보내기
// module.exports = sum; 

// 각각 내보내기
exports.sum = sum;
exports.sumOf = sumOf;
```

```java
const { sum, sumOf } = require('./sum');

describe('sum', () => {
    it('calculates 1 + 2', () => {
        expect(sum(1, 2)).toBe(3);
    });

    it('calculates all numbers', () => {
        const array = [1, 2, 3, 4, 5];
        expect(sumOf(array)).toBe(15);
    })
})
```

# 출처

[https://mangkyu.tistory.com/182](https://mangkyu.tistory.com/182)

[http://clipsoft.co.kr/wp/blog/tddtest-driven-development-방법론/](http://clipsoft.co.kr/wp/blog/tddtest-driven-development-%EB%B0%A9%EB%B2%95%EB%A1%A0/)

[https://wooaoe.tistory.com/33](https://wooaoe.tistory.com/33)

[https://m.blog.naver.com/suresofttech/221569611618](https://m.blog.naver.com/suresofttech/221569611618)

[https://hanamon.kr/tdd란-테스트-주도-개발/](https://hanamon.kr/tdd%EB%9E%80-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A3%BC%EB%8F%84-%EA%B0%9C%EB%B0%9C/)

[https://gmlwjd9405.github.io/2018/06/03/agile-tdd.html](https://gmlwjd9405.github.io/2018/06/03/agile-tdd.html)

[https://mangkyu.tistory.com/145](https://mangkyu.tistory.com/145)

[https://cho-coding.tistory.com/231](https://cho-coding.tistory.com/231)

[https://dev.to/pat_the99/basics-of-javascript-test-driven-development-tdd-with-jest-o3c](https://dev.to/pat_the99/basics-of-javascript-test-driven-development-tdd-with-jest-o3c)