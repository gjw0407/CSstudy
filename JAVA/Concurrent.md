# Concurrent

> java.util.concurrent
> 

java.util.concurrent는 Java 5에서 추가된 패키지로, 동기화가 필요한 상황에서 사용할 수 있는 다양한 유틸리티 클래스들을 제공한다. 패키지에서 제공하는 주요 기능을 요약하면 아래와 같다.

- Locks : 상호 배제를 사용할 수 있는 클래스를 제공한다.
- Atomic : 동기화가 되어있는 변수를 제공한다.
- Executors : 쓰레드 풀 생성, 쓰레드 생명주기 관리, Task 등록과 실행 등을 간편하게 처리할 수 있다.
- Queue : thread-safe한 FIFO 큐를 제공한다.
- Synchronizers : 특수한 목적의 동기화를 처리하는 5개의 클래스를 제공한다. (Semaphroe, CountDownLatch, CyclicBarrier, Phaser, Exchanger)

## java.util.concurrent.locks

locks 패키지엔 상호 배제를 위한 Lock API가 정의되어 있다. java의 synchronized 블록을 사용했을 때와 동일한 메커니즘으로 동작한다. 내부적으로 synchronized를 사용해 구현되었으며, synchronized를 더욱 유연하고 정교하게 처리하기 위해 사용하는 것이지 대체하는 목적을 가지진 않았다.

### Interface

- `Lock` : 공유 자원에 한번에 한 쓰레드만 read, write를 수행 가능하도록 한다.
- `ReadWriteLock` : Lock에서 한 단계 발전된 메커니즘을 제공하는 인터페이스다. 공유 자원에 여러개의 쓰레드가 read를 수행할 수 있지만, write는 한번에 한 쓰레드만 수행 가능하다.
- `Condition` : Object 클래스의 monitor method인 wait, nofity, notifyAll 메서드를 대체한다. wait → await, notify → signal, notifyAll → signalAll로 생각하면 된다.

### Interface의 구현체

- ReentrantLock : Lock의 구현체. 임계영역의 시작 지점과 종료 지점을 직접 명시할 수 있게 해준다.
- ReentrantReadWriteLock : ReadWriteLock의 구현체.

### 주요 메서드

- `lock()` : Lock 인스턴스에 잠금을 걸어둔다. Lock 인스턴스가 이미 잠겨있는 상태라면, 잠금을 걸어둔 쓰레드가 unlock()을 호출할 때까지 실행이 비활성화된다.
- `lockInterruptibly()` : 현재 쓰레드가 intterupted 상태가 아닐 때 Lock 인스턴스에 잠금을 건다. 현재 쓰레드가 intterupted 상태면 InterruptedException를 발생시킨다.
- `tryLock()` : 즉시 Lock 인스턴스에 잠금을 시도하고 성공 여부를 boolean 타입으로 반환한다.
    - tryLock(long timeout, TimeUnit timeUnit) : tryLock()과 동일하지만, 잠금이 실패했을 때 바로 false를 반환하지 않고 인자로 주어진 시간동안 기다린다.
- `unlock()` : Lock 인스턴스의 잠금을 해제한다.
- `newCondition()` : 현재 Lock 인스턴스와 연결된 Condition 객체를 반환한다.

### Lock 적용 예제

여러 쓰레드가 동일한 자원을 공유할 때 벌어지는 일을 확인하기 위한 간단한 예제를 만들어보자.

SharedData는 모든 쓰레드가 공유할 데이터를 정의한 클래스다.

여러개의 쓰레드가 하나의 SharedData 인스턴스를 공유하며 increase() 메서드를 마구마구 호출할 예정이다.

```java
public class SharedData {
private int value;

public void increase() {
        value += 1;
    }

public void print() {
        System.out.println(value);
    }
}
```

main 함수에서 10개의 TestRunnable 객체를 생성해 쓰레드별로 각각 increase()를 100번씩 호출하는 코드를 작성했다.

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Main {
public static void main(String[] args) {
final SharedData mySharedData = new SharedData();// shared resource
for (int i = 0; i < 10; i++) {
new Thread(new TestRunnable(mySharedData)).start();
        }
    }
}

