# JWT

# 개념

> JSON 포맷을 이용해 사용자에 대한 속성을 저장하는 Claim(정보의 조각) 기반의 Web Token
> 

클라이언트(사용자 PC)에 저장 → 서버에 부담이 적음

## 종류

1. Access Token
    1. 유효기간을 짧게
2. Refresh Token
    1. 유효기간을 길게

<aside>
💡 사용자는 Access Token으로 요청(유효기간 만료 시) → Refresh Token을 서버에 보냄 → 새로운 Access Token 발급

</aside>

## Claim

1. 등록된 클레임
    1. 서비스에서 필요한 정보 X, 정보 저장을 위해 이미 정해진 클레임
    
    | iss | 토큰 발급자(issuer) |
    | --- | --- |
    | sub | 토큰 제목(subject) |
    | aud | 토큰 대상자(audience) |
    | exp | 토큰 만료 시간(expiration), NumericDate 형식 ex)20220622 |
    | nbf | 토큰 활성 날짜(not before), 이 날이 지나기 전의 토큰은 활성화 X |
    | lat | 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음 |
    | jti | JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용, 일회용 토큰 등에 사용 |
2. 공개 클레임
    1. 충동일 방지 된 (collision-resistant) 이름을 가지고 있어야 함
    2. 클레임 이름을 URI 형식으로 지음
3. 비공개 클레임
    1. 클라이언트와 서버가 합의하에 클레임을 지정해서 사용

# 구조

![Untitled](JWT%2040433fbf32e544aab879ae198a63f595/Untitled.png)

## 헤더 (Header)

- typ : 토큰의 타입을 지정
- alg : 서명 및 토큰 검증에 쓰이는 알고리즘 방식

## 내용 (Payload)

- BASE64Url로 인코딩
- JWE(JSON Web Encryption)로 암호화하거나 Payload에 중요 데이터를 넣지 않아야 함
    - JWE : JSON 및 Base64를 기반으로 암호화된 데이터 교환을 위한 표준화된 구문을 제공하는 IETF 표준

## 서명 (Signature)

- 비밀키를 포함하여 암호화 되어 있음
- 인코딩, 유효성 검증 때 사용하는 고유한 암호화 코드
- 헤더, 페이로드의 값을 Base64로 인코딩 → 비밀 키를 이용해 헤더에서 정의한 알고리즘으로 해싱 → 다시 Base64로 인코딩

# 출처

[https://mangkyu.tistory.com/56?category=925341](https://mangkyu.tistory.com/56?category=925341)

[https://dev-coco.tistory.com/103](https://dev-coco.tistory.com/103)