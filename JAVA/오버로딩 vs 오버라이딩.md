# 오버로딩 vs 오버라이딩

# 오버로딩

## 메소드 시그니처 (method signature)

> 메소드의 선언부에 명시되어 있는 매개변수의 리스트
> 

### 조건

- 메소드 이름
- 매개변수 수
- 매개변수 타입의 순서
- 리턴타입은 메소드 시그니처에 포함되지 않는다

```java
// 기본
public int method(int x, int y) {
    return x + y;
}

// 가능
public String method(String s) {
    return s;
}

// 불가능(매개변수 수, 타입의 순서가 동일)
public void method(int a, int b) {
}
```

## 메소드 오버로딩

> 매소드 오버로딩은 같은 이름의 메소드를 중복하여 정의. 즉, 서로 다른 시그니처를 갖는 여러 메소드를 같은 이름으로 정의하는 것
> 

## 생성자 오버로딩

> 한 클래스 내에 같은 이름의 메소드를 중복하여 정의하고, 클래스로부터 객체를 생성할 때 필요한 변수들만 적절히 초기화하기 위해 사용되는 것
> 

<aside>
💡 **동일한 이름을 가진 메소드들의 매개변수의 타입 또는 개수가 모두 달라야지 오버로딩이 성립 + 한번에 여러개의 변수값들을 초기화 시킬 수 있음**

</aside>

## 장점

- 기억해야 할 이름 개수가 줄어듬
- 기능 예측이 쉬워짐
- 이름 절약
- 가독성 상승
- 매개변수 값을 다양하게 받아서 다양한 처리

## 단점

- 혼동 불러 일으킬 수 있음

## 나쁜 예시

```java
public class Main {
    public static void main(String[] args) {
        Collection<?>[] collections = {
            new HashSet<String>(),
            new ArrayList<BigInteger>(),
            new HashMap<String, String>().values()
        };
        for (Collection<?> c : collections) {
            System.out.println(classify(c));
        }
    }

    public static String classify(Set<?> s) {
        return "Set";
    }

    public static String classify(List<?> s) {
        return "List";
    }

    public static String classify(Collection<?> s) {
        return "Unknown Collection";
    }
}
```

오버로딩된 메소드 가운데 어떤 메소드를 호출할 것인지는 **컴파일 시점**에 결정

⇒ 컴파일 시점에 `Collection<?>` 타입이었던 객체 모두 `Collection`을 파라미터로 가지는 메서드가 실행

### 결과

```java
Unknown Collection
Unknown Collection
Unknown Collection
```

→ **오버라이드한 메서드는 동적으로 선택되고, 오버로딩한 메서드는 정적으로 선택된다**

```java
// instanceof 사용
public static String classify(Collection<?> c) {
			return c instanceof Set ? "SET" : c instanceof List ? "List" : "Unknown Collection";
}
// 결과
SET
List
Unknown Collection
```

# 오버라이딩

> 상속 관계에 있는 클래스 간에 같은 이름의 메소드를 정의하는 기술
> 

## 조건

1. 부모 클래스의 메소드와 동일한 시그니처를 가져야 함
2. 접근 제어자는 부모 클래스의 메서드보다 좁은 범위로 변경할 수 없다
    1. 예 : 부모가 `protected`이면 자식은 `protected` 아님 `public`만 가능
3. 부모 클래스의 메서드보다 많은 수의 예외를 선언 불가

## 필요성

오버라이딩을 하지 않으면 이름을 다르게 해야하는데 그럼 부모 클래스의 메소드를 사용할 수 있기 때문에 문제가 발생할 수 있다. 알맞지 않은 기능은 사용하지 못하게 해야 하기 때문에 오버라이딩을 사용해야 한다.

# 메소드 디스패치

<aside>
💡 디스패치 : 메서드를 호출 (객체들 간의 메세지 전송)

</aside>

## 정적 메소드 디스패치 (Static Method Dispatch)

