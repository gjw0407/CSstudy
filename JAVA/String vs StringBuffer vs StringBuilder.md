### 공통점

- Java에서 문자열을 다루는 대표 클래스

### 차이점

- String은 **불변**(immutable), StringBuilder와 StringBuffer는 **가변**(mutable)의 속성을 가진다.

연산 횟수가 많아지거나 멀티 스레드, Race condition 등의 상황이 자주 발생한다면 각 클래스의 특징을 이해하고 상황에 맞는 적절한 클래스를 사용해야 한다.

# String

### 특징

- 한 번 heap 메모리(GC가 동작하는 영역)에 생성되면 할당된 메모리 공간이 변하지 않는다. (불변)
    - 기존 객체가 제거되면 GC가 회수한다.
- 기존 문자열에 concat()이나 + 연산자를 통해 새로운 문자열을 붙이면 새로운 String 객체에 저장하고 참조한다.
- thread-safe하다. 멀티스레드 환경에 사용 가능

### 예시 코드

```java
String str = new String("hello");
str += "world";
```

![image](https://user-images.githubusercontent.com/55528172/181888919-e93c2983-1b8d-4828-9e2f-c58263247214.png)

- 이 경우 String 클래스의 참조변수 `str`이 가리키는 곳에 `hello` 문자열에 `world`가 더해진 `hello world`로 변경된 것으로 착각할 수 있다.
- 하지만 `str`이 `hello world`라는 값을 가진 새로운 메모리 영역을 가리키게끔 변경되고, 처음 `hello`로 할당된 메모리 영역은 garbage로 남아있다가 GC에 의해 사라진다.
- String 클래스는 불변이기 때문에 새로운 인스턴스가 생성된 것

### 사용 예시

- 변하지 않는 문자열을 자주 읽어들이는 경우

### 단점

- 추가, 수정, 삭제 등 연산이 빈번하게 발생하는 알고리즘에 사용하면 heap 메모리에 많은 임시 garbage가 생성되어 힙메모리가 부족해진다.
    - 어플리케이션 성능에 영향

# StringBuffer & StringBuilder

### 특징

- 동일 객체 내에서 문자열을 변경 가능
- 문자열 연산을 할 때 기존 버퍼 크기를 늘리며 유연하게 동작한다.
- 두 클래스가 제공하는 메서드는 동일하다.

### 차이점

- **동기화**의 유무

### 사용 예시

- 추가, 수정, 삭제가 빈번하게 발생할 경우

## StringBuffer

![image](https://user-images.githubusercontent.com/55528172/181888949-94267d23-8878-4458-ab5d-da3328b5e28f.png)

- 동기화 키워드 지원
    - 각 메서드에 synchronized 키워드가 존재
- 멀티스레드 환경에서 안전하다 (thread-safe)
- 동기화 처리로 인해 단일 스레드 환경에서는 StringBuilder에 비해 성능이 좋지 않다.

## StringBuilder

- 동기화를 지원하지 않는다.
    - 멀티스레드 환경에 적합하지 않음
- 단일 스레드에서의 성능은 StringBuffer보다 더 뛰어나다.

## 정리

![image](https://user-images.githubusercontent.com/55528172/181888979-d7a96e13-c0e3-4c02-b7b2-5f67f57181de.png)

- String
    - 짧은 문자열을 더할 경우
    - 문자열 연산이 적고 멀티스레드 환경일 경우
- StringBuffer
    - 스레드에 안전한 프로그램
    - 개발 중인 시스템의 부분이 스레드에 안전한지 모를 경우
    - 문자열 연산이 많고 멀티스레드 환경일 경우
- StringBuilder
    - 스레드에 안전한지 여부에 관계 없는 프로그램
    - 문자열 연산이 많고 단일스레드이거나 동기화를 고려하지 않아도 되는 경우
