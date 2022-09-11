1. 추상화
2. 캡슐화
3. 상속
4. 다형성

# 추상화 Abstraction

> 공통 속성이나 기능을 추출하는 작업
> 

## 정의

- 클래스를 설계하는 것 자체
- 세부적인 사물들의 공통적인 특징으로 파악한 후 하나의 집합으로 만들어내는 것

## 특징

- 추상적인 개념에 의존하여 설계해야 유연함을 갖출 수 있다.

# 캡슐화 Encapsulation

> 객체가 내부적으로 어떻게 기능을 구현하는지 감춘다.
> 

## 정의

- 추상화의 실현
- 기능과 특성의 모음을 클래스라는 캡슐에 분류하는 것

## 목적

1. **접근 제어자**를 통한 **정보 은닉**
    - 높은 응집도와 낮은 결합도
2. 코드를 재수정 없이 재활용하는 것
    - 책임이 있는 객체만 수정 가능하다.

## 특징

- 객체가 외부에 노출하지 않아야할 정보나 기능을 접근 제어자를 통해 **제어 권한이 있는 개체만 접근 가능**하도록 한다.
    - 외부에서 접근할 필요가 없는 요소들은 `private` 으로 접근 제한을 둔다.

### 장점

- 낮은 결합도를 유지할 수 있다.
    - 한 곳에서 변화가 일어나도 다른 곳에 미치는 영향을 최소화할 수 있다.
- 불필요한 정보는 숨기고 중요한 정보만을 표현함으로써 프로그램을 간단히 만든다.

# 상속 Inheritance

> 클래스는 다른 클래스로부터 파생(derived)될 수 있고, 그 클래스들의 필드와 메서드를 상속(inheriting)받을 수 있다.
> 

## 용어

1. 다른 클래스로부터 파생된 클래스, 하위 클래스
    - **subclass**(서브 클래스)
    - derived class(파생 클래스)
    - extended class(확장된 클래스)
    - child class(자식 클래스)
2. 서브클래스를 파생시킨 클래스, 상위 클래스
    - **superclass**(수퍼 클래스)
    - base class(기반 클래스)
    - parent class(부모 클래스)

## Object class

- java.lang package에 존재
- 모든 객체의 조상 객체
    - `Object` 클래스는 수퍼클래스가 없다.
    - 모든 클래스의 공통된 행위를 정의하고 구현한다.
- `Object`를 제외한 모든 클래스는 명시적인 다른 수퍼클래스 뿐만 아니라 내부적으로 `Object`를 수퍼클래스로 가진다.
    - 상속의 상속의 상속을 타고 올라가도 보면 궁극적으로 제일 위에 `Object` 클래스가 위치한다.

### 계층

Java Platform에서 Object 클래스의 자손 객체들

