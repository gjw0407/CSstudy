# Synchronized

# 자바 메모리

- static
- heap
- stack

멀티 스레드 환경에서는 스레드들끼리 static 영역과 heap 영역을 공유 → 공유 자원에 대한 동기화 문제 발생

`synchronized`는 `lock`을 이용해 동기화를 시킴 → 4가지 사용법

1. synchronized method
2. static synchronized method
3. synchronized block
4. static synchronized block

# Synchronized Method

> `synchronized` 메소드는 인스턴스 단위로 `lock` 을 걸지만, `synchronized` 키워드가 붙은 메소드에 대해서만 `lock`을 공유함
→ 인스턴스 접근 자체에 `lock`이 걸리는 것이 아님
> 

## 예시

```java
private synchronized void syncMethod1(String msg) {
    System.out.println(msg + "의 syncMethod1 실행중" + LocalDateTime.now());
    try {
        TimeUnit.SECONDS.sleep(5);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

**하나의 인스턴스를 서로 다른 스레드가 실행한 경우** 

```java
public static void main(String[] args) {
    Method sync = new Method();
    Thread thread1 = new Thread(() -> {
        System.out.println("스레드1 시작 " + LocalDateTime.now());
        sync.syncMethod1("스레드1");
        System.out.println("스레드1 종료 " + LocalDateTime.now());
    });

    Thread thread2 = new Thread(() -> {
        System.out.println("스레드2 시작 " + LocalDateTime.now());
        sync.syncMethod2("스레드2");
        System.out.println("스레드2 종료 " + LocalDateTime.now());
    });

    thread1.start();
    thread2.start();
}
-------------------------------------------------------------------------------------
스레드1 시작 2021-12-20T17:32:09.983815800
스레드2 시작 2021-12-20T17:32:09.983815800
스레드1의 syncMethod1 실행중2021-12-20T17:32:10.003817100
스레드1 종료 2021-12-20T17:32:15.013816900
스레드2의 syncMethod2 실행중2021-12-20T17:32:15.013816900
스레드2 종료 2021-12-20T17:32:20.014763700
```

**각각의 인스턴스를 만들고 스레드들이 메소드를 호출**

```java
public static void main(String[] args) {
    Method method1 = new Method();
    Method method2 = new Method();

    Thread thread1 = new Thread(() -> {
        System.out.println("스레드1 시작 " + LocalDateTime.now());
        method1.syncMethod1("스레드1");
        System.out.println("스레드1 종료 " + LocalDateTime.now());
    });

    Thread thread2 = new Thread(() -> {
        System.out.println("스레드2 시작 " + LocalDateTime.now());
        method2.syncMethod2("스레드2");
        System.out.println("스레드2 종료 " + LocalDateTime.now());
    });

    thread1.start();
    thread2.start();
}
--------------------------------------------------------------------------------------
스레드1 시작 2021-12-20T17:39:12.626511900
스레드2 시작 2021-12-20T17:39:12.626511900
스레드1의 syncMethod1 실행중2021-12-20T17:39:12.644481900
스레드2의 syncMethod2 실행중2021-12-20T17:39:12.644481900
스레드1 종료 2021-12-20T17:39:17.653776400
스레드2 종료 2021-12-20T17:39:17.653776400
```

# Static Synchronized Method

> 인스턴스가 아닌 클래스 단위로 `lock`을 공유
> 

## 예시

```java
public static synchronized void syncStaticMethod1(String msg) {
    System.out.println(msg + "의 syncStaticMethod1 실행중" + LocalDateTime.now());
    try {
        TimeUnit.SECONDS.sleep(5);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

```java
public static void main(String[] args) {
  Thread thread1 = new Thread(() -> {
      System.out.println("스레드1 시작 " + LocalDateTime.now());
      syncStaticMethod1("스레드1");
      System.out.println("스레드1 종료 " + LocalDateTime.now());
  });

  Thread thread2 = new Thread(() -> {
      System.out.println("스레드2 시작 " + LocalDateTime.now());
      syncStaticMethod2("스레드2");
      System.out.println("스레드2 종료 " + LocalDateTime.now());
  });

  thread1.start();
  thread2.start();
}
--------------------------------------------------------------------------------------
스레드1 시작 2021-12-20T18:07:09.872182200
스레드2 시작 2021-12-20T18:07:09.872182200
스레드1의 syncStaticMethod1 실행중2021-12-20T18:07:09.887184500
스레드1 종료 2021-12-20T18:07:14.895190300
스레드2의 syncStaticMethod2 실행중2021-12-20T18:07:14.895190300
스레드2 종료 2021-12-20T18:07:19.895435900
```

<aside>
💡 클래스 단위에 거는 `lock`과 인스턴스 단위에 거는 `lock`은 공유가 안 되기 때문에 혼용해서 쓰게 된다면 동기화 이슈가 발생

</aside>

# Synchronized Block

> 인스턴스의 `block` 단위로 `lock`을 건다
> 

## 사용 방법

1. synchronized(this)
2. synchronized(Object)

## Synchronized(this)

`this`를 사용하면 모든 `synchronized block`에 `lock`이 걸림 → `this`으로 자기 자신에 `lock`을 걸었기 때문에 기다려야 함

```java
public static void main(String[] args) {

    Block1 block = new Block1();

    Thread thread1 = new Thread(() -> {
        System.out.println("스레드1 시작 " + LocalDateTime.now());
        block.syncBlockMethod1("스레드1");
        System.out.println("스레드1 종료 " + LocalDateTime.now());
    });

    Thread thread2 = new Thread(() -> {
        System.out.println("스레드2 시작 " + LocalDateTime.now());
        block.syncBlockMethod2("스레드2");
        System.out.println("스레드2 종료 " + LocalDateTime.now());
    });
    thread1.start();
    thread2.start();
}

private void syncBlockMethod1(String msg) {
    synchronized (this) {
        System.out.println(msg + "의 syncBlockMethod1 실행중" + LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
-------------------------------------------------------------------------------------
스레드2 시작 2021-12-20T18:40:11.652664400
스레드1 시작 2021-12-20T18:40:11.652664400
스레드2의 syncBlockMethod2 실행중2021-12-20T18:40:11.668626700
스레드2 종료 2021-12-20T18:40:16.675778600
스레드1의 syncBlockMethod1 실행중2021-12-20T18:40:16.675778600
스레드1 종료 2021-12-20T18:40:21.676119600
```

## S****ynchronized(Object)****

> 블록마다 다른 `lock`이 걸리게 해 효율성 증가
> 

```java
private final Object o1 = new Object();
private final Object o2 = new Object();

public static void main(String[] args) {
    Block2 block = new Block2();

    Thread thread1 = new Thread(() -> {
        System.out.println("스레드1 시작 " + LocalDateTime.now());
        block.syncBlockMethod1("스레드1");
        System.out.println("스레드1 종료 " + LocalDateTime.now());
    });

    Thread thread2 = new Thread(() -> {
        System.out.println("스레드2 시작 " + LocalDateTime.now());
        block.syncBlockMethod2("스레드2");
        System.out.println("스레드2 종료 " + LocalDateTime.now());
    });
    thread1.start();
    thread2.start();
}

private void syncBlockMethod1(String msg) {
    synchronized (o1) {
        System.out.println(msg + "의 syncBlockMethod1 실행중" + LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
-------------------------------------------------------------------------------------
스레드2 시작 2021-12-20T18:44:09.476825500
스레드1 시작 2021-12-20T18:44:09.476825500
스레드2의 syncBlockMethod2 실행중2021-12-20T18:44:09.494827100
스레드1의 syncBlockMethod1 실행중2021-12-20T18:44:09.494827100
스레드2 종료 2021-12-20T18:44:14.504974100
스레드1 종료 2021-12-20T18:44:14.504974100
```

`o1`과 `o2` 객체를 만들어 인자로 넘겨주면 동시에 `lock`이 걸려야 하는 부분을 따로 지정

## Static Synchronized Block

- static method 안에 synchronized block을 지정
- lock 객체를 지정하고 block으로 범위를 한정 지을 수 있음
- 클래스 단위로 lock을 공유

# Singleton 객채에서의 동기화

멀티 스레드 환경에서는 동기화 이슈 발생

```java
public class BasicSingleton {
  private static BasicSingleton sBasicSingleton;
	// synchronized을 붙여 해결
	// BUT, 병목현상을 겪을 수 있음
  public static synchronized BasicSingleton getInstance() {
      if (Objects.isNull(sBasicSingleton)) {
          sBasicSingleton = new BasicSingleton();
      }
      return sBasicSingleton;
  }
}
```

# Double Checked Locking

최초 인스턴스가 생성된 이후로는 동기화 블럭에 진입하지 않기 때문에 효율적인 방식이라고 생각할 수 있지만, 특정 상황에서는 정상 동작하지 않을 수 있음

```java
public class LazySingleton {
  private volatile static LazySingleton sLazySingleton;
  private LazySingleton() {
  }

  public static LazySingleton getInstance() {
    if (sLazySingleton == null) {
        synchronized (LazySingleton.class) {
            if (sLazySingleton == null) {
                sLazySingleton = new LazySingleton();
            }
        }

    }
    return sLazySingleton;
  }
}
```

# LazyHolder

JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 싱글톤의 초기화 책임을 JVM에게 넘김

```java
public class Singleton {
	// Singleton 클래스 로딩시 Holder 클래스를 초기화하지 않음
  private Singleton() {
  }
	// Holder 클래스는 Singleton 클래스의 getInstance() 메서드에서 
	// Holder.instance를 참조하는 순간 클래스가 로딩되며 초기화가 진행됨
  public static Singleton getInstance() {
      return Holder.instance;
  }
  
  private static class Holder {
      public static final Singleton instance = new Singleton();
  }
}
```

# 참고

## JAVA 메모리

![Untitled](https://user-images.githubusercontent.com/61227459/178134511-30754288-91e7-4bc2-9626-c3ca344e5e73.png)

1. Method Area
    1. Class 정보, 전역변수 정보, Static 변수 정보 저장
    2. Runtime Constant Pool : 상수 저장
    3. 모든 스레드에서 정보 공유
2. Heap
    1. new 연산자로 생성된 객체, Array와 같은 동적으로 생성된 데이터를 저장
    2. Garbage Collector 가 처리하지 않는 한 소멸되지 않음
    3. Reference Type 의 데이터 저장
    4. 모든 스레드에서 정보 공유
3. Stack
    1. 지역변수, 메소드의 매개변수와 같이 잠시 사용되고 필요가 없어지는 데이터 저장
    2. 지역변수 이지만 Reference Type일 경우에는 Heap 에 저장된 데이터의 주소값을 Stack 에 저장
    3. 스레드마다 하나씩 존재
4. PC Register
    1. 스레드가 생성되면서 생기는 공간
    2. 스레드가 어느 명령어를 처리하고 있는지 그 주소를 등록
    3. JVM이 실행하고 있는 현재 위치를 저장
5. Native Method Stack
    1. Java 가 아닌 다른 언어 (C, C++) 로 구성된 메소드를 실행이 필요할 때 사용되는 공간

## Static

Garbage Collector의 관리 영역 밖에 존재하기에 Static영역에 있는 멤버들은 프로그램의 종료시까지 메모리가 할당된 채로 존재

![Untitled 1](https://user-images.githubusercontent.com/61227459/178134514-2585e3bd-c212-4f7f-8fa3-a111956dfae3.png)
## this

- 객체 자신의 대한 참조값을 가짐 ( 자기 자신)
- 메소드 내에서만 사용
- 객체 자신을 메소드에 전달하거나 리턴하기 위해 사용
- 객체 생성자 내에서 사용할 경우, 다른 생성자를 호출
- 매개 변수와 객체 자신이 가지고 있는 변수의 이름이 같은 경우 이를 구분하기 위해 자신의 변수에 this를 사용
- static 메소드에서는 사용할 수 X

## Singleton

> 어떤 클래스가 최초 한번만 메모리를 할당하고(static) 그 메모리에 객체를 만들어 사용
> 

```java
public class BasicSingleton {
  private static BasicSingleton sBasicSingleton;

  public static BasicSingleton getInstance() {
      if (Objects.isNull(sBasicSingleton)) {
          sBasicSingleton = new BasicSingleton();
      }
      return sBasicSingleton;
  }
}
```

# 예상 질문

1. 자바에서 동기화 문제를 신경 써야 하는 이유
    1. 멀티 스레드 환경에서는 스레드들끼리 `static` 영역과 `heap` 영역을 공유하기 때문
2. synchronized란?
    1. `lock`을 이용해 동기화를 시킴
    2. 4가지 사용법 : `synchronized method`, `static synchronized method`, `synchronized block`, `static synchronized block`
3. `Singleton`객체에 `synchronized`키워드를 사용하면 생기는 문제점
    1. 병목현상 → Singleton이 싱글 스레드처럼 동작함
4. 3번 문제의 해결 방안
    1. `LazyHolder`
    2. JVM의 클래스 초기화 과정에 보장되는 thread-safe 특성을 이용해 Singleton의 초기화 책임을 JVM에게 넘김
    3. static class를 참조하기 전까지 클래스 로딩이 발생하지 않아 메모리를 효율적으로 사용 가능

# 출처

[https://steady-coding.tistory.com/305](https://steady-coding.tistory.com/305)

[https://elfinlas.github.io/2019/09/23/java-singleton/](https://elfinlas.github.io/2019/09/23/java-singleton/)

[https://jaynamm.tistory.com/entry/JAVA-this-의미와-사용법](https://jaynamm.tistory.com/entry/JAVA-this-%EC%9D%98%EB%AF%B8%EC%99%80-%EC%82%AC%EC%9A%A9%EB%B2%95)

[https://library1008.tistory.com/4](https://library1008.tistory.com/4)

[https://velog.io/@shin_stealer/자바의-메모리-구조](https://velog.io/@shin_stealer/%EC%9E%90%EB%B0%94%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0)