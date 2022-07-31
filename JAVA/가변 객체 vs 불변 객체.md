# 가변 객체 Mutable Object

> Class의 인스턴스가 생성된 이후에 **내부 상태가 변경 가능**한 객체
> 

## 특징

- 멀티 스레드 환경에서 사용하려면 별도의 동기화 처리가 필요하다.

## 예시

- ArrayList, HashMap, StringBuilder, StringBuffer
- 커스텀 객체를 생성하여 내부 상태를 변경할 수 있게 만든 경우



# 불변 객체 Immutable Object

> Class의 인스턴스가 생성된 이후에 **내부 상태를 변경할 수 없는** 객체
> 

## 특징

- 멀티 스레드 환경에서도 안전하게 사용할 수 있다는 신뢰성을 보장한다.
- read-only 메서드만 제공
- 객체 내부 상태를 제공하는 메서드를 제공하지 않거나
- 방어적 복사(defensive copy)를 통해 제공한다.
    - 내부를 복사하여 전달하는 방식
    - 참조(reference)를 통해 전달하면 값을 수정하고 내부의 상태가 변할 수 있다.

## 예시

- String
    - String::toCharArray()의 방어적 복사
    
    ```java
    public char[] toCharArray() {
        // Cannot use Arrays.copyOf because of class initialization order issues
        char[] result = new char[value.length];
        System.arraycopy(value, 0, result, 0, value.length);
        return result;
    }
    ```
    
- 커스텀 객체를 생성하여 내부 상태가 변경되지 않게 만든 경우

## 장점

1. **Thread-Safe**하여 병렬 프로그래밍에 유용하며 동기화를 고려하지 않아도 된다.
    - 멀티스레드 환경에서 동기화 문제가 발생하는 이유는 공유 자원에 동시에 쓰기(write) 때문이다.
    - 공유 자원이 불변이라면 항상 동일한 값을 반환하기 때문에 동기화를 고려하지 않아도 된다.
    
    → 안정성 보장 & 성능상의 이점
    

1. **실패 원자적인(Failure Atomic) 메서드**를 만들 수 있다.
    - 가변 객체로 작업하는 도중 예외가 발생하면 객체가 불안정한 상태에 빠지고, 또 다른 에러를 유발할 수 있다.
    - 불변 객체는 어떠한 예외가 발생하든 메서드 호출 전 상태를 유지한다.
        - 오류가 발생하여도 그렇지 않은 것처럼 다음 로직을 처리할 수 있다.

1. **Cache, Map, Set 등의 요소**로 활용하기 적합하다.
    - 캐시, 맵, 셋의 원소인 가변 객체가 변경되면 이를 갱신하는 부가 작업이 필요하다.
    - 불변 객체가 한 번 저장되면 다른 작업들을 고려하지 않아도 돼서 사용하기 좋다.

1. 부수 효과를 피해 **오류가능성을 최소화**할 수 있다.
    - 부수 효과: 변수의 값이나 상태 등의 변화가 발생하는 효과
    - 객체의 수정자(setter)를 통해 여러 객체들에서 값을 변경한다면 객체의 상태는 예측하기 어렵고 유지보수성을 떨어뜨린다.
    - 불변 객체는 값의 수정이 불가능하여 변경 가능성이 낮고 객체의 생성과 사용이 제한되기 때문에 부수 효과가 없는 순수 함수들로 구성된다.
        - 다른 메서드가 호출돼도 객체의 상태가 유지되기 때문에 안전하게 객체를 다시 사용할 수 있다.
        - 오류를 줄여 유지보수성을 높여준다.

1. 다른 사람이 작성한 함수를 예측 가능하며 안전하게 사용할 수 있다.
    - 불변성이 보장된 함수는 협업 과정에서 다른 사람이 개발한 함수를 위험없이 이용할 수 있다.

1. **가비지 컬렉션의 성능**을 높일 수 있다.
    - 불변 객체를 가지는 또 다른 컨테이너 객체(ImmutableHolder)가 존재한다.
        1. Object 타입의 value 불변 객체 생성
        2. ImmutableHolder 타입의 컨테이너 객체 생성
        3. ImmutableHolder가 value 객체 참조
    - GC가 컨테이너 객체 하위의 불변 객체들은 skip한다.
        - 가비지 컬렉터가 스캔해야 하는 객체의 수가 줄어서 스캔해야하는 메모리 영역과 빈도수 역시 줄어든다.

## 생성

### final 키워드

- 자바에서 변수들은 기본적으로 가변이다.
- 변수에 final 키워드를 붙이면 참조값을 변경 못하도록하여 불변성을 확보할 수 있다.
    
    ```java
    final String str = "hello";
    // str = "world";  // 컴파일 에러
    ```
    
- 내부의 객체 상태를 변경 못하도록하진 못한다.
    
    ```java
    final List<String> list = new ArrayList<>();
    list.add("hello");
    ```
    
    - 새로운 객체가 List에 더해져도(상태가 변해도) 문제가 없다.
    - 참조에 의해 값이 변경될 수 있는 경우들을 유의하고 이때는 불변 클래스를 만들어야 한다.

### 불변 클래스

1. 클래스를 final로 선언
2. 모든 클래스 변수를 private과 final로 선언
3. 객체 생성을 위한 생성자 또는 정적 팩토리 메서드 추가
    - 생성자를 선언하지 않으면 기본 생성자가 자동으로 생성된다. 다른 클래스에서 해당 객체를 자유롭게 호출할 수 있다.
4. 참조에 의한 변경 가능성이 있는 경우 방어적 복사 사용
    - 배열이나 다른 컬렉션 등

```java
public final class ImmutableClass {
    private final int age;
    private final String name;
    private final List<String> list;

    private ImmutableClass(int age, String name) {  // private
        this.age = age;
        this.name = name;
        this.list = new ArrayList<>();
    }

    public static ImmutableClass of(int age, String name) {  // 정적 팩토리 메서드 제공
        return new ImmutableClass(age, name);
    }
    
    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public List<String> getList() {
        return Collections.unmodifiableList(list);  // 방어적 복사
    }
    
}
```
