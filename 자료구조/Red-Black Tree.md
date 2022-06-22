# Red-Black Tree

# Binary Search Tree

[Binary Search Tree](Binary%20Search%20Tree.md)

최악의 경우, O(h), 한 쪽으로만 치우쳐진 상황 발생

→ 이를 해결하기 위해 RB Tree 탄생

# Balanced Binary Search Tree

## 시간 복잡도

O(logn)

## 조건

1. Root Property : 루트 노드의 색깔은 검정
2. External Property : 모든 external node(리프 노드)들은 검정
3. Internal Property : 빨강 노드의 자식은 검정 (No Double Red)
    1. 검정이 두 번 나와도 됨
4. Depth Property : 모든 리프(leaf) 노드에서 Black Depth는 같다 
    1. 리프 노드 → 루트 노드까지 가는 경로에서 만나는 블랙 노드의 개수는 같다

## NIL Node (null leaf node)

자식 노드가 존재하지 않은 경우 → NIL Node라 지칭

모든 NIL 리프에 대한 포인터가 이 노드를 가리키도록 하면 된다 → 공간도 절약 & 경계 조건을 다루기도 편리

![Untitled](https://user-images.githubusercontent.com/61227459/175044357-4282c2dc-2fda-444c-9b1c-5b397d1564c5.png)


# Restructuring vs Recoloring

> 노드를 삽입하는 중 Internal Property를 위반할 때 쓰는 해결책
Uncle node(부모 노드와 같은 층)의 색깔에 따라 갈림
> 
- Restructuring : Uncle(NIL 포함)이 검정
- Recoloring : Uncle이 빨강

![Untitled 1](https://user-images.githubusercontent.com/61227459/175044264-c5d4a1a4-c49e-47a3-866d-d96938ec9181.png)

## Restructuring

1. 새로운 노드, 부모 노드, 조상 노드를 오름차순으로 정렬
2. 중간 값 = 부모, 검정 / 나머지 = 자식, 빨강

## Recoloring

1. 부모, 삼촌 → 검정 / 조상 →빨강
    1. if 조상이 루트 → 검정
    2. if 조상 Double Red, 다시 Restructuring or Recoloring

> Q : 부모 노드랑 uncle 노드를 검정으로 바꿔도 Depth Property 만족?

A : 네. 만족합니다. Black Depth는 일제히 1증가하기 때문에 문제가 없음
> 

# 코드

## Node

```java
private static class Node {
		private int value;
		private int color;
		
		Node left;
		Node right;
		Node parent;
		
		Node(int value) {
			this.value = value;
			color = BLACK;
			
			left = null;
			right = null;
			parent = null;
		}
		
		Node() {
			this(-1);
		}
		
		int getValue() {
			return value;
		}
		
		String getColor() {
			return color == RED ? "RED" : "BLACK";
		}
		
		void setColor(int color) {
			this.color = color;
		}
	}
```

## Insert

```java
public static void insertNode(Node node) {
    System.out.println("Inserted " + node.getValue());

    // 트리가 비었는지 확인
    if(root == null) {
        // 루트가 없는 경우, 검은색 루트 노드를 새로 만들며 시작 (조건 1.1.)
        node.setColor(BLACK);
        root = node;
    }
    else {
        // 루트 노드가 있는 경우, 빨간색 리프 노드를 삽입 (조건 1.2.)
        node.setColor(RED);
        Node parent = root;

        // 기본 이진트리 규칙대로 노드 추가하기 (빨간색 리프 노드의 자리 찾기)
        while(true) {
            // 노드의 값이 부모보다 크다면 계속 오른쪽 자식을 부모로 접근
            if(node.getValue() > parent.getValue()) {
                // 부모의 오른쪽 자식 자리가 비었다면, 오른쪽 자식이 됨
                if(parent.right == null) {
                    parent.right = node;
                    node.parent = parent;
                    break;
                }
                else {
                    // 트리에서 한줄기 오른쪽으로 내려와서 빈공간 찾기 (while)
                    parent = parent.right;
                }
            }
            // 노드의 값이 부모보다 작다면 계속 왼쪽 자식을 부모로 접근
            else {
                // 부모의 왼쪽 자식 자리가 비었다면, 왼쪽 자식이 됨
                if(parent.left == null) {
                    parent.left = node;
                    node.parent = parent;
                    break;
                } else {
                    // 트리에서 한줄기 왼쪽으로 내려와서 빈공간 찾기 (while)
                    parent = parent.left;
                }
            }
        }

        // 새 노드의 부모가 빨간색일 때만 새 노드의 부모의 형제 확인 및 재색칠, 회전 적용 (조건 1.2.2.)
        if(node.parent != null && "RED".equals(node.parent.getColor())){
            recolorTree(node);
        }
    }
}
```

## Recoloring

```java
public static void recolorTree(Node node) {
    System.out.println("RedBlackTreeTest.recolorTree");

    // 부모의 색이 빨간색이 아닐 때까지 재색칠을 할 것이다.
    while (node.parent != null && "RED".equals(node.parent.getColor())) {
        Node siblingOfParent = null;

        // 해당 노드 부모가 조부모 노드의 왼쪽노드 자식이라면 (회전 방향 정하기)
        if(node.parent == node.parent.parent.left) {
            // 부모의 형제는 부모의 부모의 오른쪽노드 자식이다. (부모는 왼쪽 노드 자식이기 때문)
            siblingOfParent = node.parent.parent.right;

            // 부모의 형제가 빨간색이라면 (1.2.2.2)
            if(siblingOfParent != null && "RED".equals(siblingOfParent.getColor())) {
                node = whenSiblingOfParentIsRed(node, siblingOfParent);
                continue;
            }

            // 내 부모의 형제가 검은색이라면 (1.2.2.1) -> 회전 + 재색칠
            else {
                // 내가 부모 기준 우측에 있는 자식이라면, 좌측 회전
                if(node == node.parent.right) {
                    node = node.parent;
                    rotateLeft(node);
                }

                // 부모의 색이 빨간색인 것이 실행 조건이었으니,
                // 내 부모의 색은 검은색을 만들어줌 (재색칠)
                node.parent.setColor(BLACK);
                // 내 부모의 부모는 빨간색이 됨 (재색칠)
                node.parent.parent.setColor(RED);

                // 내 부모의 부모를 우측 회전시키기
                rotateRight(node.parent.parent);
                break;
            }
        }

        // 내 부모가 / 조부모 노드의 오른쪽 자식이라면 (회전 방향 정하기)
        else {
            // 부모 노드의 형제는 부모 노드의 부모 노드의 왼쪽 자식이다. (부모는 오른쪽 노드 자식이기 때문)
            siblingOfParent = node.parent.parent.left;

            // 내 부모의 형제가 빨간색이라면 (1.2.2.2)
            if(siblingOfParent != null && "RED".equals(siblingOfParent.getColor())) {
                node = whenSiblingOfParentIsRed(node, siblingOfParent);
                continue;
            }

            // 내 부모의 형제가 검은색이라면 (1.2.2.1) -> 회전 + 재색칠
            else {
                // 내가 부모 기준 좌측에 있는 자식이라면, 우측 회전
                if(node == node.parent.left) {
                    node = node.parent;
                    rotateRight(node);
                }

                // 부모의 색이 빨간색인 것이 실행 조건이었으니,
                // 내 부모의 색은 검은색을 만들어줌 (재색칠)
                node.parent.setColor(BLACK);
                // 내 부모의 부모는 빨간색이 됨 (재색칠)
                node.parent.parent.setColor(RED);

                // 내 부모의 부모를 좌측 회전시키기
                rotateLeft(node.parent.parent);
                break;
            }
        }
    }

    root.setColor(BLACK);
}

public static Node whenSiblingOfParentIsRed(Node node, Node siblingOfParent) {
    // 부모를 검정으로 만듦 (부모의 형제가 빨간색인 것이 밝혀졌으니, 부모도 빨간색일 것임)
    node.parent.setColor(BLACK);
    // 부모의 형제도 검정으로 만듦
    siblingOfParent.setColor(BLACK);
    // 부모의 부모는 빨강으로 만듦 (이래야 5. 조건이 만족됨)
    node.parent.parent.setColor(RED);

    //부모의 부모를 반환하고, 다시 반복해서 부모의 부모의 부모가 또 빨간색이면 같은 행위(재색칠)를 반복해야 함
    return node.parent.parent;
}
```

## RotateRight

```java
private static void rotateRight(Node node) {
    System.out.println("RedBlackTreeTest.rotateRight");

    if(node.parent == null) {
        Node left = root.left;
        root.left = root.left.right;
        left.right = new Node();
        left.right.parent = root;
        root.parent = left;
        left.right = root;
        left.parent = null;
        root = left;
    }

    // 부모가 존재한다면,
    else {
        // 해당 노드가 왼쪽 자식이라면 (해당 노드가 부모 노드보다 작다면)
        // 해당 노드의 왼쪽 자식도 부모 노드보다 작음
        if(node == node.parent.left) {
            node.parent.left = node.left;
        }
        // 해당 노드가 오른쪽 자식이라면 (해당 노드가 부모 노드보다 크다면)
        // 해당 노드의 왼쪽 자식도 부모 노드보다 큼
        // (조부모 노드보다 작았다면 조부모노드의 왼쪽 방향에 삽입되어야 하기 때문)
        else {
            node.parent.right = node.left;
        }

        node.left.parent = node.parent;
        node.parent = node.left;

        if(node.left.right != null) {
            node.left.right.parent = node;
        }

        node.left = node.left.right;
        node.parent.right = node;
    }
}
```

# JAVA

- Collection → ArrayList의 내부적인 알고리즘이 RBT
- HashMap의 Separate chaining(LinkedList로 Hash 충돌 해결)에서 사용됨

# 출처

[https://code-lab1.tistory.com/10](https://code-lab1.tistory.com/10)

[https://minhamina.tistory.com/97](https://minhamina.tistory.com/97)

[https://junboom.tistory.com/18](https://junboom.tistory.com/18)