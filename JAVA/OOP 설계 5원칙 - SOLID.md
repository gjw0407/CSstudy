# 객체 지향 설계 과정

- 제공해야 하는 기능을 찾고 세분화한 다음, 알맞은 객체에 할당한다.
- 기능을 구현하는 데 필요한 데이터를 객체에 추가한다.
- 데이터를 이용하는 기능을 최대한 캡슐화하여 구현한다.
- 객체간 메서드 요청을 어떻게 주고받을지 결정한다.

# 객체 지향 설계 원칙 SOLID

1. **S**ingle Responsibility Principle
2. **O**pen-Closed Principle
3. **L**iskov Substitution Principle
4. **I**nterface Segregation Principle
5. **D**ependency Inversion Principle

## 목적

설계 원칙은 유지 가능하고(maintainable), 이해 가능하고(understandable), 유연한(flexible) 소프트웨어를 만들 수 있게 해준다.

어플리케이션의 크기가 커질수록 복잡성(complexity)을 줄여준다.

## Single Responsibility Principle(SRP) 단일 책임 원칙

> 한 클래스는 하나의 책임만을 가지고, 변경될 이유는 단 하나여야 한다.
> 

### 장점

1. 테스트: 더 적은 테스트 케이스를 가진다.
2. 낮은 결합도: 한 클래스에 적은 기능은 더 적은 의존성을 가진다.
3. 구조: 작고 잘 짜여진 클래스들은 거대한 하나의 클래스에서보다 찾기 쉽다.

### 예시

```java
public class Book {

    private String name;
    private String author;
    private String text;

    //constructor, getters and setters

    // methods that directly relate to the book properties
    public String replaceWordInText(String word){
        return text.replaceAll(word, text);
    }

    public boolean isWordInText(String word){
        return text.contains(word);
    }
}
```

- `Book` 클래스는 잘 작동하고, 우리는 어플리케이션에서 많은 책들을 저장할 수 있다.
- 여기서 `text`를 콘솔에 출력하거나 읽을 수 없다면 정보를 저장하는 것은 무엇에 좋을까?

```java
public class Book {
    //...

    void printTextToConsole(){
        // our code for formatting and printing the text
    }
}
```

- `Book`에 `print()` 메서드를 추가해보았다. 이 코드는 단일 책임 원칙을 위반한다.

```java
public class BookPrinter {

    // methods for outputting text
    void printTextToConsole(String text){
        //our code for formatting and printing the text
    }

    void printTextToAnotherMedium(String text){
        // code for writing to any other location..
    }
}
```

- 텍스트를 출력하는 클래스를 따로 분리하여 구현했다.
- `Book`이 출력도 해야하는 의무에서 벗어났고, `BookPrinter`가 `text`를 다른 매체에 전송할 수 있게까지 수정하였다.
- 이메일, 로깅, 다른 어느 것이든 간에 하나의 주제에 관련된 별개의 클래스를 작성해야 한다.

## Open-Closed Principle(OCP) 개방 폐쇄 원칙

> 소프트웨어 구성요소(컴포넌트, 클래스, 모듈, 함수)가 확장에는 열려있고, 변경에는 닫혀있어야 한다.
> 

### 특징

- 기존에 작성했던 코드를 수정하고 새로운 버그를 만드는 것으로부터 멈춰야 한다.
- 현재 코드의 버그를 고치는 일은 예외이다.

### 추상화

- 고정돼있지만 제한되지는 않은, 가능한 동작의 묶음
- 구체적인 타입 대신 추상적인 타입에 의존하도록 코드를 작성한다.
- 모듈이 고정된 추상화에 의존한다면 수정에 닫혀있을 수 있고, 추상화의 새 파생 클래스를 만드는 것을 통해 확장도 가능하다.
- 변경되어서는 안되므로 여러 경우를 고려하고 예측해야하며, 적당한 추상화 레벨을 선택한다.

### 효과

- 유연성, 재사용성, 유지보수성
    - → 관리와 재사용이 가능한 코드를 만드는 기반이다.

### 예시

```java
public class Guitar {

    private String make;
    private String model;
    private int volume;

    //Constructors, getters & setters
}
```

- `Guitar`가 약간 지루해졌고 flame 패턴을 입혀서 더 힙하게 만들고 싶다.
- `Guitar` 클래스에 단순히 `flamePattern`을 추가하고자 하는 마음이 들 것이지만, 어플리케이션에 어떤 에러가 발생할지 모른다.

```java
public class SuperCoolGuitarWithFlames extends Guitar {

    private String flameColor;

    //constructor, getters + setters
}
```

- 개방-폐쇄 원칙에 따라 `Guitar` 클래스를 단순히 확장(extend)하였다.
    - 기타 클래스를 확장함으로써 우리의 어플리케이션은 영향을 받지 않게 되었다.

## Liskov Substitution Principle(LSP) 리스코프 치환 원칙

> 상위 타입은 하위 타입으로 대체해도 항상 문제없이 동작해야 한다.
> 

