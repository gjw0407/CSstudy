# Thread Pool

# 개념

병렬 작업의 폭증으로 인해 스레드의 폭증을 막으려면 스레드 풀 필요

작업 처리에 사용되는 스레드를 제한된 개수만큼 정해 놓고 작업 큐에 들어오는 작업들을 하나씩 스레드가 맡아 처리

→ 작업 큐라는 곳에 작업이 대기하다가 여유가 있는 스레드가 처리

# 스레드 풀 생성 및 종료

## 생성

`ExecutorService` 인터페이스 + `Executors` 클래스

### 메소드

- newCachedThreadPool()
    - 60초 동안 스레드가 작업하지 않으면 스레드를 풀에서 쫒아냄
- newFixedThreadPool(int nThreads)
    - 스레드가 놀고 있어도 제재 없음

```java
// newCachedThreadPool()
ExecutorService executorService = Executors.newCachedThreadPool();

// newCachedThreadPool(int nThreads)
ExecutorService executorService = Executors.newFixedThreadPool(
    Runtime.getRuntime().availableProcessors()
};

// 커스텀
ExecutorService threadPool = new ThreadPoolExecutor(
    3, // 코어 스레드 개수
    100, // 최대 스레드 개수
    120L, // 최대 놀 수 있는 시간 (이 시간 넘으면 스레드 풀에서 쫓겨 남.)
    TimeUnit.SECONDS, // 놀 수 있는 시간 단위
    new SynchronousQueue<Runnable>() // 작업 큐
);
```

## 종료

- main 스레드가 종료되어도 프로세스는 계속 실행 중
- main 스레드 종료 시 해당 스레드 풀을 종료해야 함

### 메소드

- shutdown()
    - 작업 큐에 대기하고 있는 모든 작업을 처리한 뒤에 스레드 풀을 종료
- shutdownNow()
    - interrupt해서 작업 중지를 시도하고 스레드 풀을 종료
- awaitTermination(long timeout, TimeUnit unit)
    - `shutdown()` 메소드 호출 이후, 모든 작업 처리를 timeout 시간 내에 완료하면 true를 리턴
    - IF NOT, 작업 처리 중인 스레드를 interrupt하고 false를 리턴

# 작업 생성과 처리 요청

## 작업 생성

- Runnable
    - 작업 완료 이후 리턴 X
- Callable
    - 작업 완료 이후 리턴 O

```java
// Runnable
Runnable task = new Runnable() {
  @Override
  public void run() {
      // 스레드가 처리할 내용
  }
}

// Callable
Callable<T> task = new Callable<T>() {
  @Override
  public T call() throws Exception {
      // 스레드가 처리할 내용
      return T;
  }
}
```

## 작업 처리 요청

> ExecutorService의 작업 큐에 Runnable 또는 Callable 객체를 넣는 행위
> 
- execute(Runnable command)
    - void 리턴 타입
    - Runnable을 작업 큐에 저장하고, 작업 처리 결과를 받지 못함
    - 예외가 발생하면 해당 스레드를 스레드 풀에서 제거함 → `Exception` 발생
- submit(Runnable task), submit(Runnable task, V result), submit(Callable task)
    - Future 리턴 타입
    - Runnable 또는 Callable을 작업 큐에 저장
    - 리턴된 Future를 통해 작업 처리 결과를 알 수 있음
    - 예외가 발생하더라도 스레드는 종료되지 않고 다른 작업에 재사용될 수 있음
    - 이 메소드를 사용하는 것을 추천

# ****블로킹 방식의 작업 완료 통보****

- `submit()` 메소드는 매개 값으로 넘긴 Runnable 또는 Callable 작업을 스레드 풀의 작업 큐에 저장하고 즉시 Future 객체를 리턴
- Future의 `get()` 메소드를 호출하면 스레드가 작업을 완료할 때까지 블로킹 되었다가 작업을 완료하면 처리 결과를 리턴
- Runnable → 리턴 X
- Callable → 리턴 O

## ****작업 처리 결과를 외부 객체에 저장****

두 개 이상의 스레드가 작업 처리를 완료하고 외부 Result 객체에 작업 결과를 저장하면, Result 공유 객체가 작업들을 취합하여 애플리케이션에게 알림

```java
public class Result {
  int accumValue;

  public synchronized void addValue(int value) {
      this.accumValue += value;
  }
}

public class Task implements Runnable {
  private final Result result;

  public Task(Result result) {
      this.result = result;
  }

  @Override
  public void run() {
      int sum = 0;
      for (int i = 1; i <= 10; i++) {
          sum += i;
      }
      result.addValue(sum);
  }
}

public class Main {

  public static void main(String[] args) {
      ExecutorService executorService = Executors.newFixedThreadPool(
          Runtime.getRuntime().availableProcessors()
      );

      System.out.println("[작업 처리 요청]");
      Result result = new Result();
      Runnable task1 = new Task(result);
      Runnable task2 = new Task(result);
			// 2번째 인자를 반환 받을 수 있음
      Future<Result> future1 = executorService.submit(task1, result);
      Future<Result> future2 = executorService.submit(task2, result);

      try {
          result = future1.get();
          result = future2.get();
          System.out.println("[처리 결과] " + result.accumValue);
          System.out.println("[작업 처리 완료]");
      } catch (Exception e) {
          e.printStackTrace();
          System.out.println("[실행 예외 발생함] " + e.getMessage());
      }
      executorService.shutdown();
  }
}
------------------------------------------------------------------------------------
[작업 처리 요청]
[처리 결과] 110
[작업 처리 완료]
```

