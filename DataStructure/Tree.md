# Tree

- 선형 구조(Array, LinkedList, Stack, Queue)가 아닌 계층형 그래프
- 노드가 하나 이상의 자식 노드를 가진다
- 회로가 없으며 한 노드에서 다른 노드로 가는 방법은 한 가지이다
- root node(최상위 노드), leaf node(자식 노드가 없는 노드), internal node

## Binary Tree (이진 트리)

부모 노드가 최대 2개의 자식 노드를 가지는 트리

## Binary Search Tree (BST, 이진 검색 트리)

왼쪽 자식 노드 포함 아래의 모든 노드의 데이터가 현재 노드의 데이터보다 작고, 오른쪽 자식 노드 아래의 모든 노드의 데이터가 현재 노드의 데이터보다 클 경우

## Full Binary Tree (정 이진 트리)

모든 노드들이 0개 혹은 2개의 자식만 가질 경우

## Complete Binary Tree (완전 이진 트리)

마지막 레벨을 제외한 모든 서브트리의 레벨이 같고, 마지막 레벨은 왼쪽부터 채워진 경우

## Perfect Binary Tree (포화 이진 트리)

마지막 레벨을 제외한 모든 노드가 2개의 자식 노드를가져서 피라미드 모양을 만드는 경우

## Binary Tree Traversal

- Left Child Node → Right Child Node의 순회 순서는 유지하되, Parent Node를 처음, 중간, 마지막에 순회할 것이냐를 정한다.
- 재귀 함수로 트리 순회 메서드 구현 가능

1. 전위 순회 (preorder)
    
    Root → Left → Right
    
    ```java
    void preorder(Node node) {
    	if (node != null) {
    		System.out.println(node.data);
    		preorder(node.left);
    		preorder(node.right);
    	}
    }
    ```
    
2. 중위 순회 (inorder)
    
    Left → Root → Right
    
    ```java
    void inorder(Node node) {
    	if (node != null) {
    		inorder(node.left);
    		System.out.println(node.data);
    		inorder(node.right);
    	}
    }
    ```
    
3. 후위 순회 (postorder)
    
    Left → Right → Root
    
    ```java
    void postorder(Node node) {
      if (node != null) {
        postorder(node.left);
        postorder(node.right);
        System.out.println(node.data);
      }
    }
    ```