구현 클래스를 이용해 컴파일 시점에서부터 어떤 메서드가 호출될 지 정해져 있음

```java
public class Parent {
  public void method1(){
      System.out.println("Parent method1입니다");
  }
}

public class Child extends Parent{
  @Override
  public void method1() {
      System.out.println("Child method1입니다");
  }
}

public class Main {
  public static void main(String[] args){
      Child child = new Child();
      child.method1(); //정적 메소드 디스패치
  }
}
```

자바에서 객체 생성은 Runtime 시에 호출 됨

→ 컴파일 시점에 알 수 있는 것은 타입에 대한 정보

⇒ 따라서 컴파일러 역시 이 메소드를 호출하고 실행 시켜야 되는 것을 명확하게 알고 있음

## 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

인터페이스를 이용해 참조함으로서 호출되는 메서드가 동적으로 정해지는 것

```java
public class Parent {
  public void method1(){
      System.out.println("Parent method1입니다");
  }
}

public class Child extends Parent{
  @Override
  public void method1() {
      System.out.println("Child method1입니다");
  }
}

public class Main {
  public static void main(String[] args){
      Parent parent = new Child();
      parent.method1(); //동적 메소드 디스패치
  }
}
```

`parent`객체는 `Parent`이라는 클래스 타입이기 때문에 `Child` 클래스를 할당할지라도 `Child` 클래스의 `method1`에 접근할 수가 없다

→ 하지만 결과는 `Child` 클래스의 `method1()`이 호출된다. 그 이유는 컴파일러가 어떤 메소드를 호출해야 되는지 모르지만 런타임에 정해져서 메서드를 호출하기 때문

# 참고

| public | protected | default | private |
| --- | --- | --- | --- |
| All | 상속 or 동일패키지 | 상속 && 동일패키지 | 클래스 내부 |

<aside>
💡 JAVA는 다중상속을 지원하지 않는다 (extends는 안되고 implements는 됨)

</aside>

## extends

- 부모에서 선언/정의를 모두 하며 자식은 메소드/변수를 그대로 사용 가능
- 오버라이딩 필요 X
- 일반 클래스, abstract 클래스
- interface → interface, class → class

## implements

- 부모 객체는 선언만 하며 정의(내용)은 자식에서 오버라이딩 해야 함
- interface
- class ↔ interface

## abstract

- extends와 interface 혼합. extends 하되 몇 개는 추상 메소드로 구현

# 예상 질문

1. 오버로딩 vs 오버라이딩
    1. 오버로딩 : 같은 이름의 메소드를 중복해 정의
    2. 오버라이딩 : 상속 관계에 있는 클래스 간에 같은 이름의 메소드를 정의
2. 메소드 시그니처?
    1. 메소드의 이름과 매개변수 리스트의 조합
    2. 자바 컴파일러는 메소드 시그니처를 통해 메소드 간의 차리르 식별
3. 오버로딩의 장점
    1. 메소드명을 절약
    2. 기능 예측이 쉬워짐
    3. 소스코드의 가독성 향상
    4. 매개변수 값에 따라 다양한 처리 가능
4. 오버라이딩 조건
    1. 부모 클래스의 메소드와 동일한 시그니처를 갖어야 함
    2. 접근 제어자는 부모 클래스보다 좁은 범위로 변경 불가능
    3. 부모 클래스의 메서드보다 많은 수의 예외를 선언 불가능

# 출처

[https://zbomoon.tistory.com/13](https://zbomoon.tistory.com/13)

[https://velog.io/@hkoo9329/자바-extends-implements-차이](https://velog.io/@hkoo9329/%EC%9E%90%EB%B0%94-extends-implements-%EC%B0%A8%EC%9D%B4)

[https://krapoi.tistory.com/entry/자바-extends와-implements-에-대해](https://krapoi.tistory.com/entry/%EC%9E%90%EB%B0%94-extends%EC%99%80-implements-%EC%97%90-%EB%8C%80%ED%95%B4)