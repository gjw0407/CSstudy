# Disjoint Set

# 정의

> “상호 배타적”인 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조
> 

→ 교집합이 없는 서로 다른 데이터들로 이루어진 자료구조

→ 서로소

# 연산

## Make-Set(x)

- 원소 x에 대해 독립된 집합을 만듦
- 초기 설정
    - 각 노드들은 자기 자신을 부모노드로 가짐
- 시간 복잡도 : O(N)

## Find(x)

- x가 속한 부분집합의 루트 노드를 반환
- 각각의 노드들의 부모 노드가 같은지 확인 해 같은 집합에 속해 있는지 확인
- 시간 복잡도 : O(h) (Tree 높이에 비례)

```java
public static int find(int[] parent, int u) { 
    if (u == parent[u])
        return u;
 
    return find(parent, parent[u]);
}
```

### Path Compression

- 트리의 높이를 낮춤으로 인해 시간 복잡도를 줄임

```java
// 부모를 찾아가는 과정에서 자기가 속해있는 부모노드의 값을 바꿈
public static int findPathCompression(int[] parent, int u) { 
        if (u == parent[u])
            return u;
        return parent[u] = find(parent, parent[u]);
}
```

## Union(x,y)

- x가 속한 부분 집합과 y의 부분 집합 합침

```java
// 높이가 낮은 트리로 합침 -> 시간 복잡도 최소화
public static void union(int[] parent, int[] rank, int u, int v) { 
        int uRoot = find(parent, u);
        int vRoot = find(parent, v);
 
        if (uRoot == vRoot)
            return;
 
        int uRank = rank[uRoot];
        int vRank = rank[vRoot];
 
        if (uRank < vRank)
            parent[uRoot] = vRoot;
        else if (uRank > vRank)
            parent[vRoot] = uRoot;
        else {
            parent[uRoot] = vRoot;
            rank[vRoot] += 1;
        }     
}
```

## Union-Find

- 시간 복잡도 : O(N + Mlog*N) → O(1)
    - log*N : N에 몇 번의 log를 취했을 때, 그 값이 1이 나오는지 반환
    - ex) N = 65536 → log*N = 4
    - 즉, 수가 아무리 커도 O(1)

# 출처

[https://ghkvud2.tistory.com/98](https://ghkvud2.tistory.com/98)