## 작업 완료 순으로 통보

- CompletionService의 `poll()` 메소드 또는 `take()` 메소드를 사용
    - `poll()` : 완료된 작업의 Future를 가져오되, 완료된 작업이 하나도 없다면 null을 리턴
    - `take()` : 완료된 작업의 Future를 가져오되, 완료된 작업이 없다면 블로킹

```java
// 완료된 작업이 있을 때까지 블로킹
Future<Integer> future = completionService.take();
// 블로킹 없이 곧 바로 리턴
int value = future.get();
```

# 콜백 방식

> 애플리케이션이 스레드에게 작업 처리를 요청한 후, 스레드가 작업을 완료하면 특정 메소드를 자동 실행
>

![Untitled](https://user-images.githubusercontent.com/61227459/178503215-7e056429-9101-4807-8305-d3d1ffdf18ff.png)

- 작업 처리를 요청한 후 결과를 기다리지 않고 다른 기능 수행 가능
    - WHY? 작업 처리가 완료되면 자동적으로 콜백 메소드가 실행되어 결과를 알 수 있음

## 구현

```java
// V는 결과 값의 타입, A는 첨부 값
// 첨부 값 : 콜백 메소드에 결과 값 이외에 추가적으로 전달하는 객체
CompletionHandler<V, A> callback = new CompletionHandler<>() {
  @Override
  public void completed(V result, A attachment) {}

  @Override
  public void failed(Throwable exc, A attachment) {}
};
```

```java
public class CallbackExample {
  private final ExecutorService executorService;

  private CompletionHandler<Integer, Void> callback = new CompletionHandler<Integer, Void>() {
      @Override
      public void completed(Integer result, Void attachment) {
          System.out.println("completed() 실행: " + result);
      }

      @Override
      public void failed(Throwable exc, Void attachment) {
          System.out.println("failed() 실행: " + exc.toString());
      }
  };

  public CallbackExample() {
      this(Executors.newFixedThreadPool(
          Runtime.getRuntime().availableProcessors()
      ));
  }

  public CallbackExample(ExecutorService executorService) {
      this.executorService = executorService;
  }

    public void doWork(String x, String y) {
        Runnable task = () -> {
            try {
                int intX = Integer.parseInt(x);
                int intY = Integer.parseInt(y);
                int result = intX + intY;
                callback.completed(result, null);
            } catch (NumberFormatException e) {
                callback.failed(e, null);
            }
        };
        executorService.submit(task);
    }

    public void finish() {
        executorService.shutdown();
    }
}

public class Main {
  public static void main(String[] args) {
      CallbackExample example = new CallbackExample();
      example.doWork("3", "3");
      example.doWork("3", "삼");
      System.out.println("메인 스레드 작업 할당 완료");
      example.finish();
  }
}
------------------------------------------------------------------------------------
메인 스레드 작업 할당 완료
completed() 실행: 6
failed() 실행: java.lang.NumberFormatException: For input string: "삼"
```

→ 블로킹 되지 않아 출력문 먼저 나오고 후에 콜백 메소드 실행

# 참고

## Future (지연 완료 객체)

> 비동기적인 연산의 결과를 표현
> 
- 비동기 처리가 완료되었는지 확인하고, 처리 완료를 기다리고, 처리 결과를 리턴하는 메소드를 제공
- 멀티스레드 환경에서 처리된 데이터를 다른 스레드에 전달 가능
- `get()` 메소드를 호출하면 스레드가 작업을 완료할 때까지 블로킹되었다가 작업을 완료하면 처리 결과를 리턴

```java
interface ArchiveSearcher { String search(String target); }

class App {
 ExecutorService executor = ...
 ArchiveSearcher searcher = ...

 void showSearch(final String target) throws InterruptedException {
   Future<String> future = executor.submit(new Callable<String>() {
       public String call() {
           return searcher.search(target);
       }});
   displayOtherThings(); // do other things while searching
   try {
     displayText(future.get()); // use future
   } catch (ExecutionException ex) { cleanup(); return; }
 }
}
```

→ 미래에 실행되는 Callable의 수행 결과값을 구할 때 사용

# 예상 질문

1. 스레드 풀이란?
    1. 작업 처리에 사용되는 스레드를 제한된 개수만큼 정해 놓고, 작업 큐에 들어오는 작업들을 하나씩 스레드가 맡아 처리하는 기법
2. 왜 사용하는가?
    1. 병렬 작업 많음 → 스레드 개수 증가 + 스케줄링 → CPU 바쁨 → 메모리 사용량 증가 → 성능 저하
    2. 작업 처리가 끝난 스레드는 다시 작업 큐에서 새로운 작업을 가져와 처리
    3. 작업 큐라는 곳에 작업이 대기하다가 여유가 있는 스레드가 그것을 처리하므로 스레드의 전체 개수는 일정하며 애플리케이션의 성능도 저하되지 않음
3. 블로킹 방식 vs 콜백 방식
    1. 블로킹 : 작업 처리를 요청한 후 작업이 완료될 때까지 블로킹
    2. 콜백 : 작업 처리를 요청한 후 결과를 기다릴 필요 없이 다른 기능을 수행

# 출처

[https://gunju-ko.github.io/java/2018/07/05/Future.html](https://gunju-ko.github.io/java/2018/07/05/Future.html)

[https://codechacha.com/ko/java-future/](https://codechacha.com/ko/java-future/)