# Collection

![https://blog.kakaocdn.net/dn/BxT2E/btq8Zvjrar6/7IkSL66YAUeB79lv1kxNvk/img.png](https://blog.kakaocdn.net/dn/BxT2E/btq8Zvjrar6/7IkSL66YAUeB79lv1kxNvk/img.png)

자바에서 사용하는 컬렉션 프레임워크의 주요 클래스표이다.

좀더 상세한 클래스와 구현 인터페이스는 아래와 같다.

![https://blog.kakaocdn.net/dn/CiJc0/btq8wmvLAc0/SazMn8zAYuSFoQsY1N3CH0/img.jpg](https://blog.kakaocdn.net/dn/CiJc0/btq8wmvLAc0/SazMn8zAYuSFoQsY1N3CH0/img.jpg)

## 1. Collection Interface

**Iterator** 인터페이스를 상속한 **Collection**은 가장 기본이 되는 인터페이스로 **add(), size(), iterator()** 메소드를 가지고 있다.

![Untitled](https://user-images.githubusercontent.com/55426397/180901004-0b1e4827-1a20-481b-bbcc-e0228c00eaa1.png)


### P.S Collections란?

Collection인터페이스와 달리 Java 1.2이상부터 Collections라는 static클래스가 존재한다.

### Collections 클래스의 메소드

![Untitled 1](https://user-images.githubusercontent.com/55426397/180901013-267dea70-ad77-4ee1-a439-35982ca7ee75.png)


자바에 **Collection** **자료구조는 크게 List , Set , Map 3가지로 나눌 수 있다.**

![https://blog.kakaocdn.net/dn/cl5u0d/btq8wljpAC3/Epmb4I8fOVrvCCyaxGpjSk/img.png](https://blog.kakaocdn.net/dn/cl5u0d/btq8wljpAC3/Epmb4I8fOVrvCCyaxGpjSk/img.png)

그중 Collection 인터페이스를 상속하는 자료구조는 List , Set이며,

Map은 기본적으로 Key, Value 라는 다른 구조를 가지기 때문에, 독립적인 인터페이스가 구현되어 있다.

![Untitled 2](https://user-images.githubusercontent.com/55426397/180901017-ab974d69-8155-4ef0-90fe-72332a017301.png)

## 2. Collection 하위 인터페이스

### **1) List 인터페이스**

> index 라는 식별자로 순서를 가지며, 데이터의 중복을 허용하는 자료구조.
> 
- **ArrayList**
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남
    - 배열과 달리 초기 크기를 지정하지 않아도 되므로, 삽입 삭제가 자유로움
- **LinkedList**
    - 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치 정보만 수정하면 되기에 유용
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임
- **Vector**
    - ~~과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음~~

### **2) Set 인터페이스**

> 순서가 없고, 데이터 유일성을 가지는 자료구조.
> 
- **HashSet**
    - 가장 빠른 임의 접근 속도
    - 순서를 예측할 수 없음
- **TreeSet**
    - 정렬 방법을 지정할 수 있음

### **3) Map 인터페이스**

> 키(Key), 값(Value)의 쌍으로 이루어진 자료구조
순서가 없고, 키(Key)는 유일성을 가지고, 값(Value)의 중복은 허용한다.
> 
- **Hashtable**
    - HashMap보다는 느리지만 동기화 지원
    - null불가
- **HashMap**
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
- **TreeMap**
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름

[[JAVA] 컬렉션(Collection)이란?(추가 : Collecion의 요소 상세설명)](https://crazykim2.tistory.com/557)
