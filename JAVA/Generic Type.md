> 데이터 타입(data type)을 일반화(generalize)한다.
> 

## 정의

- 클래스나 메서드에서 사용할 내부 데이터 타입을 컴파일 시 미리 지정하는 방법
- type을 파라미터로 가지는 클래스와 인터페이스

## 장점

1. 반환값에 대한 타입 검사와 타입 변환에 들어가는 노력을 줄일 수 있다.
    - 자바 컴파일러는 제네릭 코드에 대해 강한 타입 체크를 한다.
    - 타입을 국한하기 때문에 요소를 찾을 때 타입 변환을 할 필요가 없어 프로그램 성능이 향상된다.
2. 클래스나 메서드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있다.
    - 실행시 타입 에러가 나는 것보다 컴파일 시에 미리 타입을 강하게 체크하여 에러를 사전에 방지한다.
    - 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있다.

# 타입 매개변수

```java
public class ExampleList<E, U> {...}
```

- `E`, `U`
- type parameter, type variable
- 임의의 참조형 타입
- 어떠한 문자를 사용해도 상관 없다.
    - 일반적으로 대문자 알파벳 한 글자로 표현한다.
- 여러 개의 타입 변수는 `,`로 구분한다.
- 클래스뿐만 아니라 메서드의 매개변수나 반환값으로도 사용할 수 있다.
- 기본 타입을 바로 사용할 수 없고 wrapper 클래스를 사용해야 한다.

### 자주 사용하는 타입 인자

- `T` : Type
- `E` : Element
- `K` : Key
- `V` : Value
- `N` : Number
- `R` : Result

# 선언, 생성, 제거

## Generic * 선언

```java
public class 클래스명<T> {...}
public interface 인터페이스명<T> {...}
```

### generic class

```java
class GenericClass<T> {

    T element;

    void setElement(T element) {
        this.element = element;
    }

    T getElement() {
        return element;
    }
}
```

### generic interface

```java
interface GenericInterface<T> {
    T print();
}

class GenericImpl implements GenericInterface<String> {
    @Override
    public String print() {
        return "generic interface";
    }
}
```

### generic method

```java
public class Pair<K, V> {...}

public class Util {
    public static<K, V> boolean compare(Pair<K, V> pair1, Pair<K, V> pair2) {
        return pair1.getKey().equals(pair2.getKey()) &&
            pair1.getValue().equals(pair2.getValue());
    }
}

public class GenericApplication {
    public static void main(String[] args) {
        Pair<String, Integer> pair1 = new Pair<>("abc", 1);
        Pair<String, Integer> pair2 = new Pair<>("def", 1);
        boolean isSame = Util.<String, Integer>compare(pair1, pair2);
    }
}
```

- return type과 상관없이 제네릭 메서드임을 컴파일러에게 알려주어야 한다.
    - 리턴 타입 정의 전에 제네릭 타입에 대한 정의를 반드시 먼저 명시해야 한다.
- 제네릭 클래스가 아닌 일반 클래스에도 제네릭 메서드를 정의할 수 있다.
- 클래스의 타입 인자와 제네릭 메서드의 타입 인자는 상관 없다.
    - 한 클래스 `class GenericClass<T, U>` 내에 `<T, K> void test()` 메서드가 있더라도 각자의 타입 인자 `T`는 전혀 상관 없다.

## Generic class 생성

```java
// 1. type parameter 지정
MyArray<Integer> myArray = new MyArray<Integer>();
// 2. type parameter 추정
MyArray<Integer> myArray = new MyArray<>();
```

- 타입 변수 자리에 사용할 실제 타입을 명시한다.
- 제네릭 클래스를 생성할 때 실제 타입을 명시하면 내부적으로 정의된 타입 변수가 실제 타입으로 변환되어 처리된다.
- Java SE 7부터 인스턴스 생성 시 타입을 추정할 수 있는 경우 타입을 생략할 수 있다.

