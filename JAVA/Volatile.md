# Volatile

## 하는 일

- Java 변수를 Main Memory에 저장
- 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽음
- 변수의 값을 Write할 때마다 Main Memory에 까지 작성

## Non-volatile

![Untitled](https://user-images.githubusercontent.com/61227459/179383572-6e066d3b-2ad8-4e61-85d0-58886a7447e5.png)

- 멀티쓰레드 어플리케이션에서는 Task를 수행하는 동안 `성능 향상`을 위해 Main Memory에서 읽은 변수 값을 CPU Cache에 저장
- 즉, non-volatile 변수에 CPU 캐시 이용

## 가시성 문제

> 여러 개의 스레드가 사용됨에 따라, CPU Cache Memory 와 RAM 의 데이터가 서로 일치하지 않아 생기는 **문제**
> 

- Thread-1은 counter값을 증가 → CPU Cache에만 반영 → Main Memory에는 반영 X
- Thread-2는 count값을 계속 읽어오지만 0을 가져오는 문제가 발생

## 해결

- `volatile` 키워드를 추가

```java
public class SharedObject {
    public volatile int counter = 0;
}
```

- Main Memory에 저장하고 읽어오기 때문에 `변수 값 불일치 문제` 해결 → 같은 메모리 주소 참조

## When

- Thread-1 read & write
- Thread-2 read

> 여러 Thread가 write → synchronized으로 원자성 보장
> 

## Payback

- read & write를 Main Memory에 실행
- CPU Cache 보다 비용이 더 큼

# 참고

## Java volatile happens before guarantee

- `volatile`변수 수정 시 → 수정 전에 수정한 모든 변수들이 함께 메인 메모리에 저장(flush)
- `volatile` 변수 읽을 때 → 수정하면서 메인 메모리로 함께 저장된 다른 모든 변수들도 메인 메모리에서 읽음

### 예시

```java
public class Exchanger {

    private Object   object       = null;
    private volatile boolean hasNewObject = false;

    public void put(Object newObject) {
        while(hasNewObject) {
            //wait - do not overwrite existing new object
        }
        object = newObject;
        hasNewObject = true; //volatile write
    }

    public Object take(){
        while(!hasNewObject){ //volatile read
            //wait - don't take old object (or null)
        }
        Object obj = object;
        hasNewObject = false; //volatile write
        return obj;
    }
}
```

- Thread A - put()
- Thread B - take()

→ 각자 역할을 하기에 `synchronized`블록 없이도 작동 이상 X

### JVM 관점: put() & take()의 순서를 바꾼다면?

- JVM은 성능 향상을 위해 코드를 재정리 함
    
    ```
    a = b + c
    d = a + e
    
    l = m + n
    y = x + z
    ```
    
    ```
    a = b + c
    
    l = m + n
    y = x + z
    
    d = a + e
    ```
    

```java
while(hasNewObject) {
    //wait - do not overwrite existing new object
}
hasNewObject = true; //volatile write
object = newObject;
```

- `hasNewObject` & `object`순서 바뀜
- JVM 관점에서는 서로 의존하지 않기에 바꿔도 괜찮음
- BUT, object 가시성에 문제 생길 수 있음
    - Thread B(take)는 Thread A(put)이 object에 `newObject`를 세팅하기 전에 `hasNewObject`값을 true로 읽을 수 있음
    - `object`가 어느 시점에 메인 메모리로 저장될지 보장이 없음

**Happens before guarantee**

> `volatile`변수에 대한 읽기/쓰기 명령은 JVM에 의해 재정리 되지 않음
> 

```java
// JVM이 순서 바꿀 수 있음, volatile 변수에 쓰기가 실행 되기 전
sharedObject.nonVolatile1 = 123;
sharedObject.nonVolatile2 = 456;
sharedObject.nonVolatile3 = 789;

sharedObject.volatile     = true; //a volatile variable

// volatile 실행 후 재정리 가능
int someValue1 = sharedObject.nonVolatile4;
int someValue2 = sharedObject.nonVolatile5;
int someValue3 = sharedObject.nonVolatile6;
```

# 출처

[https://parkcheolu.tistory.com/16](https://parkcheolu.tistory.com/16)

[https://nesoy.github.io/articles/2018-06/Java-volatile](https://nesoy.github.io/articles/2018-06/Java-volatile)

[https://ttl-blog.tistory.com/238](https://ttl-blog.tistory.com/238)