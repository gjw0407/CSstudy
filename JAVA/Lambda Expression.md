> 메서드를 하나의 식으로 표현한 것
> 

## 예시

- 메서드
    
    ```java
    int min(int x, int y) {
        return x < y ? x : y;
    }
    ```
    
- 람다식
    
    ```java
    (x, y) -> x < y ? x : y;
    ```
    
- 익명 클래스
    - 단 하나의 객체만을 생성할 수 있는 클래스
    
    ```java
    new Object() {
        @Override
        int min(int x, int y) {
            return x < y ? x : y;
        }
    }
    ```
    

## 장점

- 클래스를 작성하고 **객체를 생성하지 않아도 메서드를 사용**할 수 있다.
    - 함수를 만드는 과정 없이 한 번에 처리할 수 있어 생산성이 높아진다.
- 불필요한 코드를 줄여주고 작성된 코드의 가독성을 높여준다.
- 병렬 프로그래밍이 용이하다.

## 단점

- 람다를 사용하면서 만든 무명 함수는 재사용이 불가능하다.
- 디버깅이 어렵다.
- 람다를 남발하면 비슷한 함수가 중복 생성되어 코드가 지저분해질 수 있다.
- 재귀를 만들 경우에 부적합하다.

## 특징

- 람다식 내에서 사용되는 지역변수는 final이 붙지 않아도 상수로 간주된다.
- 람다식으로 선언된 변수명은 다른 변수명과 중복될 수 없다.
- Java SE 8부터 람다 표현식을 사용하여 함수형 프로그래밍을 할 수 있다.

# 작성

`->`를 사용

```java
(매개 변수 목록) -> { 함수 몸체 }
```

```java
new Thread(new Runnable() {
    public void run() {
        System.out.println("전통적인 방식의 일회용 스레드 생성");
    }
}).start();

new Thread(() -> {
    System.out.println("람다 표현식을 사용한 일회용 스레드 생성");
}).start();
```

## 유의사항

1. 매개변수의 타입을 추론할 수 있는 경우에는 타입을 생략할 수 있다.
2. 매개변수가 하나인 경우는 목록을 묶은 괄호 `()`를 생략할 수 있다.
3. 함수의 몸체가 하나의 명령문으로만 이루어진 경우에는 중괄호 `{}`를 생략할 수 있고, 이때 세미콜론 `;`은 붙이지 않는다.
4. 함수의 몸체가 하나의 return 문으로만 이루어진 경우에는 중괄호 `{}`를 생략할 수 없다.
5. return 문 대신 표현식을 사용할 수 있고, 이때 반환값은 표현식의 결과값이 되며 세미콜론 `;`은 붙이지 않는다.

# 함수형 인터페이스 functional interface

> **람다 표현식을 하나의 변수에 대입**할 때 사용하는 참조 변수의 타입
> 

```java
참조변수타입 참조변수이름 = 람다 표현식
```

## 특징

- 추상 클래스와 달리 단 하나의 추상 메서드만을 가진다.
- `@FunctionalInterface` 어노테이션을 사용하여 명시할 수 있다.
    - 자바 컴파일러는 인터페이스에 두 개 이상의 메서드가 선언되면 오류를 발생한다.
- 람다식으로 생성된 순수 함수는 함수형 인터페이스로만 선언이 가능하다.
- 람다 표현식을 저장하기 위한 참조 변수의 타입을 결정해야 한다. → 인터페이스명
- java.util.function 패키지를 통해 다양한 함수형 인터페이스를 제공한다.

## 예시

```java
@FunctionalInterface
interface Calculator {
    public int min(int x, int y);
}

public class Application {
    public static void main(String[] args) {
        Calculator minNum = (x, y) -> x < y ? x : y;  // 추상 메서드 구현
        System.out.println(minNum.min(3, 4);  // 함수형 인터페이스 사용
    }
}
```

## 종류

java.util.function에 40여개의 자바 빌트인 함수형 인터페이스가 있다.

| 인터페이스명 | 추상 메서드 |
| --- | --- |
| Runnable | void run() |
| Supplier<T> | T get() |
| Consumer<T> | void accept() |
| Predicate<T> | boolean test() |
| Function<T, R> | R apply(T t) |
| Comparator<T> | int compare(T o1, To2) |

# 메서드 참조 method reference

> 함수형 인터페이스를 람다식이 아닌 일반 메서드를 참조시켜 선언하는 방법
> 

람다 표현식이 단 하나의 메서드만을 호출하는 경우 불필요한 매개변수를 제거하고 `::` 기호를 사용하여 표현할 수 있음

```java
클래스이름::메서드이름
or
참조변수이름::메서드이름
```

## 예시

- Math 클래스의 pow() 호출
    
    ```java
    // 람다 표현식
    (base, exponent) -> Math.pow(base, exponent);
    
    // 메서드 참조
    Math::pow;
    ```
    
- 특정 인스턴스의 메서드를 참조변수의 이름을 통해 메서드 참조
    
    ```java
    MyClass myClass = new MyClass();
    Function<String, Boolean> function1 = (a) -> myClass.equals(a);  // 람다 표현식
    Fundtion<String, Boolean> function2 = myClass::equals(a);  // 메서드 참조
    ```
    
- 생성자 참조
    
    ```java
    // 단순히 객체를 생성하고 반환하는 람다 표현식
    (a) -> { return new Object(a); }
    
    // 생성자 참조
    Object::new;
    ```
    
    ```java
    Function<Integer, double[]> function1 = a -> new double[a]; // 람다 표현식
    Function<Integer, double[]> function2 = double[]::new; // 생성자 참조
    ```
