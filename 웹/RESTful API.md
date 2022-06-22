# RESTful API

### REST의 구성

- 자원(RESOURCE) - URI
- 행위(Verb) - HTTP METHOD (POST, GET, PUT, DELETE, PATCH)
- 표현(Represntations)

### REST의 특징

- Uniform (유니폼 인터페이스): URI로 지정한 리소스에 대한 조직을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말한다.
- Stateless (무상태성): 작업을 위한 상태정보를 따로 저장하거나 관리하지 않는다. 세션정보나 쿠키정보를 별도로 저장하고 관리하지 않아 API 서버는 들어오는 요청만을 단순히 처리하면 된다. 이렇게 되면 서비스의 자유도가 높아지고 구현이 단순해진다.
- Cacheable (캐시 가능): REST의 가장 큰 특징은 HTTP라는 기존 웹표준을 그대로 사용하는것에 있다. 따라서 HTTP가 가진 캐싱 기능이 적용이 가능하다. 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
- Self-descriptiveness (자체 표현 구조): REST의 다른 특징중 하나는 API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것이다.
- Client-Server 구조: REST 서버는 API 제공, 클라이언트는 사용자 인증이나 세션, 로그인 정보등을 직접 관리하는 구조로 역할이 구분되기 때문에 서로간의 의존성이 줄어든다.
- 계층형 구조: REST 서버는 다중 계층으로 구성될 수 있으며, 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고, PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.

### REST API 디자인 가이드 (중요)

REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약이 가능하다.

**첫번째**, URI는 정보의 자원을 표현해야 한다.

**두번째**, 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

### REST API 중심 규칙

1. URI는 정보의 자원을 표현해야 한다. (리소스명은 동사보다는 명사를 사용)

```
GET /members/delete/1
```

이러한 방식은 REST를 제대로 적용하지 않은 URI다. 자원을 표현하는데 중점을 두어야 한다. delete와 같은 행위에 대한 표현은 좋지 않다. 올바르게 수정하면

```
DELETE /members/1
```

으로 수정이 가능하다.

회원정보를 가져올 때에는 GET, 회원 추가 시의 행위를 표현하고자 할 때는 POST METHOD를 사용하여 표현한다.

회원정보를 가져오는 URI

```
GET /members/show/1 (x)
GET /members/1      (o)
```

회원을 추가할 때

```
GET /members/insert/2 (x)
POST /members/2       (o)
```

[참고] HTTP METHOD의 알맞은 역할

- POST: POST를 통해 해당 URI를 요청하면 리소스를 생성한다.
- GET: GET을 통해 해당 리소스를 조회한다. 조회하고 해당 도큐먼트에 대한 자세한 정보도 가져온다.
- PUT: PUT을 통해 해당 리소스를 수정한다.
- DELETE: DELETE를 통해 리소스를 삭제한다.

URI 설계시 주의할 점

1. 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용한다.

```
http://restapi.example.com/animals/mammals/whales
http://restapi.example.com/houses/apartments
```

1. URI 마지막 문자로 슬래시를 포함하지 않는다. URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며, URI가 다르면 리소스가 다르고, 리소스가 다르면 URI가 다르다. 따라서 분명한 URI를 만들어야 하므로, 혼동이 없기 위해 마지막엔 (/)를 사용하지 않는다.
2. 하이픈(-)은 URI 가독성을 높이는데 사용한다. 쉽게 읽고 해석하기 위해, 불가피하게 긴 URI경로를 사용한다면, 하이픈을 사용할 수 있다.
3. 밑줄은 URI에 사용하지 않는다. 보기 어렵거나 문자가 가려지기도 하기 때문에 밑줄대신 하이픈을 사용한다.
4. URI경로에는 소문자가 좋다. 대문자는 피하는게 좋다. 이에 따라 다른 리소스로 인식하기 때문이다. URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문이다.
5. 파일 확장자는 URI에 포함시키지 않는다. Accept hearder를 사용하도록 하자.
6. REST 리소스 간에는 연관 관계가 있을 수 있고 이런 경우 다음과 같이 표현한다.

```
/리소스명/리소스 ID/관계가 있는 다른 리소스명
ex) GET: /users/{userid}/devices (일반적으로 소유 'has'의 관계를 표현할 때)
```

### 자원을 표현하는 Collection과 Document

이에 대해 알면 URI 설계가 한층 쉬워진다. DOCUMENT는 단순히 문서 혹은 객체라고 이해하면 되고, Collection은 문서들의 집합 또는 객체들의 집합이라고 이해하면 된다.

출처: [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)

추가로 보면 좋을 글: [REST 아키텍처를 훌륭하게 적용하기위한 몇가지 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)