## Generic Type 제거

자바 코드에서 선언, 사용된 generic type은 컴파일 시 컴파일러에 의해 자동으로 검사되고 타입이 변환된다.

그 후 generic을 사용하지 않는 코드와의 호환성을 유지하기 위해 코드의 모든 generic type은 제거되고, 컴파일된 class 파일에는 어떠한 generic type도 포함하지 않는다.

# 와일드카드

- unknown type
- 제네릭 코드에서 `?` 키워드

## 특징

- 파라미터, 필드, 지역변수, 리턴 타입 등 다양한 상황에 사용된다.
- 제네릭 메서드 호출, 제네릭 클래스 인스턴스 생성, 슈퍼타입의 타입 인자로 절대 사용되지 않는다.

## 종류

### Unbounded Wildcards, `<?>`

1. Object 클래스에서 제공하는 기능을 사용한 메서드
2. generic class의 타입 인자를 사용하지 않는 메서드
    - ex> List.size, List.clear
    - `Class<T>`의 대부분의 메서드는 T에 의존하지 않기 때문에 `Class<?>`가 자주 사용된다.

- 타입 인자를 대치
- 모든 클래스나 인터페이스 타입이 올 수 있다.

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```

`List<Integer>`, `List<String>`, `List<Double>` 등은 `List<Object>`의 위 메서드를 사용할 수 없다.

```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```

모든 타입이 위 메서드 사용 가능

```java
List<Integer> ints = Arrays.asList(1, 2, 3);
List<String>  strs = Arrays.asList("one", "two", "three");
printList(ints);  // 1 2 3
printList(strs);  // one two three
```

### Upper Bounded Wildcards, `<? extends 상위타입>`

- 와일드 카드 범위: 특정 객체의 하위 클래스만
    - `extends`나 `implements`로 표현하는 구체 타입, 서브 타입

```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

```java
List<Integer> ints = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(ints));  // 6.0
```

- `List<Number>`이 `List<? extends Number>`보다 제한적이다.
- Number의 서브타입: Integer, Double, Float 등

### Lower Bounded Wildcards, `<? super 하위타입>`

- 와일드 카드 범위: 특정 객체의 상위 클래스만

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
```

- Integer의 수퍼타입: Integer, Number, Object

# 예시

### 타입 변환

- generic 사용 X
    
    ```java
    ArrayList arrayList = new ArrayList();
    arrayList.add("element");
    String str = (String) arrayList.get(0);
    ```
    
- generic 사용 O
    
    ```java
    ArrayList<String> arrayList = new ArrayList<>();
    arrayList.add("element");
    String str = arrayList.get(0);
    ```
    

### 타입 변수의 다형성

```java
class LandAnimal {
    public void cry() {
        System.out.println("육지동물");
    }
}

class Cat extends LandAnimal {
    public void cry() {
        System.out.println("냐옹");
    }
}

class Dog extends LandAnimal {
    public void cry() {
        System.out.println("멍멍");
    }
}

class Bird {
    public void cry() {
        System.out.println("짹짹");
    }
}

class AnimalList<T> {
    ArrayList<T> animals = new ArrayList<T>();

    void add(T animal) {
        animals.add(animal);
    }

    T get(int index) {
        return animals.get(index);
    }

    boolean remove(T animal) {
        return animals.remove(animal);
    }

    int size() {
        return animals.size();
    }
}

public class GenericApplication {
    public static void main(String[] args) {
        AnimalList<LandAnimal> landAnimals = new AnimalList<>();

        landAnimals.add(new LandAnimal());
        landAnimals.add(new Cat());
        landAnimals.add(new Dog());
        // landAnimals.add(new Bird()); // 오류 발생

        for (int i = 0; i < landAnimals.size(); i++) {
            landAnimals.get(i).cry();
        }
    }
}
```

```
<실행 결과>

육지동물
냐옹
멍멍
```
