# Red-Black Tree

## 정의

**self-balancing** binary search tree (자가 균형 이진 탐색 트리)

- balanced: 트리 모양이 균형이 잡혀있다.
    - 일반 이진 탐색 트리의 경우 반복적으로 큰/작은 값만 추가되면 한 방향으로 치우칠 수 있다.
    - 트리의 높이 ≤ logn
        - search할 경우 O(logn)
- binary search tree: left subtree에는 자신보다 작은 값을, right subtree에는 자신보다 큰 값을 가져야 한다.

## 조건

아래 4가지 조건을 만족시켜주면 Red-Black tree는 높이가 logn에 bounded된다.

### 1. Root Property

Root node는 Black이다.

### 2. External Property

모든 Leaf node는 Black이다.

### 3. Internal Property

Red node의 자식은 Black이다. = No Double Red

### 4. Depth Property

모든 Leaf node에서 Black Depth는 같다. = Root부터 Leaf까지 경로에서의 Black node의 개수는 같다.

## 특징

- search, insert, delete의 시간복잡도는 모두 O(logn)
- 동일한 노드 개수일 때 complete binary tree로 만들어 depth를 최소화, 시간 복잡도를 줄인다.
- 위의 모든 조건을 만족시킬 때
    - leaf node까지 가장 짧은 거리: black - black - black - … - black → n
    - leaf node까지 가장 긴 거리: black - red - black - red - … - black → 2n-1
    - 둘의 최대 높이차는 2배 정도 → **balanced**
- 자식 노드가 없을 경우 NIL로, 이 NIL을 leaf node로 간주한다.
- 구현: C++의 Map, Java의 TreeMap

## 삽입

삽입되는 노드의 색은 무조건 Red이다. → Double Red의 상황이 생긴다. 어떻게 해결해야 할까?

새로 삽입한 노드의 Uncle node(부모 노드의 형제)의 색을 보고 결정한다.

### Restructuring

- Uncle node가 Black일 경우
- Double Red를 해결한 후와 전의 Black Depth는 동일하다. 다른 서브트리에 영향을 미치지 않는다. reconstructing은 1번만에 끝난다.
- time complexity
    - insertion 후 내 위치 찾기 = O(logn)
    - reconstructing = O(1)
    - 총 O(logn)

![reconstruct](https://user-images.githubusercontent.com/55528172/173755143-cfb29c65-caca-4ce1-888f-b239308dbda4.png)

1. inserted node, parent node, grand parent node(부모의 부모 노드)를 오름차순 정렬
2. 가운데 있는 노드를 부모로 만든 후, 나머지 두 노드를 자식으로 만든다.
3. 부모 노드를 Black으로, 자식 노드들을 Red로 칠한다.
4. 나머지 노드들 추가

![reconstruct_1](https://user-images.githubusercontent.com/55528172/173755146-a8cf637c-c4b7-4658-9914-3651ff90b5b7.png)
![reconstruct_2](https://user-images.githubusercontent.com/55528172/173755147-171a203e-582b-4e82-8c43-19a6ce2d150c.png)
![reconstruct_3](https://user-images.githubusercontent.com/55528172/173755148-e019c0e5-653b-4ab2-aafb-28ed33e876bd.png)
![reconstruct_4](https://user-images.githubusercontent.com/55528172/173755139-a8c73c25-1d4f-4b98-b7cc-ad6c16568947.png)

### Recoloring

- Uncle node가 Red일 경우
- 1번의 동작으로 끝나지 않고 전파될 수 있다.
- time complexity
    - insertion한 후 내 위치 찾기 = O(logn)
    - recoloring = O(1)
    - root까지 recoloring이 propagation될 경우 = O(logn)
    - 총 O(logn)

![recoloring](https://user-images.githubusercontent.com/55528172/173755164-7418e212-b66d-4884-8dcc-65ef895edf6b.png)

1. parent node, uncle node를 Black, grand parent node를 Red로 칠한다.
2. grand parent node가 root라면 Root Property에 의해 Black으로 변경
3. grand parent node가 root가 아닐 경우 double red 발생할 수 있다. double red가 해결될 때까지 root 방향으로 올라가며 reconstructing 반복

![recoloring_1](https://user-images.githubusercontent.com/55528172/173755170-768bb665-c186-42b0-ab8b-b24dc4f96be8.png)
![recoloring_2](https://user-images.githubusercontent.com/55528172/173755173-2d26139e-812a-44fc-bef6-5a42c44fd76f.png)
![recoloring_3](https://user-images.githubusercontent.com/55528172/173755174-cab2f918-64f6-4d83-a98e-1b1bdd400a94.png)