class Test Runnable implements Runnable {
private final SharedData mySharedData;

public Test Runnable(SharedData mySharedData) {
this.mySharedData = mySharedData;
    }

@Override
public void run() {
for (int i = 0; i < 100; i++) {
            mySharedData.increase();
        }

        mySharedData.print();
    }
}
```

실행 결과는 아래와 같다.

```
101
300
401
494
594
200
694
894
794
994
```

TestData 객체를 공유하는 10개의 쓰레드가 run() 블록에 정의된 작업을 시분할 방식으로 번갈아가며 실행하고 있다. 이로인해 실행 결과는 매번 조금씩 달라져 동일한 결과가 보장되지 않는다. 만약 개발자가 value가 순차적으로 100씩 증가하는 상황을 의도했다면 이는 잘못된 동작에 해당된다.

이제 Lock 인스턴스를 사용해 이러한 동시성 문제를 해결할 수 있다.

쓰레드들이 공유할 **Lock 인스턴스를 만들고**, 동기화가 필요한 실행문의 앞 뒤로 **lock(), unlock()**을 호출하면 된다.

이때 lock()을 걸어놨다면 unlock()도 빼먹지 말고 반드시 호출해줘야 한다. 임계 영역 블록의 실행이 끝나더라도 unlock()이 호출되기 전까지는 쓰레드의 잠금 상태가 영원히 유지되기 때문이다. 어떤 예외가 발생하더라도 반드시 unlock()이 호출되도록 `try-catch-finally` 형태를 사용하는 것이 권장된다.

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Main {
public static void main(String[] args) {
final SharedData mySharedData =new SharedData();// shared resource
final Lock lock =new ReentrantLock();// lock instance
for (int i = 0; i < 10; i++) {
new Thread(new TestRunnable(mySharedData, lock)).start();
        }
    }
}

class Test Runnable implements Runnable {
private final SharedData mySharedData;
private final Lock lock;

public Test Runnable(SharedData mySharedData, Lock lock) {
this.mySharedData = mySharedData;
this.lock = lock;
    }

@Override
public void run() {
        lock.lock();
try {
				for (int i = 0; i < 100; i++) {
                mySharedData.increase();
        }

        mySharedData.print();

        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

실행해보면 값이 순차적으로 100씩 증가하는 것을 알 수 있다.

```
// 실행 결과
100
200
300
400
500
600
700
800
900
1000
```

### synchronized vs Lock

앞에서 ReentrantLock는 기존의 synchronized보다 확장된 기능을 가지고 있다는 이야기를 했지만, 사실 앞선 예제는 synchronized 키워드로 충분히 대체가 가능하다. 그렇다면 synchronized와 Lock는 어떤 차이가 있는 것일까?

이 둘을 구분짓는 키워드는 **fairness(공정성)**이다. 여기서 공정성은 모든 쓰레드가 자신의 작업을 수행할 기회를 공평하게 갖는 것을 의미한다. 공정한 방법에선 큐 안에서 쓰레드들이 무조건 순서를 지켜가며 lock을 확보한다. 불공정한 방법에선 만약 특정 쓰레드에 lock이 필요한 순간 release가 발생하면 대기열을 건너뛰는 새치기 같은 일이 벌어지게 된다.

다른 쓰레드들에게 우선순위가 밀려 자원을 계속해서 할당받지 못하는 쓰레드가 존재하는 상황을 starvation(기아 상태)라 부른다. 이러한 기아 상태를 해결하기 위해 공정성이 필요하다.

synchronized는 공정성을 지원하지 않는다. 반면 ReentrantLock은 생성자의 인자를 통해서 공정/불공정 설정을 할 수 있다. ReentrantLock의 생성자는 아래와 같이 정의되어 있다.

```java
public ReentrantLock() {
    sync = new NonfairSync();
}

