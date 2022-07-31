## Garbage

유효하지 않은 메모리

```java
Person person = new Person("Jiho");
person = null;

person = new Person("Wonsang");  // garbage 발생
```

처음에 “Jiho”라는 이름으로 생성된 객체는 더 이상 참조와 사용을 하지 않기 때문에 가비지가 되었다.

# Garbage Collection

> Java의 메모리 관리 방법 중 하나
Heap 영역에 동적으로 할당했던 메모리 영역 중 필요없어진 메모리 영역을 주기적으로 삭제하는 프로세스
> 

- C언어의 경우 free() 함수로 직접 메모리를 해제해주어야 하지만
- Java나 Kotlin의 경우 **JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리**해주기 때문에 개발자가 메모리를 직접 해제해주지 않는다.
- **메모리 누수를 방지**하기 위해 Garbage Collector(GC)가 주기적으로 검사하여 메모리를 청소해준다.

## 용어

- Reachable: 객체가 참조되고 있는 상태
- Unreachable: 객체가 참조되고 있지 않은 상태
    - GC의 대상이 된다.

![image](https://user-images.githubusercontent.com/55528172/181889813-8d35b839-4c3d-4b76-80f8-899e3c5d5136.png)

# JVM의 Heap 메모리

### Weak Generational Hypothesis

힙 영역은 처음 설계될 때 2가지를 전제로 설계되었다.

1. 대부분의 객체는 금방 불가능한 상태(Unreachable)가 된다.
2. 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

= 객체는 대부분 일회성이며 메모리에 오랫동안 남아있는 경우는 드물다.

객체의 생존 기간에 따라 물리적인 heap 영역을 Young, Old 두가지로 나누었다.

![image](https://user-images.githubusercontent.com/55528172/181889696-b10279b2-130d-4819-b307-ee546bdc0a5e.png)

1. Young Generation
    - 새롭게 생성된 객체가 할당되는 영역
    - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에 많은 객체가 young 영역에 생성되었다가 사라진다.
    - Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.
2. Old Generation
    - Young 영역에서 Reachable 상태를 유지해서 살아남은 객체가 복사되는 영역
    - Young 영역보다 크게 할당되고, 영역 크기가 큰 만큼 가비지는 적게 발생한다.
        - 수명이 짧은 young 영역의 객체들은 큰 공간이 필요 없음
    - Old 영역에 대한 가비지 컬렉션을 Major GC 혹은 Full GC라고 부른다.

# GC 동작 방식

## 공통

Young, Old 영역은 서로 다른 메모리 구조로 되어있기 때문에 가비지 컬렉션이 실행될 때 세부 동작은 다르지만, 아래 2가지 공통적인 단계를 따른다.

1. Stop The World
2. Mark and Sweep

### 1. Stop The World

- JVM이 어플리케이션의 실행을 멈추는 작업
- GC를 실행하는 스레드를 제외한 모든 스레드의 작업이 중단되고, GC가 완료되면 작업이 재개된다.
- 일반적으로 ‘GC의 성능 개선 = stop the wrold의 시간을 줄이는 작업’이다.

### 2. Mark and Sweep

1. Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
    - 스택의 모든 변수, Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지 탐색한다.
2. Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

# 단점

1. 개발자가 메모리가 언제 해제되는지 정확하게 알 수 없다.
2. 가비지 컬렉션이 동작하는 동안 다른 동작을 멈추기 때문에 오버헤드가 발생한다.
    - 너무 자주 실행되면 소프트웨어 성능이 하락할 수 있다.
