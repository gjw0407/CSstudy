# Interface vs Abstract

# 인터페이스 (Interface)

```java
public interface 인터페이스명 { 
	//상수 타입 
	상수명 = 값; 

	//추상 메소드 
	타입 메소드명(매개변수, ...); 

	//디폴트 메소드 
	default 타입 메소드명(매개변수, ...) { ... } 

	//정적 메소드
	 static 타입 메소드명(매개변수) { ... } 
}
```

## 특징

- 멤버 변수는 `public static final` 이어야 함 (생략 가능)
- 모든 메소드는 `public abstract` 이어야 함 (생략 가능)
- Java 8 부터는 static, default method 사용 가능
- 다중 상속 가능
- 상속하는 집합간에는 연관관계가 존재하지 않을 수 있다

## 메소드

### default 메소드

- Java 8에서 추가
- `public` 특성을 가짐 (생략 가능)
- 인터페이스 내부라도 코드를 작성 가능
- 인터페이스 유지보수에 중점 (하위 호환성)

> ...(중략) ... 바로 "하위 호환성"때문이다. 예를 들어 설명하자면, 여러분들이 만약 오픈 소스코드를 만들었다고 가정하자. 그 오픈소스가 엄청 유명해져서 전 세계 사람들이 다 사용하고 있는데, 인터페이스에 새로운 메소드를 만들어야 하는 상황이 발생했다. 자칫 잘못하면 내가 만든 오픈소스를 사용한 사람들은 전부 오류가 발생하고 수정을 해야 하는 일이 발생할 수도 있다. 이럴 때 사용하는 것이 바로 default 메소드다. (자바의 신 2권)
> 

```java
public interface Calculator {
    public int plus(int i, int j);
    public int multiple(int i, int j);
    default int exec(int i, int j){      //default로 선언함으로 메소드를 구현할 수 있다.
        return i + j;
    }
}

//Calculator인터페이스를 구현한 MyCalculator클래스
public class MyCalculator implements Calculator {
    @Override
    public int plus(int i, int j) {
        return i + j;
    }

    @Override
    public int multiple(int i, int j) {
        return i * j;
    }
}

public class MyCalculatorExam {
    public static void main(String[] args){
        Calculator cal = new MyCalculator();
        int value = cal.exec(5, 10);
        System.out.println(value);
    }
}
```

### static 메소드

- Java 8에서 추가
- 객체가 없어도 인터페이스만으로 호출이 가능
- 상속 불가능

```java
public interface Calculator {
    public int plus(int i, int j);
    public int multiple(int i, int j);
    default int exec(int i, int j){
        return i + j;
    }
    public static int exec2(int i, int j){   //static 메소드 
        return i * j;
    }
}

//인터페이스에서 정의한 static메소드는 반드시 인터페이스명.메소드 형식으로 호출해야한다.  
public class MyCalculatorExam {
    public static void main(String[] args){
        Calculator cal = new MyCalculator();
        int value = cal.exec(5, 10);
        System.out.println(value);

        int value2 = Calculator.exec2(5, 10);  //static메소드 호출 
        System.out.println(value2);
    }
}
```

### private 메소드

- Java 9에서 추가

> 자바 8의 default method와 static method는 여전히 불편하게 만든다. 단지 특정 기능을 처리하는 내부 method일 뿐인데도, 외부에 공개되는 public method로 만들어야 하기 때문이다. interface를 구현하는 다른 interface 혹은 class가 해당 method에 액세스 하거나 상속할 수 있는 것을 원하지 않아도, 그렇게 될 수 있는 것이다. 자바 9에서는 위와 같은 사항으로 인해 private method와 private static method라는 새로운 기능을 제공해준다. 따라서 코드의 중복을 피하고 interface에 대한 캡슐화를 유지할 수 있게 되었다.
> 

# 추상 클래스 (abstract)

```java
public abstract class 클래스명 { 
	int a;		
	abstract 타입 메소드명(); 
	public 타입 메소드명(){
		구현
	}
}
```

## 특징

