## 필요성

- 객체를 파일에 쓰거나 읽어야 하는 경우
- 다른 서버로 객체를 전송하거나 받아야 하는 경우
    
    → JVM의 역할
    

# Serialization

> 자바 시스템 내부에서 사용되는 객체/데이터를 외부의 자바 시스템에서도 사용할 수 있도록 byte 형태 데이터로 변환하고(직렬화), 다시 객체로 변환(역직렬화)하는 기술
> 

## 특징

- JVM의 힙 또는 스택 메모리의 객체 데이터 ↔ 바이트 형태의 데이터
- 객체를 연속적인 데이터로 변형하여 Stream을 통해 데이터를 읽을 수 있게 바꾼다.
- 직렬화와 역직렬화 시 객체들의 순서가 중요하다.
    - 순서를 매번 고려해야 하고 하나라도 맞지 않으면 역직렬화에 실패한다.
    - 같은 객체를 여러번 직렬화하는 것보다 컬렉션 등의 자료구조로 한 번에 넣는 것이 더 효율적이다.

## 직렬화 가능한 클래스

### Serializable interface 구현

1. Serializable 인터페이스를 구현
    
    ```java
    public class Member implements Serializable {...}
    ```
    
2. Serializable 인터페이스를 구현한 클래스를 상속
    
    ```java
    public class Nonmember extends Member {...}
    ```
    

### transient 예약어로 직렬화 대상에서 제외

보안 등의 이유로 특정 멤버변수를 직렬화 대상에서 제외할 수 있다.

```java
public class Member implements Serializable {
    private String id;
    private transient String password;
    ...
}
```

### 멤버변수가 객체인 경우

멤버 변수 중 객체를 타입으로 가지는 경우 모든 클래스가 Serializable 인터페이스를 구현해야 한다.

하나라도 없다면 직렬화가 불가능하다.

```java
public class Member implements Serializable {
    private String id;
    private transient String password;
    private Address address;
    private Calendar registrationDate;
    ...
}
```

## SerialVersionUID

> 직렬화 가능한 클래스의 버전 관리 고유 번호
> 

### 특징

- 기본적으로 내부에서 자동으로 생성하여 관리한다.
- 직렬화 역직렬화 시 SerialVersionUID가 맞는지 확인하고, 맞지 않는다면 InvalidClassException을 일으킨다.
    - 별도로 지정하지 않으면 클래스 변경시 아이디가 변경되기 때문에 예외가 발생한다.
- SerialVersionUID를 멤버 변수로 직접 선언하고 관리하는 방식을 적극 권장한다.
    - static final long 으로 선언
    - 변수명 = serialVersionUID
    - 값은 아무 값이나 지정해주면 된다. = 버전을 명시해주는 것임
    
    ```java
    public class Member implements Serializable {
        private static final long serialVersionUID = 1L;
    
        private String id;
        private transient String password;
        private Address address;
        private Calendar registrationDate;
        ...
    }
    ```
    

### 예시

- HashMap
    
    ```java
    public class HashMap<K,V> extends AbstractMap<K,V>
        implements Map<K,V>, Cloneable, Serializable {
    
        private static final long serialVersionUID = 362498820763181265L;
    
        ...
    }
    ```
    
- 양쪽 서버에서 통신할 때 해당 객체가 같은지 다른지 확인하기 위해 serialVersionUID로 인식, 관리한다.
    - 클래스 이름이 같아도 ID가 다르면 다른 클래스라고 인식한다.
    - 같은 ID여도 변수 개수, 타입이 달라도 다른 클래스라고 인식한다.
