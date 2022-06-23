# Graph

# Graph 개념

> 정점(Vertex)과 간선(Edge)으로 이루어져 정점간의 관계를 표현
> 

## 용어

- 인접 정점(adjacent vertex) : 간선에 의해 직접 연결된 정점
- 정점의 차수(degree) : 무방향 그래프에서 하나의 정점에 인접한 정점의 수
    - 무방향 그래프 정점의 모든 차수의 합 = 그래프의 간선 수 * 2
- 진입 하수(in-degree) : 방향 그래프에서 외부에서 오는 간선의 수
- 진출 차수(out-degree) : 방향 그래프에서 외부로 향하는 간선의 수
- 단순 경로(simple path) : 경로 중에서 반복되는 정점이 없음
- 사이클(cycle) : 단순 경로의 시작 정점 = 종료 정점

## 특징

- 네트워크 모델 → 객체들 간 관계를 나타내는 유연한 방식
- 무방향 / 방향 경로
- 루트 노드 / 부모-자식 X

## 구현

### 인접 행렬(Adjacency Matrix)

- 2차원 배열로 나타냄
- 간선 있으면 1, 없으면 0
- 간선 조회 시간 복잡도 : O(1)
- 배열의 크기가 항상 $n^2$ → 메모리 공간 낭비
    - 최악 시간 복잡도 : O($n^2$)

### 인접 리스트(Adjacency List)

- 연결리스트(Linked List)로 표현
- 정점의 개수만큼 인접 리스트 존재
- 메모리 낭비 없음
- 시간 복잡도 : O(n)
- 간선 조회가 오래 걸림

## 종류

- 무방향
- 방향
- 가중치
- 연결
    - 모든 정점 쌍에 대해 항상 경로가 존재 (Tree)
- 비연결
- 완전
    - 모든 정점이 서로 연결 (인접 연결)
- 순환
- 비순환
- 신장트리
    - 모든 노드가 연결되어 있음
    - 사이클 존재 X
    - n개의 정점을 n-1개의 간선으로 연결
- 최소 신장트리

## 탐색

### DFS(깊이 우선 탐색)

- 모든 노드를 방문을 목적

### BFS(너비 우선 탐색)

- 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾는 것을 목표

# 최소 신장트리(Minimum Spanning Tree)

> 가중치의 합이 최소가 되는 신장 트리
> 

## 크루스칼 알고리즘 (Kruskal’s algorithm)

- Greedy를 이용
- 간선 선택 기반
- 만들어진 신장 트리와는 상관없이 최소 간선만을 선택
- 간선이 적으면 적합 (희소 그래프)
- 시간 복잡도 : O(MlogM)

### 알고리즘

1. 간선들을 가중치의 오름차순으로 정렬
2. 낮은 가중치부터 선택하되 사이클을 형성하지 않음
    1. 분리 집합 (Disjoint Set) 사용
    
    [Disjoint Set](Disjoint%20Set.md)
    

### 코드

```java
public class Kruskal {
	static int V, E;
	static int[][] graph;
	// 각 노드의 부모
	static int[] parent;
	// 최종적으로 연결된 최소 신장 트리 연결 비용.
	static int final_cost;

	public static void main(String[] args) {
		// 그래프의 연결상태(노드1, 노드2, 비용)를 초기화.
		Scanner sc = new Scanner(System.in);
		V = sc.nextInt();
		E = sc.nextInt();
		graph = new int[E][3];
		for (int i = 0; i < E; i++) {
			graph[i][0] = sc.nextInt();
			graph[i][1] = sc.nextInt();
			graph[i][2] = sc.nextInt();
		}
		parent = new int[V];
		final_cost = 0;

		// 간선 비용 순으로 오름차순 정렬
		Arrays.sort(graph, (o1, o2) -> Integer.compare(o1[2], o2[2]));

		// makeSet
		for (int i = 0; i < V; i++) {
			parent[i] = i;
		}
		// 낮은 비용부터 크루스칼 알고리즘 진행
		for (int i = 0; i < E; i++) {
			// 사이클이 존재하지 않는 경우에만 간선을 선택한다(여기서는 최종 비용만 고려하도록 하겠다).
			if (find(graph[i][0] - 1) != find(graph[i][1] - 1)) {
				System.out.println("<선택된 간선>");
				System.out.println(Arrays.toString(graph[i]));
				union(graph[i][0] - 1, graph[i][1] - 1);
				final_cost += graph[i][2];
				System.out.println("<각 노드가 가리키고 있는 부모>");
				System.out.println(Arrays.toString(parent) + "\n");
				continue;
			}
		}
		
		System.out.println("최종 비용 : " + final_cost);
		sc.close();
	}

	private static void union(int a, int b) {
		a = find(a);
		b = find(b);
		if (a > b) {
			parent[a] = b;
		} else {
			parent[b] = a;
		}
	}

	private static int find(int x) {
		if (parent[x] == x)
			return x;
		else
			return find(parent[x]);
	}
}
```

