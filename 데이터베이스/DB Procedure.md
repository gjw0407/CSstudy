# DB Procedure

# Stored Procedure

- 절차형 SQL을 활용하여 특정 기능을 수행 할 수 있는 트랜잭션 언어
- 쿼리문의 집합
- 첫 실행시 컴파일 (재컴파일 안함)

# 구성

| 구성요소 | 설명 |
| --- | --- |
| 선언부(DECLARE) | 프로시저의 명칭, 변수와 인수 그리고 그에 대한 데이터 타입을 정의 |
| 시작/종료부(BEGIN/END) | 프로시저의 싲가과 종료를 표현하며, BEGIN/END가 쌍을 이룸 / 다수 실행을 제어하는 기본적 단위가 되며 논리적 프로세스를 구성 |
| 제어부(CONTROL) | 기본적으로는 순차적으로 처리 / 조건문과 반복문을 이용하여 문장을 처리 |
| SQL | DML을 주로 사용 / 자주 사용되지 않지만 DDL 중 TRUNCATE 사용 |
| 예외부(EXCEPTION) | BEGIN~END 절에서 실행되는 SQL문이 실행될 때 예외 발생 시 예외 처리 |
| 실행부(TRANSACTION) | 트리거에서 수행된 DML 수행 내역의 DBMS의 적용 또는 취소 여부를 결정 |

# 예시

```graphql
CREATE OR REPLACE procedure name 
   IN argument 
   OUT argument 
   IN OUT argument 
 
IS 
   [변수의 선언]
 
BEGIN  --> 필수 
 
   [PL/SQL Block] 
   -- SQL문장, PL/SQL제어 문장 
 
   [EXCEPTION]  --> 선택
  -- error가 발생할 때 수행하는 문장
 
END;  --> 필수
```

# 호출

```java
EXECUTE 프로시저_명(파라미터_명1, 파라미터_명2, ...);
```

# 장점

- DB에 바로 업데이트 가능
- 모듈식으로 저장 후 언제든 실행 가능
- 모듈식 프로그래밍
- 네트워크 전송량 감소

# 단점

- 문자나 숫자 연산 사용 시 느림
- 디버깅 및 유지보수가 어려움
- DB 확장이 어려움

# 출처

[https://benggri.tistory.com/76](https://benggri.tistory.com/76)