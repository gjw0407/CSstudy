# REST API

### 정의

- Representational State Transfer API
- 웹(HTTP)의 장점을 최대한 활용할 수 있는 아키텍처

### 구성

- URI - resource (정보의 자원)
- HTTP Method - verb (자원에 대한 행위)
- Representations (표현)

### 특징

1. Uniform interface
    - 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
2. Self-descriptiveness 자체 표현 구조
    - REST API 메세지만 보고도 쉽게 이해할 수 있는 구조
3. Client-Server 구조
    - 클라이언트: 사용자 인증, 컨텍스트(세션, 로그인 정보) 관리
    - 서버: API 제공
    - 역할 구분, 서로간 의존성 ↓
4. 계층형 구조
    - 서버는 다중 계층으로 구성될 수 있다.
    - 로드 밸런싱, 암호화 계층 / Proxy, 게이트웨이(네트워크 기반 중간 매체)
5. Stateless
    - 상태 정보를 따로 저장하고 관리하지 않는다
6. Cacheable
    - HTTP 웹표준 적용해서 HTTP의 캐싱 기능 적용 가능
    - Last-Modified 태그, E-Tag 이용

### URI 규칙

- 리소스명은 동사보다 명사 사용
- 행위에 대한 표현은 URI가 아닌 HTTP Method로 표현한다.
- `/`는 자원의 계층 관계를 나타낸다.
    - URI의 마지막 문자로 `/`를 사용하지 않는다.
- 불가피하게 긴 URI 경로는 `_` 대신 `-`를 사용하여 가독성을 높인다.
- URI 경로에는 소문자가 적합하다.
- 파일 확장자를 포함시키지 않는다.
- doucment: 한 문서, 객체 / collection: 문서(객체)들의 집합
    - 컬렉션은 복수, 도큐먼트는 단수
    - teams/team-a/members/1