![image](https://user-images.githubusercontent.com/55528172/186415355-99b5c835-2a04-4bc9-a237-4a0c3f7ea9ff.png)

## 특징

- 기능의 확장(extension)
- 상속의 개념은 간단하지만 강력하다.
    - 새로운 클래스를 만들고 싶은데 내가 원하는 코드를 포함한 클래스가 이미 존재한다면, 그 클래스로부터 새로운 클래스를 파생시키면 된다.
- 서브 클래스는 수퍼클래스(이미 존재하는 클래스)의 **모든 `public`, `protected` 멤버**(필드, 메서드, 중첩 클래스 nested class)를 상속받아 재사용할 수 있다.
    - 서브클래스가 어느 패키지에 있건 상관없다.
    - 같은 패키지에 있다면 `package-private` 멤버도 상속 가능
    - 생성자는 멤버가 아니므로 상속될 수 없다. 서브 클래스에서 수퍼 클래스의 생성자를 호출할 수는 있다.
    - 상속받은 중첩 클래스의 모든 `private` 멤버들에도 접근 가능
- 같은 코드를 반복하여 적지 않아도 되고 디버깅이 필요 없다.

### 단점

1. 상위 클래스의 변경이 어려워진다.
    - 이를 의존하는 자식 클래스들이 영향을 받는다.
2. 불필요한 클래스가 증가할 수 있다.
    - 유사기능 확장시 필요 이상의 클래스를 만들어야 할 수도 있다.
3. 상속이 잘못 사용될 수 있다.
    1. `IS-A` 관계가 아니고 단순 재사용을 위해 상속을 받으면 문제가 발생할 수 있다.

## 예시

```java
public class Bicycle {
        
    // the Bicycle class has three fields
    public int cadence;
    public int gear;
    public int speed;
        
    // the Bicycle class has one constructor
    public Bicycle(int startCadence, int startSpeed, int startGear) {
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;
    }
        
    // the Bicycle class has four methods
    public void setCadence(int newValue) {
        cadence = newValue;
    }
        
    public void setGear(int newValue) {
        gear = newValue;
    }
        
    public void applyBrake(int decrement) {
        speed -= decrement;
    }
        
    public void speedUp(int increment) {
        speed += increment;
    }
        
}
```

```java
public class MountainBike extends Bicycle {
        
    // the MountainBike subclass adds one field
    public int seatHeight;

    // the MountainBike subclass has one constructor
    public MountainBike(int startHeight,
                        int startCadence,
                        int startSpeed,
                        int startGear) {
        super(startCadence, startSpeed, startGear);
        seatHeight = startHeight;
    }   
        
    // the MountainBike subclass adds one method
    public void setHeight(int newValue) {
        seatHeight = newValue;
    }   
}
```

- `MountainBike`는 4개의 필드와 5개의 메서드를 가진 것처럼 작성되었다.
- `super` 키워드를 사용하여 수퍼클래스의 생성자를 호출하였다.

# 다형성 Polymorphism

> 하나의 변수명, 함수명이 상황에 따라 다른 동작을 할 수 있다.
> 

## 정의

### 사전적 정의

- 생물학: 한 개체나 종은 여러 다양한 형태(form)와 단계/시기(stage)를 가질 수 있다.

### OOP

- 서브 클래스는 그들만의 고유한 행위를 정의할 수 있고, 부모 클래스의 같은 기능을 공유(share)할 수 있다.
- **오버라이딩**(overriding), 오버로딩(overloading)이 가능하다.

## 특징

- 서로 다른 클래스의 객체가 같은 메세지를 받았을 때 각자의 방식으로 동작할 수 있다.
- 상속과 함께 사용할 때 큰 힘을 발휘한다. → 오버라이딩

## 예시

```java
public class Bicycle {
    // ...

    public void printDescription(){
        System.out.println("\nBike is " + "in gear " + this.gear
            + " with a cadence of " + this.cadence +
            " and travelling at a speed of " + this.speed + ". ");
    }
}
```

```java
public class MountainBike extends Bicycle {
    private String suspension;

    public MountainBike(int startCadence, int startSpeed, int startGear, String suspensionType){
        super(startCadence, startSpeed, startGear);
        this.setSuspension(suspensionType);
    }

    // ...

    @Override
    public void printDescription() {
        super.printDescription();
        System.out.println("The " + "MountainBike has a" + getSuspension() + " suspension.");
    }
}
```

```java
public class RoadBikeextends Bicycle {
    // In millimeters (mm)
    private int tireWidth;

    public MountainBike(int startCadence, int startSpeed, int startGear, int newTireWidth){
        super(startCadence, startSpeed, startGear);
        this.setTireWidth(newTireWidth);
    }

    // ...

    @Override
    public void printDescription() {
        super.printDescription();
        System.out.println("The RoadBike" + " has " + getTireWidth() + " MM tires.");
    }
}
```

- `MountainBike`와 `RoadBike`는 `Bicycle`을 상속받았다.
- `MountainBike`와 `RoadBike`는 `Bicycle`의 `printDescription()`을 override하고 행위를 추가했다.
