### 1. Element 검색

<Array>

- **Random Access**를 지원.
  - element들을 **index**를 통해 직접적으로 접근 가능.
  - 특정 element에 접근하는 시간 복잡도는 **O(1)**으로 빠르게 찾을 수 있음

<Linked List>

- **Sequential Access**를 지원
  - 어떤 element/node를 접근할 때 **처음부터 찾는다.** element에 **도달할 때까지 순차적으로 검색**하면서 찾아야 함
  - 특정 element에 접근할 때의 시간 복잡도는 **O(n)**

### 2. 저장 방식

<Array>

- element 들은, **인접한 memory 위치에 저장됨**

<Linked List>

- LinkedList 의 element 들은, **memory 어딘가에 저장됨**
  - 새로운 element 의 memory **위치 주소**는, linked list 의 **이전 node 에 저장**됨

### 3. 삽입, 삭제

<Array>

- memory 위치가 **연속적**이고 **고정적**이기 때문에 삽입, 삭제 연산에 많은 시간을 소모함.

<Linked List>

- 새로운 element는 **첫 번째** **사용 가능한 빈 Memory 위치**에 저장되며, **이전 Node**에 **Memory 위치의 주소**를 저장하는 단 하나의 overhead만이 존재 → 따라서 삽입, 삭제 연산이 빠름

### 4. 메모리 할당

<Array>

- Memory 는 Array 가 **선언**되자 마자 **Compile time** 에 할당되어짐
  - 이것을 **Static Memory Allocation** 이라고 부름
  - **Stack** section 에 memory 할당이 이루어짐

<Linked List>

- Memory 는 **새로운 node 가 추가**될 때 **run time** 에 할당되어짐
  - 이것을 **Dynamic Memory Allocation** 이라고 부름
  - **Heap** section 에 memory 할당이 이루어짐

### 5. 크기

<Array>

- 크기가 **반드시** array **선언** 시점에 **지정되어있어야 합니다.**

<Linked List>

- 크기는 **다양**할 수 있습니다
  - node 들이 추가될 때 **runtime 시점**에서 LinkedList **크기는 커질 수 있음**

### 6. 종류

<Array>

- single dimensional, two dimensional, multidimensional

<Linked List>

- Linear(Singly), Doubly, Circular linked list
