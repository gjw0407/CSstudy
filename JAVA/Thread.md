# Thread

# 메인 스레드

> 메인 스레다가 `main()` 메소드를 실행하면서 시작, `main()`의 마지막 코드 or return 만나면 종료
>

![Untitled](https://user-images.githubusercontent.com/61227459/179383607-0c52ad55-9071-4bde-a6c9-eaa0747d2af5.png)

- 싱글 스레드 : 메인 스레드 종료 = 프로세스 종료
- 멀티 스레드 : 실행 중인 스레드가 하나라도 있으면 프로세스는 살아 있음

# 작업 스레드 생성과 실행

## 생성

```java
Thread thread = new Thread(new Runnable() {
  @Override
  public void run() {
      
  }
});

Thread thread = new Thread(() -> System.out.println("작업 스레드입니다."));
```

`Runnable`: 작업 스레드가 실행할 수 있는 코드를 가진 I/F

## 실행

### **메인 스레드만 사용한 예제**

```java
public static void main(String[] args) {
    Toolkit toolkit = Toolkit.getDefaultToolkit();
    for (int i = 0; i < 5; i++) {
        toolkit.beep();
        try {
            Thread.sleep(500);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
		// 비프음을 작업이 끝나야만 프린팅 가능
    for (int i = 0; i < 5; i++) {
        System.out.println("띵");
        try {
            Thread.sleep(500);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### **메인 스레드와 작업 스레드를 동시에 사용한 예제**

```java
public static void main(String[] args) {
    Thread thread = new Thread(() -> {
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 5; i++) {
            toolkit.beep();
            try {
                Thread.sleep(500);
            } catch (Exception e) {
                e.printStackTrace();;
            }
        }
    });
		// 새로운 작업 스레드가 비프음을 발생, 메인 스레드는 그 아래 비프음을 출력
		// 거의 동시에 비프음 발생 & 출력
    thread.start();

    for (int i = 0; i < 5; i++) {
        System.out.println("띵");
        try {
            Thread.sleep(500);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

# 스레드 우선 순위

## 동시성 vs 병렬성

[동시성 문제](동시성문제.md)

## 스레드 스케줄링

> 스레드의 개수 > 코어의 수인 경우, 어떤 스레드에게 CPU 제어권을 주어주는지 결정
> 
- 우선 순위 방식
    - 프로그래머가 특정 스레드에게 우선 순위를 코드로 제어
- 라운드 로빈 방식
    - JVM에 의해 정해져서 코드로 제어 X

## 우선 순위 방식

```java
public class Main {
  public static void main(String[] args) {
      for (int i = 1; i <= 10; i++) {
          Thread thread = new CalcThread("thread" + i);
          if (i != 10) {
              thread.setPriority(Thread.MIN_PRIORITY);
          } else {
              thread.setPriority(Thread.MAX_PRIORITY);
          }
          thread.start();
      }
  }
}

public class CalcThread extends Thread {
  public CalcThread(String name) {
      setName(name);
  }

  @Override
  public void run() {
			// ( '_' == ',' ) 
      for (int i = 0; i < 2_000_000_000; i++) {

      }
      System.out.println(getName());
  }
}
------------------------------------------------------------------------------------
// thread10이 우선 순위 높아서 먼저 수행, 나머지는 랜덤
thread10
thread8
thread5
thread9
thread6
thread1
thread7
thread3
thread4
thread2
```

# 동기화 메소드와 동기화 블록

## 임계 영역 (Critical Section)

> 멀티 스레드 프로그램에서 단 하나의  스레드만 실행 할 수 있는 코드 영역
> 

### 동기화 메소드

```java
public synchronized void method() {
  임계 영역; // 단 하나의 스레드만 실행
	// 스레드가 동기화 메소드를 실행하는 즉시 객체에 lock이 걸리고 
	// 동기화 메소드 실행을 종료해야 lock이 풀림
}
```

### 동기화 블록

```java
public void method() {
  // 여러 스레드가 실행 가능한 영역
  ...
  synchronized(공유 객체) {
      임계 영역 // 단 하나의 스레드만 실행
  }
  // 여러 스레드가 실행 가능한 영역
  ...
}
```

![Untitled 1](https://user-images.githubusercontent.com/61227459/179383602-64f08169-0603-4d17-bb16-b464ce2a3c18.png)
# 스레드 상태

![Untitled 2](https://user-images.githubusercontent.com/61227459/179383604-6396a555-b1d2-4925-9b96-71814ce67be4.png)
# 스레드 상태 제어

![Untitled 3](https://user-images.githubusercontent.com/61227459/179383605-460392be-bf99-423b-bc5b-bcb16844214c.png)

| 메소드 | 설명 |
| --- | --- |
| interrupt() | 일시 정지 상태의 스레드에서 InterruptedException 예외를 발생시켜, 예외 처리 코드(catch)에서 실행 대기 상태로 가거나 종료 상태로 갈 수 있도록 한다. |
| notify(), notifyAll() | 동기화 블록 내에서 wait() 메소드에 의해 일시 정지 상태에 있는 스레드를 실행 대기 상태로 만든다. |
| resume() | suspend() 메소드에 의해 일시 정지 상태에 있는 스레드를 실행 대기 상태로 만든다. 다만, 이 메소드는 Deprecated 되었으므로 notify()나 notifyAll()을 사용한다. |
| sleep() | 주어진 시간동안 스레드를 일시 정지 상태로 만든다. 주어진 시간이 지나면 자동으로 실행 대기 상태가 된다. |
| join() | join() 메소드를 호출한 스레드는 일시 정지 상태가 된다. 실행 대기 상태로 가려면, join() 메소드를 멤버로 가지는 스레드가 종료되거나, 매개 값으로 주어진 시간이 지나야 한다. |
| wait() | 동기화 블록 내에서 스레드를 일시 정지 상태로 만든다. 매개 값으로 주어진 시간이 지나면 자동으로 실행 대기 상태가 된다. 시간이 주어지지 않으면 notify()나 notifyAll()을 호출해야만 실행 대기 상태로 갈 수 있다. |
| suspend() | 스레드를 일시 정지 상태로 만든다. resume() 메소드를 호출하면 다시 실행 대기 상태가 된다. 다만, 이 메소드는 Deprecated 되었으므로 wait()을 사용한다. |
| yield() | 실행 중에 우선 순위가 동일한 다른 스레드에게 실행을 양보하고 실행 대기 상태가 된다. |
| stop() | 스레드를 즉시 종료한다. 다만, 이 메소드는 Deprecated 되었으므로 사용하지 않는 것이 좋다. |

## 스레드 간 협업

> 동기화 메소드 또는 동기화 블록 내에서 사용
>

![Untitled 4](https://user-images.githubusercontent.com/61227459/179383606-90c242db-5723-4057-8a1c-f70907957e22.png)

- `notify()` : `wait()`에 의해 일시 정지된 스레드 중 하나를 실행 대기 상태로 만듦
- `notifyAll()` : `wait()`에 의해 일시 정지된 **모든** 스레드를 실행 대기 상태로 만듦

## 스레드 종료

- `stop()` : 사용 중이던 자원을 회수 하지 않고 강제 종료 시킴 (비추천)
- `interrupt()` : 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외를 발생
    - catch 블록에서 스레드 정상 종료 가능
    - `Thread.interrupted()` 통해 현재 스레드가 interrupt 요청 받았는지 확인해 스레드 종료

<aside>
💡 스레드가 실행 대기 상태거나 실행 중인 상태라면 interrupt 요청이 들어와도 스레드 종료 X

</aside>

# 데몬 스레드

> 주 스레드의 작업을 돕는 보조 스레드 (스레드 종료 시 같이 종료)
> 

주 : 워드, 미디어 플레이어, JVM

데몬 : 워드 자동 저장, 미디어 플레이어의 동영상 및 음악 재생, 가비지 컬렉터

```java
public class Main {
  public static void main(String[] args) {
      AutoSaveThread autoSaveThread = new AutoSaveThread();
			// 데몬 스레드
      autoSaveThread.setDaemon(true);
      autoSaveThread.start();
      
      try {
          Thread.sleep(3000);
      } catch (InterruptedException e){
          e.printStackTrace();
      }

      System.out.println("메인 스레드 종료");
  }
}
```

# 스레드 그룹

JVM 실행 → system 스레드 그룹 생성 → JVM 운영 스레드 생성 → system 스레드 그룹에 포함

system의 하위 스레드 그룹으로 main 생성 → 메인 스레드를 main 스레드 그룹에 포함

<aside>
💡 프로그래머가 생성하는 작업 스레드는 대부분 메인 스레드가 생성하므로 main 스레드 그룹에 속한다 생각하면 된다

</aside>

## 생성

```java
// 1. 부모 스레드 그룹을 명시 X
ThreadGroup tg = new ThreadGroup(String name);
// 2. 부모 스레드 그룹을 명시
ThreadGroup tg = new ThreadGroup(ThreadGroup parent, String name);
-----------------------------------------------------------------------------------
// 스레드 그룹에 스레드 포함
Thread t = new Thread(ThreadGroup group, Runnable target);
Thread t = new Thread(ThreadGroup group, Runnable target, String name);
Thread t = new Thread(ThreadGroup group, Runnable target, String name, long stackSize);
Thread t = new THread(ThreadGroup group, String name);
```