# 동일성 vs 동등성

# 동일성

> 두 개의 객체가 완전히 같은 경우
> 

⇒ 두 객체가 사실상 하나의 객체로 봐도 무당 (주소 값도 같음)

Primitive 타입은 객체가 아니라 주소가 없음 → `==` 연산자를 사용했을 때 내용이 같으면 동일

# 동등성

> 두 객체가 같은 정보를 갖고 있는 경우
> 

동일하면 동등하지만, 동등해서 동일하지는 않다

`equals`연산자를 통해 판별

`new` 키워드를 통해 다른 String 객체를 메모리에 할당 

- 객체의 주소 값은 다르므로 동일하지 X
- `equals()` 은 유효

# 예상 질문

1. 동일성 vs 동등성
    1. 동일성 : 메모리 주소가 같음
    2. 동등성 : 객체의 내용이 같음
2. `==` vs equals
    1. == : 객체의 동일성 판별
    2. equals : 동등성 판별 

# 참고

## Primitive

- 실제 값을 저장하는 공간으로 스택 메모리에 저장
- Null이 존재 X
- Ex
    - boolean
    - byte
    - short
    - int
    - long
    - float
    - double
    - char

### Reference

- Null이 존재
- 값이 저장되어 있는 곳의 주소값을 저장하는 공간 → 힙 메모리에 저장
- Ex
    - Array
    - Enumeration
    - Class
    - Interface