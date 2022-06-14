# Stack and Queue

## Stack

- 쌓다, 더미 → 데이터를 쌓는 자료 구조
- Last In First Out(LIFO, 후입선출)
- 한 쪽 끝에서만 데이터를 넣고 뺄 수 있다.

![stack](https://user-images.githubusercontent.com/55528172/173641349-c1198945-019f-4a02-bbca-0432628a8570.png)

### 대표 메서드

```java
import java.util.Stack;

Stack<Integer> stack = new Stack<>();

stack.push(1);    // 삽입
stack.push(2);
Integer topElement = stack.pop();    // 가장 최근에 추가된 원소(top) 삭제, 반환
stack.peek();    // 가장 최근에 추가된 원소 조회
boolean isEmpty = stack.empty();
Integer searchedElement = stack.search(1);    //  top으로부터 parameter에 일치하는 데이터의 위치 반환
int stackSize = stack.size();
int index = stack.indexOf(1);
boolean doesExist = stack.contains(2);
```

### 예제

- 인터럽트 처리
- 수식 계산
- Depth First Search(DFS, 깊이 우선 탐색)
- 재귀 함수

## Queue

- 줄, 대기 행렬 → 데이터가 줄을 지어 순서대로 처리되는 자료구조
- First In First Out(FIFO, 선입선출)
- front는 삭제 연산만, rear는 삽입 연산만 수행

![queue](https://user-images.githubusercontent.com/55528172/173641341-62ed6519-1fb1-4e91-968a-59ffeed0a215.png)

### 대표 메서드

```java
import java.util.Queue;
import java.util.LinkedList;

Queue<Integer> queue = new LinkedList<>();

boolean isAddSuccess = queue.add(1);    // 삽입에 성공했는지 여부(성공시 true, 실패시 IllegalStateException)
boolean isOfferSuccess = queue.offer(2);    // add()와 동일
Integer polledElement = queue.poll();    // front의 원소 삭제 후 반환, queue가 비어있다면 return null
Integer removedElement = queue.remove();    // queue가 비어있다면 NoSuchElementException
Integer frontElement = queue.peek();    // front의 원소 조회
```

### 예제

- Breadth First Search(BFS, 너비 우선 탐색)
- 컴퓨터 구조의 buffer

### Priority Queue

- 큐에 들어온 순서대로 데이터가 나가는 것이 아니라 우선순위를 지정하여 순위가 높은 원소가 먼저 나가는 자료구조
- heap(이진트리 구조) 사용
    - O(logn)

```java
Queue<Integer> priorityQueue = new PriorityQueue<>();    // 낮은 숫자가 높은 우선순위
Queue<Integer> reversePriorityQueue = new PriorityQueue<>(Collections.reverseOrder());    // 높은 숫자가 높은 우선순위
```
