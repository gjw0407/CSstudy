# Array vs Linked List

## Array

**배열**: 동일한 자료형를 가지고 연속된 공간에 저장되는 데이터(element)의 집합

### 길이

배열의 길이는 고정이며 변경할 수 없다.

### 메모리 할당

- 배열의 값은 메모리에 연속적으로 저장된다.
    - 논리적 저장 순서 = 물리적 저장 순서
- 데이터의 자료형 크기 * 배열의 길이로 메모리의 크기가 결정된다.
- 자바 지역 변수는 Stack 영역에 저장되고, 배열은 실제로 Heap 영역에 레퍼런스형으로 할당되어 변수가 배열의 시작 주소(참조값)를 기억한다.
    - `new` 연산자(동적 메모리 할당 연산자)로 생성된 데이터 → heap
- 연속된 빈 공간이 없다면 생성되지 않는다.

![array](https://user-images.githubusercontent.com/55528172/173606813-47703052-0f8b-44ce-b264-ccddff47bea8.png)

### 선언 & 생성

- 선언: `int[] numbers`
- 생성: `numbers = new int[3]`
    - 배열을 처음 생성할 때 초기화를 할 값들의 집합 또는 배열크기를 지정해야 한다.

```java
// 1. 초기값으로 선언 및 생성
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};    // 1차원 배열

// 2. 배열 크기로 선언 및 생성
String[][] sentences = new String[3][10];    // 2차원 배열

// 3. 선언 후 초기화, 값의 개수에 의해 배열의 길이 자동 결정
Object[] objects;
objects = new Object[] {new Object(), new Object()};
```

### 접근

**Indexing**(원소의 일련번호)으로 배열의 원소에 무작위 접근(random access)이 가능하다.

```java
int[] numbers = new int[3];    // 자료형의 기본값으로 초기화됨
for (int i = 0; i < numbers.length; i++) {
	numbers[i] = i * i;    // 원하는 값으로 저장(덮어쓰기)
}

int n = numbers[2];    // index 2로 접근
```

### 삽입 & 삭제 & 검색

배열의 모든 인덱스에 접근해야 한다.

1. [1, 2, 3, 4, 5]의 2번째 자리(index = 1)에 0을 삽입하고 싶다.
    
    특정 원소를 삽입할 때 배열의 마지막 원소부터 뒤로 이동한다.
    
    index = 4 → [1, 2, 3, 4, 4]
    
    index = 3 → [1, 2, 3, 3, 4]
    
    index = 2 → [1, 2, 2, 3, 4] → [1, 0, 2, 3, 4]
    

1. [1, 2, 3, 4, 5]의 원소 2(index = 1)를 삭제하고 싶다.
    
    삭제하고자하는 원소의 index로 먼저 이동
    
    index = 1 → [1, 3, 3, 4, 5]
    
    index = 2 → [1, 3, 4, 4, 5]
    
    index = 3 → [1, 3, 4, 5, 5]
    

1. 특정 값 검색
    
    배열의 원소값이 정렬되어있지 않은 경우 첫번째 원소부터 순회하면서 검색하고자 하는 값과 비교해야 한다.
    

### 출력

```java
// 1. for문
for (int i = 0; i < numbers.length; i++) {
	System.out.print(numbers[i] + ", ");
}

// 2. Arrays method
System.out.println(Arrays.toString(numbers));
```

## Linked List

**연결 리스트**: 데이터와 포인터를 가진 노드(node)들이 한 줄로 연결된 자료구조

### 특징

- 원소 삽입, 삭제를 통해 연결 리스트의 길이가 변할 수 있다.
- 논리적인 저장 순서와 물리적 저장 순서가 일치하지 않다.
- Tree의 기본 자료구조

### 선언 & 생성 & 대표 메서드

```java
LinkedList objects = new LinkedList();    // 타입 설정 X, Object로 선언
LinkedList<Integer> numbers = new LinkedList<>();    // 생성시 자료형 생략 가능

LinkedList<String> sentences = new LinkedList<>(Arrays.asList("ab", "cd", "ef"));
sentences.add("gh");
sentences.remove("cd");    // value 기반 삭제
sentences.remove(0);    // index 기반 삭제
```

### 접근

특정 원소에 접근하기 위해 첫 노드부터 순차 접근(sequential access)이 필요하다. random access 접근 불가.

![linkedlist](https://user-images.githubusercontent.com/55528172/173606824-5990c22e-810c-4bb6-8017-258a03943dfd.png)

### 삽입 & 삭제 & 검색

Array(배열)처럼 삽입, 삭제시 전체 인덱스가 밀리거나 당겨지지 않으며, 앞뒤 노드만 영향을 받는다.

| 변경 지점 | 시간 복잡도 |
| --- | --- |
| search | O(n) |
| first, last 삽입/삭제 | O(1) |
| index 기반 삽입/삭제 | O(n) |
| clear | O(n) |

index를 기반으로 삽입/삭제를 하고자 한다면 원하는 위치를 찾는 검색 과정이 필요하다.

### 에러

자바의 경우 배열/리스트의 크기를 벗어나거나 음수 인덱스에 접근하려할 경우 `ArrayIndexOutOfBoundsException`을 런타임에 발생시킨다.

```java
int[] arrayNumbers = new int[n];    // index: 0 ~ n-1
// int errorIndexNumber = arrayNumbers[n];

List<Integer> listNumbers = new ArrayList<>();
// listNumbers.get(0);
```
