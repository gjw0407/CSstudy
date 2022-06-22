# Binary Search Tree

## 정의

> 각 노드의 자식 노드가 최대 2개인 트리
> 

## 조건

1. 각 노드에 중복되지 않은 키가 있음
2. 루트 노드 > 왼쪽 노드
3. 루트 노드 < 오른쪽 노드
4. 서브 트리도 모두 이진 탐색 (recursion)

## 시간복잡도

O(h) → h: height

## Search, Insert, Delete

### Node

```java
class Node {
		int data; 
		Node left; 
		Node right;

		Node(int data){ 
			this.data = data;
		}
	}
```

### Search

타겟 값보다 작으면 왼쪽으로, 크면 오른쪽으로 이동

```java
// 순환적인 탐색 
public static Node circularSearch(Node node, int key) {
	if(node == null) {
		return null;
	} 

	if(key == node.data) {
		return node;
	} else if(key < node.data) {
		return circularSearch(node.left, key);
	} else {
		return circularSearch(node.right, key);
	}
}

// 반복적인 탐색 
public static Node repetitiveSearch(Node node, int key) {
	while(node != null) {
		if(key == node.data) {
			return node;
		} else if(key < node.data) {
			node = node.left;
		} else {
			node = node.right;
		}
	}

	return null; // 탐색 실패했을 경우 
}
```

### Insert

- 중복 허용 X
- 삽입할 값이 작으면 왼쪽, 크면 오른쪽으로 이동 후 비어 있으면 삽입, 비어 있지 않으면 이동

```java
public Node insertNode(Node node, int key) {
	if(node == null) { 
		return new Node(key); // 노드가 빈 경우, 새로운 노드 삽입후 반환 
	}

	// 그렇지 않으면 순환적으로 트리를 내려감 
	if(key < node.data) {
		node.left = insertNode(node.left, key);
	} else if(key > node.data) {
		node.right = insertNode(node.right, key);
	}

	// 삽입 완료 후, 루트 노드 반환하며 끝 
	return node;
}
```

### Delete

1. 서브 트리가 없는 경우
    1. 탐색 후 삭제 
2. 서브 트리가 있는 경우
    1. 서브 트리가 하나인 경우
        1. 하나 남은 자식 노드가 삭제하는 노드의 위치를 대신함 
    2. 서브 트리가 둘인 경우
        1. 삭제할 노드 왼쪽 서브 트리의 가장 큰 자손을 해당 노드에 올림
        2. 삭제할 노드 오른쪽 서브 트리의 가장 작은 자손을 해당 노드의 자리에 올림

```java
// 노드 삭제 
public Node deleteNode(Node root, int key) {
	if(root == null) {
		return root;
	}

	if(key < root.data) { // 키가 루트보다 작다면, 왼쪽 서브 트리에 있는 것 
		root.left = deleteNode(root.left, key);
	} else if(key > root.data) { // 키가 루트보다 크다면, 오른쪽 서브 트리에 있는 것 
		root.right = deleteNode(root.right, key);
	} else { // 키가 루트와 같다면 이 노드가 바로 삭제할 노드
		if(root.left == null) { // 1번, 2번의 경우 - 1. 단말 노드인 경우 / 2. 하나의 서브트리만 있는 경우 
			return root.right; // 널 값이면 널 반환 / 오른쪽 있으면 오른쪽 반환해서 이전의 if, else if에서의 왼쪽이든 오른쪽 노드에 붙여주는 것
		} else if(root.right == null) {
			return root.left; // left가 널인 경우와 동일 
		}

		// 3번의 경우 - 3. 두개의 서브 트리가 있는 경우 (left, right 둘 다 null 아님  
		Node temp = minValueNode(root.right); // 오른쪽 서브 트리에서 가장 작은 값(가장 왼쪽 노드)가 후계 노드 

		root.data = temp.data; // 후계 노드 값 복사(삭제할 노드의 값을 후계 노드 값으로 변경  
		root.right = deleteNode(root.right, temp.data); // 후계 노드 삭제 - 오른쪽 노드에게 가장 작은 값을 가졌던 맨 왼쪽 단말노드를 다시 deleteNode를 호출해 삭제하라고 함  
	}

	return root;
}

// 후계 노드 찾기 - 오른쪽 서브트리에서 가장 작은 값을 가지는 노드 반환 
public Node minValueNode(Node node) { 
	Node currentNode = node;

	while(currentNode.left != null) {
		currentNode = currentNode.left; // 맨 왼쪽 단말 노드를 찾으러 내려감 
	}
	return currentNode;
}
```

# 출처

[https://code-lab1.tistory.com/10](https://code-lab1.tistory.com/10)

[https://minhamina.tistory.com/97](https://minhamina.tistory.com/97)