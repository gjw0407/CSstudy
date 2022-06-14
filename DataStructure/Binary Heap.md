# Binary Heap

### 특징

- binary heap = heap
- 완전 이진 트리(complete binary tree) 기반 자료구조
- 최댓값, 최솟값을 빠르게 찾아낼 수 있다.
- 부모, 자식 노드의 데이터는 대소관계가 성립한다.
    - 형제 관계에는 대소관계가 정해지지 않는다.
- 예시 - 우선순위 큐, 다익스트라 알고리즘

### 종류

1. 최대 힙 (max heap)
    - 부모 노드의 데이터가 자식노드의 데이터보다 크거나 같다.
    - 루트 노드가 가장 큰 값을 가진다.
    
    ![maxheap_tree](https://user-images.githubusercontent.com/55528172/173660643-c8fe2cb1-bbcf-4ede-9e96-c38e08bbc9d2.png)


    
2. 최소 힙 (min heap)
    - 부모 노드의 데이터가 자식 노드의 데이터보다 작거나 같다.
    - 루트 노드가 가장 작은 값을 가진다.
    
    ![minheap_tree](https://user-images.githubusercontent.com/55528172/173660648-012600fc-1e80-498d-a7d0-5ceb9bf6f4f0.png)

    

### 표현

배열로 표현하며, 편의성과 가독성을 위하여 루트 노드의 index를 1로 설정한다.

- 부모 노드 인덱스: i
- 왼쪽 자식 노드 인덱스: 2 * i
- 오른쪽 자식 노드 인덱스: 2 * i + 1

![maxheap_array](https://user-images.githubusercontent.com/55528172/173660650-1d9b2e7e-c76c-493f-80a8-743ea097edf6.png)

### 동작

- Heapify: 이진트리에서 힙 속성(heap-order property)을 유지하는 작업
- insert
    1. 트리 가장 낮은 레벨의 빈 공간, 배열의 마지막 인덱스에 추가한다.
    2. 힙 속성에 부합하는지 leaft node부터 위로 부모 노드와 자식 노드의 데이터를 비교하며 값을 교환한다.
    3. 만족할 때까지 계속 반복
- remove
    1. 루트 노드를 삭제 후 반환
    2. 가장 마지막 노드를 루트 노드로 이동한다.
    3. 힙 속성에 부합하는지 root node부터 내려가며 부모 노드와 자식 노드의 데이터를 비교하며 자식들 중 더 작은/큰 데이터를 가진 자식과 교환한다.
    4. 만족할 때까지 계속 반복

### 복잡도 비교

|  | search | insert | remove | max/min |
| --- | --- | --- | --- | --- |
| sorted array | O(logn) | O(n) | O(n) | O(1) |
| linked list | O(n) | O(1) | O(1) | O(n) |
| binary heap | O(logn) | O(logn) | O(logn) | O(1) |
