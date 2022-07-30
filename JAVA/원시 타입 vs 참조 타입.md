# 원시 타입 vs 참조 타입

# 원시 타입

> 메모리 공간의 실제 데이터 값이 저장
>

![Untitled](https://user-images.githubusercontent.com/61227459/181913600-04448a9f-7523-4b56-a204-dc9d6c3163fd.png)

- boolean
- char
- byte
- short
- int
- long
- float
- double

<aside>
💡 JVM의 피연산자 스택은 피연산자를 **4 Bytes** 단위로 저장
→ `char`나 `short`와 같이 `int`보다 작은 자료형의 값을 계산하면 `int`형으로 **자동 형 변환**되어 연산이 수행

</aside>

## 예시

`int a = 10;`

a라는 이름의 메모리 공간이 JVM **스택 영역**에 생성 → 10이 저장

# 참조 타입

> 객체의 주소를 저장하는 타입
> 

객체 : JVM **힙 영역**에 저장

참조 타입 변수 : 실제 객체의 주소를 JVM **스택 영역**에 저장

객체 사용 시 : 참조 변수에 저장된 **객체의 주소**를 불러 사용

# 원시 vs 참조

## 성능  관점

![Untitled 1](https://user-images.githubusercontent.com/61227459/181913601-15b4da50-3f52-4c1d-a6d6-92a403c0c75f.png)

원시 : 스택 영역

참조 타입 : 스택 영역에는 참조 값만 존재, 실제 값은 힙 영역에 존재

→ 참조는 메모리를 2번 접근 & 언박싱 과정 (Integer → int) 때문에 접근 속도가 느림

## 메모리 관점

![Untitled 2](https://user-images.githubusercontent.com/61227459/181913598-c7b04405-5833-455f-8781-93e2618eff36.png)

## NULL 여부

원시 : null X

→ 값이 없으면 default 반환 (int는 0, boolean은 false)

참조 : null 가능

## 제네릭 가능 여부

원시 : 제네릭 타입에서 사용 X

참조 : 제네릭 타입에서 사용 가능

# 참고

## 제네릭

```java
class Person<T>{
  public T info;
}

public class GenericDemo { 
  public static void main(String[] args) {
      Person<String> p1 = new Person<String>();
      Person<StringBuilder> p2 = new Person<StringBuilder>();
  }
}
```

<aside>
💡 클래스를 정의 할 때는 info의 데이터 타입을 확정하지 않고 인스턴스를 생성할 때 데이터 타입을 지정

</aside>