- 추상 클래스로 생성하면 `new` 키워드를 통해 객체 생성 X
    - 상속을 위한 클래스이기 때문에 따로 객체를 생성할 수 없음
- 추상 메소드는 인터페이스의 메소드와 같이 구현부가 없음
- 추상 메소드는 서브 클래스에서 반드시 구현해야 함
    - 서브 클래스가 추상 클래스이면 구현 안해도 됨
- 접근 제어자를 자유롭게 사용 가능
- 다중 상속 불가능
- 상속하는 집합 간에 연관 관계가 있음

## 다중 상속 불가 이유

<aside>
💡 JAVA는 다중 상속 불가

</aside>

### 다이아몬드 문제

![Untitled](https://user-images.githubusercontent.com/61227459/181257644-931fd0ef-3b95-4f83-9602-bafa00d34f60.png)

→ Father, Mother 클래스는 Person 클래스의 추상 메소드 `funcA()`를 각각 구현한다. Child 클래스가 Father, Mother 클래스를 둘 다 extends 한다면, Child 클래스의 `funcA()`는 어떤 메소드를 상속받아야 할지 알 수 없다. 따라서 **다중상속의 모호성**으로 이를 금지한다.

# 차이점

![Untitled 1](https://user-images.githubusercontent.com/61227459/181257638-7ae93eca-e190-4438-ba3d-bf5b2e5f23f6.png)

Implement : 구현

Extend : 확장

![Untitled 2](https://user-images.githubusercontent.com/61227459/181257643-cd8d5fe7-2e54-407d-90b1-e981143af8cf.png)

1. 다중상속 여부
2. 상태 유무
    1. 추상 클래스는 상태 (멤버 변수, 일반 메소드, 생성자)를 가질 수 없음
    2. 인터페이스는 가능
3. 사용 의도
    1. 추상 클래스
        1. is - a : 강아지는 동물이다
        2. 상속할 각 클래스의 공통점을 찾아 추상화
        3. 부모 & 자식 간의 강한 결합 생성
    2. 인터페이스
        1. has - a : 독수리는 날 수 있다
        2. 다른 부모 클래스를 상속하더라도 같은 기능이 필요한 경우
        3. 상속 받는 클래스들 간의 관련성이 없는 경우

<aside>
💡 추상 클래스 : 향후 변화가 거의 없을 시 사용 & `is - a` 일 때 
인터페이스 : 연관관계가 불명확 & `has - a` 일 때

</aside>

# 참고

## Key : 상속 & 다형성

상속 : 재사용 + 확장

- 상위 클래스 : 추상화
- 하위 클래스 : 구체화

> “**하위 클래스는 상위 클래스이다”** 라는 식이 성립이 되어야 함
> 

다형성 : 하나의 객체가 여러 가지 타입을 가질 수 있음

- 상위 클래스가 동일한 메시지로 하위 클래스들을 서로 다르게 동작시키는 객체 지향 원리
- 부모 클래스가 자식 클래스의 동작 방식을 알 수 없어도 오버라이딩을 통해 자식 클래스를 접근 가능

동적 바인딩 : 메서드가 실행 시점에서 성격이 결정 되는 바인딩

→ 실행 시점에 동적 바인딩이 일어나 부모 클래스가 자식 클래스의 멤버함수를 접근하여 실행

# 예상 질문

1. 추상 클래스 vs 인터페이스
    1. 다중상속 유무
    2. 상태 유무
    3. 사용 의도
2. 인터페이스 변화 (Java 8, Java 9)
    1. Java 8 : default method, static method
    2. Java 9 : private method

# 출처

[https://brunch.co.kr/@kd4/6](https://brunch.co.kr/@kd4/6)

[http://alecture.blogspot.com/2011/05/abstract-class-interface.html](http://alecture.blogspot.com/2011/05/abstract-class-interface.html)

[https://pathas.tistory.com/137](https://pathas.tistory.com/137)

[https://devlog-wjdrbs96.tistory.com/370](https://devlog-wjdrbs96.tistory.com/370)

[https://life-with-coding.tistory.com/485](https://life-with-coding.tistory.com/485)