public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() :new NonfairSync();
}
```

공정한 lock을 사용할 경우 경쟁이 발생했을 때 가장 오랫동안 기다린 쓰레드에게 lock을 제공한다. 락을 요청하는 시간 간격이 긴 경우가 아니라면, 쓰레드를 공정하게 관리하는 것보다 불공정하게 관리할 때 성능이 더 우수하다. 그래서 일반적으로는 불공정 방식이 사용되는 듯 하다.

그 외의 차이점을 간단히 정리해 보았다.

- synchronized는 블록 구조를 사용하기 때문에 **하나의 메서드** 안에서 임계 영역의 시작과 끝이 있어야 한다. Lock은 lock(), unlock()으로 시작과 끝을 명시하기 때문에 임계 영역을 **여러 메서드에 나눠서** 작성할 수 있다.
- synchronized는 동기화가 필요한 블럭을 synchronized { }로 감싸 lock을 건다. 여러 쓰레드가 경쟁 상태에 있을 때 어떤 쓰레드가 진입권을 획득할지 순서를 보장하지 않는다. 이를 **암시적인(implicit) lock** 이라고 칭한다.
- Lock은 lock()-unlock() 메서드를 호출함으로써 어떤 쓰레드가 먼저 락을 획득하게 될지 순서를 지정할 수 있다. 이를 **명시적인(explicit) lock**이라고 칭한다.
- Lock은 인스턴스에 한개 이상의 Condition을 지정할 수 있다. lockInterruptibly(), tryLock() 같은 편리한 제어 메서드를 사용할 수 있고, lock 획득을 기다리고 있는 쓰레드의 목록을 간편하게 확인할 수 있다.
- synchronized는 간결한 코드로 임계 영역을 지정할 수 있다. 그리고 개발자의 실수로 lock을 해제하지 않아 문제가 생길 가능성이 없다. Lock을 사용할 경우 synchronized를 사용할 때와 달리 import 구문과 try-finally 문이 추가됨으로써 코드가 덜 간결해진다는 단점이 있다.

## Atomic Access

> 모 아니면 도로 처음부터 끝까지 완전히 수행되던가, 아니면 아예 아무것도 수행되지 않아야 하는 action
> 

쇼핑몰에서 물건을 주문하는 경우를 생각해보자. '결제'와 '상품 수량 변경'은 서로 다른 작업이지만 반드시 한 세트로 진행되어야 한다. 될 거면 둘 다 되던가, 안될 거면 둘 다 안돼야 한다.

어떤 상품의 재고가 1개밖에 없는데, 해당 상품을 사려는 고객이 2명 존재하는 상황을 생각해 보자.

아래와 같은 순서로 요청이 들어오면 어떻게 될까?

1) 1번 회원 결제 성공

2) (상품 수량 업데이트가 아직 안 끝났는데!) 2번 회원도 결제 성공

3) 상품 수량 변경 (재고가 0개가 되었다)

4) 상품 수량 변경 (2번 회원은 이미 돈을 지불했는데 재고가 없다?!)

회원이 딱 한명밖에 없어서 '1번 회원 결제 → 상품 수량 변경 → 1번 회원 결제 → 상품 수량 변경'처럼 작업이 하나씩 순서대로 진행되면 좋겠지만, 현실에서는 많은 사람들이 동시다발적으로 동일한 제품을 구입하려는 상황이 부지기수다.

이 작업이 정상적으로 수행되려면 1번 회원 결제 결과가 재고 수량에 반영되기 전 2번 회원의 결제 시도는 잠시 미뤄놔야 한다. 그리고 2번 회원의 작업이 시작되려 할 때 재고를 확인하고 품절이 되었다면 아예 결제부터 불가능하게 막아야 한다.

이 외에도 '수강 신청→출석부 갱신', '결제→좌석 예약'처럼 실행 결과가 다른 작업에 영향을 줄 수 있는 작업이 일부만 수행될 경우 데이터에 결함이 생길 수 있다.

하나의 쓰레드가 모든 작업을 순차적으로 처리한다면, 작업 간 공유하는 데이터가 틀어지는 일은 생기지 않을 것이다. 하지만 많은 양의 요청을 단일 쓰레드로 느긋하게 처리하면 사용자는 인내심을 시험받게 되므로... 보통은 멀티쓰레드가 사용된다.

여러 개의 작업을 쪼개서 번갈아가며 실행하는 멀티 쓰레드 환경에서 비 원자 연산이 돌아가면 위와 같은 문제가 생길 수 있다. **이처럼 작업 단위가 분리되면 안되는 연산**에 Atomic operation이 필요하다.

멀티쓰레드 환경에서 동시성 문제를 제어할 수 있도록 Java는 여러 가지 형태로 Atomic operation을 지원한다.

대표적으로 `volatile`, `synchronization`, `Atomic` 세 가지가 있는데, Atomic Type의 경우 예약어에 해당하는 앞의 두 개와 달리 java.util.concurrent.atomic 패키지에 정의된 클래스다. 이번 글에선 바로 요 Atomic Type을 다뤄볼 예정이다.

### Atomic Type

`Atomic Type` 은  단일 변수에 대해서 Atomic Operations을 지원한다.

Wrapping 클래스의 일종으로, 참조 타입과 원시 타입 두 종류의 변수에 모두 적용이 가능하다. 사용 시 내부적으로 Compare-And-Swap(CAS) 알고리즘을 사용해 lock 없이 동기화 처리를 할 수 있다. 사용법은 매우 간단하다. 변수를 선언할 때 타입을 Atomic 타입으로 선언 해주면 된다.

**주요 Class**

- AtomicBoolean
- AtomicInteger
- AtomicLong
- AtomicIntegerArray
- AtomicDoubleArray

**주요 Method**

- get() : 현재 값을 반환한다.
- set(newValue) : newValue로 값을 업데이트한다.
- getAndSet(newValue) : 원자적으로 값을 업데이트하고 원래의 값을 반환한다.
- compareAndSet(expect, update) : 현재 값이 예상하는 값(=expect)과 동일하다면 값을 update 한 후 true를 반환한다. 예상하는 값과 같지 않다면 update는 생략하고 false를 반환한다.
- Number 타입의 경우 값의 연산을 할 수 있도록 addAndGet(delta), getAndAdd(delta), getAndDecrement(), getAndIncrement(), incrementAndGet() 등의 메서드를 추가로 제공한다.

### 사용 예제

사용법을 알아보기 위해 가장 흔하면서도 이해하기 쉬운 예제를 작성해보자.MyLock 클래스에는 여러 개의 쓰레드가 lock을 얻기 위해 경쟁할 때 lock의 상태를 관리하는 로직이 정의되어 있다. 아래의 코드에서 개발자가 의도한 동작은 여러개의 쓰레드 중 오직 **하나의 쓰레드만 lock을 얻는 것**이다.

```java
class MyLock {
private boolean locked =false;

public boolean tryLock() {
		if (!locked) {
            locked =true;
						return true;
        }
		return false;
    }
}
```

실행되는 순간 lock 획득을 시도하는 Runnable의 구현체를 정의했다.

```java
class Test Runnable implements Runnable {
		private final MyLock myLock;

public Test Runnable(MyLock myLock) {
		this.myLock = myLock;
}

@Override
public void run() {
        System.out.println(Thread.currentThread().getName() + " - " + myLock.tryLock());
    }
}
```

main에선 하나의 Lock 객체를 공유하는 10개의 쓰레드를 생성하고 실행한다.

```java
public class Main {
public static void main(String[] args) {
		final MyLock myLock =new MyLock();// shared resource
		for (int i = 0; i < 10; i++) {
				new Thread(new TestRunnable(myLock)).start();
        }
    }
}
```

그런데 실행해보니 코드가 워낙 간단하기도 하고, 요즘 컴퓨터는 CPU 성능이 좋아서 그런가 예상과 달리 동작이 잘 된다..? 🧐 tryLock이 더 값비싼 작업이 되도록 10만 번 돌아가는 반복문을 한 줄 추가해보았다.

```java
class MyLock {
private boolean locked =false;

public boolean tryLock() {
		if (!locked) {
		// 비용이 큰 작업을 수행한다for (int i = 0; i < 100_000; i++) { }

    locked = true;
		return true;
		}
		return false;
    }
}
```

실행결과는 아래와 같다. 실행할 때마다 결과가 매번 다르고, 코드를 작성했을 때의 의도와 달리 lock을 획득한 쓰레드가 여러 개 존재한다.

- 첫 번째 실행 결과
    
    ```
    Thread-0 - true
    Thread-2 - true
    Thread-3 - false
    Thread-1 - true
    Thread-6 - false
    Thread-7 - false
    Thread-5 - false
    Thread-4 - false
    Thread-8 - false
    Thread-9 - false
    ```
    
- 두 번째 실행 결과
    
    ```
    Thread-0 - true
    Thread-5 - false
    Thread-4 - false
    Thread-3 - true
    Thread-1 - true
    Thread-2 - true
    Thread-6 - false
    Thread-7 - false
    Thread-8 - false
    Thread-9 - false
    ```
    

이 문제를 해결하려면 tryLock의 원자성이 보장되어야 한다.

변수를 선언할 때 타입을 boolean에서 AtomicBoolean으로 바꿔보자.

locked가 false일 때만 값을 true로 변경하고, 이미 값이 true라면 set은 생략한다.

compareAndSet 내부에서 값을 원자적으로 갱신하기 때문에 동시성 문제가 심플하게 해결된다.

```java
class MyLock {
private AtomicBoolean locked = new AtomicBoolean();

public boolean tryLock() {
		if (!locked.get())  {
			// 비용이 큰 작업을 수행한다
						for (int i = 0; i < 100_000; i++) { }
		}

		return locked.compareAndSet(false,true);
    }
}
```

다시 실행해보면 하나의 쓰레드만 lock을 획득할 수 있다는 것을 확인할 수 있다.

```
Thread-0 -true
Thread-1 -false
Thread-2 -false
Thread-3 -false
Thread-4 -false
Thread-5 -false
Thread-6 -false
Thread-7 -false
Thread-8 -false
Thread-9 -false
```