### 예시

- 너비와 높이의 조회(getter) 및 **할당**(setter → 가변성)을 가진 직사각형 클래스로부터 정사각형 클래스를 파생하는 경우
    - 두 클래스가 불변 객체로서 조회 메서드만 가진다면 LSP 위반이 발생하지 않는다.
- 직사각형(`Rectangle`)은 정사각형(`Square`)으로 교체될 수 없다.

```java
public class Rectangle {
    private int width;
    private int height;

    // Constructor

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }
}
```

```java
public class Square extends Rectangle {

    /*
    @Override
    public void setWidth(int length) {
        this.width = length;
        this.height = length;
    }

    @Override
    public void setHeight(int length) {
        this.width = length;
        this.height = length;
    }
    */
}
```

- `Square`가 `Rectangle`을 다루는 문맥에서 사용될 경우, 정사각형의 크기는 독립적으로 변경하면 안 되기 때문에(정사각형은 항상 너비와 높이가 같기 때문) 예기치 못한 행동을 하게 된다.
- `Square`의 setter를 수정해서 정사각형의 불변 조건(너비=높이)을 유지하면, 크기를 독립적으로 변경할 수 있다고 설명한 `Rectangle`의 할당자의 사후 조건을 무력화(위반)한다.

## Interface Segregation Principle(ISP) 인터페이스 분리 원칙

> 큰 인터페이스를 작고 구체적으로 분리시킴으로써 클래스가 필요한 메서드에만 의존해야 한다.
> 

### 특징

- 작은 인터페이스 단위를 **역할 인터페이스**라고도 부른다.
- 시스템의 내부 의존성을 약화시켜서 리팩토링, 수정, 재배포를 쉽게 할 수 있다.

### 예시

```java
public interface BearKeeper {
    void washTheBear();
    void feedTheBear();
    void petTheBear();
}
```

- 곰을 만지는 것은 위험하지만 인터페이스에 포함되어있기 때문에 반드시 구현해야 한다.

```java
public interface BearCleaner {
    void washTheBear();
}

public interface BearFeeder {
    void feedTheBear();
}

public interface BearPetter {
    void petTheBear();
}
```

```java
public class BearCarer implements BearCleaner, BearFeeder {

    public void washTheBear() {
        //I think we missed a spot...
    }

    public void feedTheBear() {
        //Tuna Tuesdays...
    }
}
```

```java
public class CrazyPerson implements BearPetter {

    public void petTheBear() {
        //Good luck with that!
    }
}
```

- 인터페이스를 분리한 덕분에 관심 있는 메서드만 구현할 수 있게 되었다.
- SRP의 예제인 `BookPrinter` 클래스도 인터페이스 분리 원칙을 적용시킬 수 있다. 간단한 `print()` 메서드만 가진 `Printer` 인터페이스를 만들고, `ConsoleBookPrinter`와 `OtherMediaBookPrinter` 클래스가 구현하도록 한다.

## Dependency Inversion Principle(DIP) 의존 역전 원칙

> 소프트웨어 모듈을 분리(decoupling)하는 특정 형식
> 

### 정의

1. 상위 모듈은 하위 모듈에 의존해서는 안 된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
2. 추상화는 세부 사항에 의존해서는 안 된다. 세부사항이 추상화에 의존해야 한다.

### 특징

- 상위 계층이 하위 계층의 구현으로부터 독립된다.
- 상위와 하위 객체 모두가 동일한 추상화에 의존해야 한다.

### 예시

```java
public class Windows98Machine {

    private final StandardKeyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine() {
        keyboard = new StandardKeyboard();
        monitor = new Monitor();
    }

}
```

- `Windows98Machine`의 생성자에서 `new` 키워드를 통해 `StandardKeyboard`와 `Monitor`를 선언하기 때문에, 세 클래스가 강하게 결합하고 있다.
    1. `Windows98Machine`를 테스트하기 어렵다.
    2. `StandardKeyboard`를 다른 키보드로 바꾸고 싶을때 변경하기 어렵게 만든다. `Monitor`도 마찬가지.

```java
public interface Keyboard { }
public class StandardKeyboard implements Keyboard { }
```

```java
public class Windows98Machine{

    private final Keyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine(Keyboard keyboard, Monitor monitor) {
        this.keyboard = keyboard;
        this.monitor = monitor;
    }
}
```

- 의존성 역전 패턴을 통해 `Windows98Machine` 클래스에 `Keyboard` 의존성을 추가하는 것이 용이해졌다.
- `StandardKeyboard` 클래스를 `Keyboard` 인터페이스로 수정했다.
- 모든 클래스들의 결합도는 낮아졌고(decoupled), `Keyboard` 추상화를 통해 연결되어있다(communicate).
- `StandardKeyboard`가 아닌 `Keyboard` 인터페이스의 다른 구현체로 변경하거나 `Windows98Machine`을 테스트하기 더 쉬워졌다.