## 프림 알고리즘 (Prim’s algorithm)

- 시작 정점에서부터 출발해 신장트리 집합을 단계적으로 확장
- 노드 중심적
- 간선이 많이 존재하면 유리 (밀집 그래프)
- 시간 복잡도 : O($N^2$)

### 알고리즘

1. 시작 정점만 MST 집합에 포함
2. MST 집합에 인접한 정점들 중에서 최소 간선으로 연결된 정점을 선택해 트리 확장
3. n-1개의 간선을 가질 때까지 반복

### 코드

```java
public static class Edge implements Comparable<Edge> {
		int node;
		double dis;

		public Edge(int node, double dis) {
			this.node = node;
			this.dis = dis;
		}
		@Override
		public int compareTo(Edge o) {
			return Double.compare(this.dis, o.dis);
		}
}

static int V,E, ans = 0, cnt = 0;
static ArrayList<ArrayList<Edge>> graph;
static boolean[] visit;
static PriorityQueue<Edge> pq;

public static void main(String[] args) throws Exception {
	System.setIn(new FileInputStream("test.txt"));
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer st = new StringTokenizer(br.readLine());

	pq = new PriorityQueue<>();
	
  //정점의 수
	V = Integer.parseInt(st.nextToken());
  //간선의 수
	E = Integer.parseInt(st.nextToken());
	visit = new boolean[V];
	
	//정점의 연결을 저장할 리스트
	graph = new ArrayList<>();
	
	for (int i = 0; i < V; i++) {
		graph.add(new ArrayList<>());
	}
	
	for (int i = 0; i < E; i++) {
		st = new StringTokenizer(br.readLine());
		int A = Integer.parseInt(st.nextToken()) -1;
		int B = Integer.parseInt(st.nextToken()) -1;
		int dis = Integer.parseInt(st.nextToken());
		
		graph.get(A).add(new Edge(B, dis));
		graph.get(B).add(new Edge(A, dis));
	}
	//임의의 정점(0)부터 시작한다.
	pq.add(new Edge(0,0));
	
	while (!pq.isEmpty()) {
		Edge edge = pq.poll();
		//방문했다면 continue;
		if(visit[edge.node]) continue;
		visit[edge.node] = true;
		//거리 증가
		ans += edge.dis;
		//그래프에 연결된 노드를 돌면서 방문하지 않았다면 pq에 넣어준다.
		for (Edge next : graph.get(edge.node)) {
			if(!visit[next.node]) {
				pq.add(next);
			}
		}
		//모든 정점을 순회하였다.
		if (++cnt == V) break;
	}
	System.out.println(ans);
}
```

# 출처

[https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)

[https://ssabi.tistory.com/60](https://ssabi.tistory.com/60)

[https://hongcoding.tistory.com/78](https://hongcoding.tistory.com/78)

[https://coding-factory.tistory.com/610](https://coding-factory.tistory.com/610)

[https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html)

[https://sskl660.tistory.com/72](https://sskl660.tistory.com/72)

[https://ghkvud2.tistory.com/98](https://ghkvud2.tistory.com/98)