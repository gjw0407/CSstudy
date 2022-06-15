# Graph

노드(Node)와 노드를 연결하는 간선(Edge)을 하나로 모아 놓은 자료구조

### 그래프 vs 트리

| 그래프 | 트리 |
| --- | --- |
| 방향(directed), 무방향(undirected) | 방향(directed) |
| 자체 간선(self-loop), 순환(cyclic), 비순환(Acyclic) | 비순환(acyclic) |
| X | root node |
| X | parent-child relationship |
| 네트워크 모델 | 계층 모델 |
| DFS, BFS | DFS, BFS의 pre-order, in-order, post-order |
| 간선의 수는 모두 다름 | 노드 N개 트리의 간선의 수는 N-1 |
| - | 두 노드간 유일 경로 |
| 지도, 최단 경로, 도로 | 이진트리, BST, RBT, heap |

## 용어 정리

- 정점 vertex: 위치, node
- 간선 edge: 노드를 연결하는 선, link
- 인접 정점 adjacent vertex: 간선으로 직접 연결된 정점
- 정점의 차수 degree: 무방향 그래프에서 한 정점에 인접한 정점의 수
- 진입 차수, 내차수 in-degree: 방향 그래프에서 외부에서 들어오는 간선의 수
- 진출 차수, 외차수 out-degree: 방향 그래프에서 외부로 향하는 가선의 수
- 경로 길이 path length: 경로를 구성하는데 사용된 간선의 수
- 단순 경로 simple path: 경로 중에서 반복되는 정점이 없는 경우
- 사이클 cycle: 단순 경로의 시작 정점과 종료 정점이 동일한 경우

## 구현

![graph](https://user-images.githubusercontent.com/55528172/173873506-a0adde6f-26e4-4086-9e64-262d726f4b46.png)

### 1. 인접 행렬 Adjacency Matrix

- n^2의 메모리 공간
- 무방향 그래프는 대칭 행렬(symmetric matrix)이 된다
- 인접한 정점을 찾기 위해 모든 노드를 순회해야 한다. (효율성 떨어짐)

![adjacency_matrix](https://user-images.githubusercontent.com/55528172/173873504-9e16dda4-38c8-42db-9342-7b2af4ef1a35.png)

### 2. 인접 리스트 Adjacency List

- 각 정점에 인접한 정점들을 리스트로 표현
- 희소 그래프(sparse graph, 적은 숫자의 간선을 가지는 그래프)인 경우
- 한 노드에 인접한 노드들을 쉽게 찾을 수 있다.

![adjacency_list](https://user-images.githubusercontent.com/55528172/173873498-f46de759-d3c3-4216-a482-5df461036484.png)

## 탐색

### 1. 깊이 우선 탐색 Depth-First Search(DFS)

- 모든 노드를 방문하고자할 경우
- 임의의 노드에서 시작해서 다음 branch로 넘어가기 전 해당 분기를 완벽하게 탐색한다.

### 2. 너비 우선 탐색 Breadth-First Search(BFS)

- 두 노드 사이의 최단 경로, 임의의 경로
- 임의이 노드에서 시작해서 인접한 노드들부터 먼저 탐색한다.

## 최소 신장 트리 Minimum Spanning Tree

### 신장 트리 Spanning Tree

- 그래프 내의 모든 정점을 포함하는 트리
- 그래프의 최소 연결 부분 그래프, 일부 간선만 선택
- 사이클을 포함하지 않는다.
- n개의 정점, n-1개의 간선
- MST: 신장 트리 중 간선들의 **가중치 합이 최소**인 트리

### Kruskal algorithm

- greedy algorithm → 각 단계에서 사이클을 이루지 않는 최소 비용 **간선을 선택**한다. (간선 기준)

1. 모든 간선들을 가중치를 기준으로 오름차순 정렬한다.
2. 사이클을 제외하지 않는 간선들을 순서대로 선택한다.
3. 선택한 간선을 MST 집합에 추가한다.

### Prim algorithm

- 신장트리 집합을 **정점 기반**으로 단계적으로 확장해나간다.

1. 시작 정점을 MST 집합에 추가한다.
2. MST 집합에 포함된 정점들 중 인접 정점들로의 간선 중 가중치가 가장 낮은 간선을 선택하여 인접 정점을 MST 집합에 추가한다.
3. 2번 과정을 트리가 N-1개 간선을 가질 때까지 반복한다.
