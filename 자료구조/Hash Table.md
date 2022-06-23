# Hash Table

# Hash Table이란?

> 키(key)와 값(value)를 1:1 형태로 가져가며 Hash Table에 저장
> 
- 내부적으로 배열을 사용해 검색 속도가 빠름
- key 값에 해시함수를 적용해 index를 생성 → index 활용해 값을 저장 & 검색
- 버킷(슬롯) : 실제 값이 저장되는 장소

![Untitled](https://user-images.githubusercontent.com/61227459/175044438-200c7441-4fd3-4463-98d4-208358f96284.png)

## Hash Function

> 고유한 인덱스 값을 설정이 목표
> 
1. Division Method : 주소 = 입력값 % 테이블의 크기
2. Digit Folding : 각 Key의 문자열을 ASCII 코드로 바꾸고 값을 합한 데이터를 테이블 내의 주소로 사용
3. Multiplication Method : h(k)=(k*A*mod1) × m
    1. k = Key 값
    2. A = 0~1 실수
    3. m = 2의 제곱 수 
    4. mod 1 = k * A의 상수 부분 제거
4. Universal Hashing : 다수의 해시함수를 만들어 집합 H에 넣어두고, 무작위로 해시함수를 선택해 해시값을 만듦

## 시간복잡도

삽입, 수정, 삭제 → O(1)

이진트리탐색 → 위치 모르니 검색 → O(logn)

Linked List → 삽입, 삭제 시 다른 값이 영향 받음 → O(n+1)

## 단점

> 공간 효율성이 떨어짐 = 적재율(load factor, 해시 테이블에 원소가 차 있는 비율)이 낮음
> 

```java
Array(91) [
 0, 0, 0, 3, 0, 0, 0, 0, 0, 0, 10, 0, 0, 0, 0, 0, 0, 0, 0, 0,
 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 90]
```

- 값은 없지만 메모리 공간은 할당 되어 있음

# 해쉬 충돌

## 저장 / 조회

```java
// 16, 32 -> index = 0 
index = hash_function(“key”) % size_of_array
array[index] = value
```

## 원인

> 해시 테이블의 한 주소를 두 개 이상의 원소가 다툼
> 

## Separate Chaining

> 동일한 버킷의 데이터에 대해 자료구조를 활용해 추가 메모리를 사용해 데이터의 주소 저장
> 

### 데이터 추출

1. Index 검색
2. Index가 가르키는 Linked List를 선형 검색
3. key에 대한 데이터가 있는지 검색 후 반환

### 데이터 삭제

1. Index 검색
2. Index가 가르키는 Linked List를 선형 검색
3. Linked List의 노드 삭제

### 장점

- 해시 테이블의 확장이 필요 X
- 손쉽게 삭제 가능

### 단점

- 데이터의 수가 많으면 캐시의 효율성이 감소

<aside>
💡 JDK 1.8 : 노드가 8개 이하일 경우 Linked List 사용 / 8개 이상이면 RB Tree 구조 사용

</aside>

## Open Addressing

> Hash table array의 빈 공간을 사용 → 해싱 함수에 의존적
> 

### Linear Probing

- 충돌 발생 시 index 뒤에 있는 버킷중에 빈 버킷을 찾아 데이터 삽입
- 검색 시 index를 검색해서 같은 key가 나오거나 key가 없을 때까지 검색

### Quadratic Probing

- 충동 발생 시 해시의 제곱을 한 해시에 데이터 저장
    - $1^2$→ $2^2$→ $3^2$ 순으로 검색

### 이중 해싱

- 다른 해시함수를 한 번 더 적용해 나온 해시에 데이터를 저장
- 추가적인 저장공간 없이 해시 테이블 내에서 데이터의 저장 및 처리 가능

## Resizing

- Open Addressing & Separate Chaining 할 때 버킷을 늘려야 할 때 Resizing
- 더 큰 버킷을 가지는 array를 만들어 hash를 다시 계산해서 복사해줘야 함

# JAVA

## HashMap vs HashTable

### HashMap

- Java 2에서 처음 선보인 Java Collections Framework
- 보조 해시 함수 사용
    - key의 해시값을 변경해 해시 충돌 가능성을 줄임
    - Java 8부터 적용되는 보조 해시 함수 (이전에도 존재했었음)
    
    ```java
    static final int hash(Object key) { 
    	int h;
      // 상위 16비트 값을 XOR 연산
    	return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); 
    }
    ```
    
- 초기 용량 16, 로드 팩터는 0.75가 디폴트
    - 로드팩터 = (데이터의 개수)/(초기 용량)
    - 버킷의 75%가 차면 용량을 2배로 늘림 → resize
- Thread-safe
    - 동기화 지원하지 않음
        - 동기화 위해 사용되는 코드
        - `Map m = Collections.synchronizedMap(new HashMap(...));`
- Null 허용

### HashTable

- JDK 1.0부터 있던 API
- JRE 1.0, JRE1.1 환경을 대상으로 구현한 Java 애플리케이션이 잘 동작할 수 있도록 하위 호환성을 제공
- Thread-safe X
- Null 허용 X

# 출처

[https://mangkyu.tistory.com/102](https://mangkyu.tistory.com/102)

[https://memostack.tistory.com/233](https://memostack.tistory.com/233)

[https://devlog-wjdrbs96.tistory.com/253](https://devlog-wjdrbs96.tistory.com/253)

[https://odol87.tistory.com/4](https://odol87.tistory.com/4)

[https://woooongs.tistory.com/68](https://woooongs.tistory.com/68)

[https://bcho.tistory.com/1072](https://bcho.tistory.com/1072)

[https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311)

[https://velog.io/@adam2/자료구조해시-테이블](https://velog.io/@adam2/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94)