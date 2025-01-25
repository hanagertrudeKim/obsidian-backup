---
tags:
  - algorithm
  - data-structure
---

## What is Tree
- undirected gragh
	- acyclic( = not cycle) connected graph
	- connected gragh with N node +  N-1 edges
	- any two vertices(top nodes) are connected by exactly one path

## composition
- Root node
	- rooted tree
	- any node can root the tree
- child node
	- extending from another node
- parent node
	- inverse of this
	- if it has no parent, it maybe usefule to assign the parent fo the root node to be itself
	  (e.g. filesystem tree)
- leaf node
	- node with no children

## What is a Binary Tree? 
every node has at most two child nodes.

![](https://i.imgur.com/cg2ma16.png)


let's make Binary Tree in code.
```c++
class Node{
	int value;
	Node* left;
	Node* right;

	Node(int data) {
		value = data;
		left = null;
		right = null;
	}
}
```
and also make code to add value binary tree

```c++
int main(){
	Node* root = new Node(0);
	
	root->left = new Node(1);
	root->right = new Node(11);
	
	root->left->left = new Node(5);
	...
}
```

## what is Binary Search Tree (BST)?
left subtree has smaller elements and right subtree has larger elements.
![](https://i.imgur.com/CRFVtG3.png)
and also not limited to only using numbers. any data that can be ordered can be placed inside a BST.
![](https://i.imgur.com/BRXJTqU.png)
but, this case since 9 is larger than 8, it should be in the right subtree of 8.
![](https://i.imgur.com/lwgUGcd.png)


## where are BST used?
- Binary Search Trees (BSTs)
	- Implemetation of some map and set ADTs
	- Red Black Trees
	- AVL Trees
	- Splay Trees
	- etc..
- implementation of binary heaps
- systax trees
- Treaps - probablistic DS (random BST)

## Complexity
![](https://i.imgur.com/ogb2MxC.png)

## Adding elements to a BST 
BST elements must be comparable so that we can order them inside the tree.

- Recurse down left subtress (< case)
- Recurse down right subtrees (> case)
- Handle finding a duplicate value (= case)
- Create a new node (found a null leaf)

ex)
![](https://i.imgur.com/87ljFrx.png)


## Removing elements from a BST
Removing elements from a Binary Search Trees can be two step precess

1. find the elements we wish to remove
2. replace the node we want to remove with its successor to maintan the **BST invariant**
   (**BST invariant** : left substree has smaller elements and right subtree has larger elements)


There some Remove Phase Four cases

>CASE 1
- Node to remove is a leaf node
	- => to remove is a leaf node then we may do so without side effect

>CASE 2 & CASE 3
- Node to remove has a right subtree but no left subtree
-  Node to remove has a left subtree but no right subtree
	- => removing the root node, in which this case immediate child become the new root

>CASE 4
-  Node to remove has a both a left subtree and a right subtree
	- ther is two option
		- find the smallest value in right subtree
		- find the larger value in the left subtree

Here is the example of the fourth case, we have to remove 7, which root node.

![](https://i.imgur.com/O6EIIPt.png)


and find smallest value in the right subtree. (you can choose to find larger value in the left subtree)
then, now we do copy the value what we found. and remove the 7, and replace it 11.

![](https://i.imgur.com/nnB8hNx.png)

Luckily, the node we found (like 11) will always be either a case 1, 2, 3 removal. 
so you swap it right subtrees. (CASE 3)

![](https://i.imgur.com/1CSNvry.png)

## Tree Traversals
Lets introduce Preorder, Inorder,  PostOrder & Level order
tree traversals usally can resolve using [[recursion]] Algorithm.



1. **Preorder**
- preorder do something **before** the recursive calls 
- 뿌리(root)를 먼저 방문함.
- print the value of the current node then traverse left -> right
```c
preorder(node):
	if (node == null) return null;
	print(node.value)
	inorder(node.left)
	inorder(node.right)
```
![](https://i.imgur.com/q9qrVtF.png)
>  this case can be order => **A,  B,  D,  H,  I,  E,  C,  F,  J,  K,  G,  L**



2. **inorder**
- Inorder do something **between** the recursive calls
- 왼쪽 하위 트리를 먼저 방문 후 뿌리(root) 방문함.
```c
inorder(node):
	if (node == null) return null;
	inorder(node.left)
	print(node.value)
	inorder(node.right)
```
![](https://i.imgur.com/wXhPQmF.png)
> this case can be order => **1,  3,  5,  6,  8,  11,  12,  13,  14,  15,  17,  19**
	Notice the value printed increasing order!



3. **Postorder**
- Postorder do something **after** the recursive calls
- 하위 트리 모두 방문 후 뿌리 방문

```c
postorder(node):
	if (node == null) return null;
	inorder(node.left)
	inorder(node.right)
	print(node.value)
```
![](https://i.imgur.com/M7LXrM5.png)
>this case can be order => **1,  5,  3,  8,  6,  12,  14,  13,  19,  17,  15,  11**



4. Levelorder
- levelorder do someething the nodes as they apear one layer ar a time
- **위 쪽 node들 부터 아래방향으로 차례로 방문**
![](https://i.imgur.com/ljs4nIf.png)
>this case can be order => **11, 6, 15, 3, 8, 13, 17, 1, 5, 12, 14, 19`**


To obtain this ordering we want to do a **Breadth First Search (BFS)** from the root node down to the leaf node
To do BFS we will need maintain a **Queue** of the nodes left to explore.


---
## example problem 

```ad-example
1. 트리의 높이는 루트 노드에서 잎 노드까지의 거리의 최대값으로 정의된다.
	임이의 이진 트리의 높이를 계산하는 함수를 작성해라
```

나의 풀이는 이러하다.

1. tree를 끝까지 세면서 각각의 깊이를 탐색한다
2. max_count에 최대값을 갱신해준다.

```c++
int getHeight(Node* tree){
	int maxCount{0};
	int count{0};
	
	if(tree == null){
		maxCount > tree.value ? maxCount = tree.value : null;
		return 0;
	}
	count++;
	getHeight(tree->left);
	getHeight(tree->right);
}
```


책에서 제시한 풀이법은 훨씬 간단하다.

서브 트리 중 하나를 제거했을때, 그 트리의 높이가 달라질까? 에서 시작한 해결책이다.
그런 경우도 있겠지만, 둘 중에서 더 큰 서브 트리를 제거했을때만 그렇다. 이게 바로 핵심이다.

1. 서브 트리중에 더 큰 쪽의 높이에 1을 더한 값이 트리 높이가 된다. 
2. 정의 자체가 재귀적이므로 다음과 같은 재귀 코드로 구현할수있다.

```c++
//public Node getLeft(){return left;}
//public Node getRight(){return Right;}

int getHeight(Node* tree){
	if(tree == null) return 0;
	return 1 + std::max(getHeight(tree.getLeft(), tree.getRight()));
}
```
이 함수의 실행시간은 어떻게 되나?
재귀적으로 각 노드마다 한번씩 함수가 호출됨으로 전체 실행 시간은 O(n)이다.


## leet code

- leetcode 226
```ad-note
Given the `root` of a binary tree, invert the tree, and return _its root_.
```

```c++
/**

* Definition for a binary tree node.
* struct TreeNode {
	* int val;
	* TreeNode *left;
	* TreeNode *right;
	* TreeNode() : val(0), left(nullptr), right(nullptr) {}
	* TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
	* TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
* };
*/

class Solution {
public:
TreeNode* invertTree(TreeNode* root) {
	//이진트리 정렬 거꾸로
	if (!root) return root;
	
	//postorder이용
	invertTree(root->left);
	invertTree(root->right);
	TreeNode* tempRight = root->right;
	root->right = root->left;
	root->left = tempRight;
	
	return root;
	}
};
```

- Time complexity: o(n)

- Space complexity: 
#### solve problem
- 이진트리 정렬 거꾸로 하기
	- 이미 정렬되어있는 상태의 트리가 주어짐으로 (큰것은 오른쪽 작은것은 왼쪽) 양 쪽의 노드들을 모두 바꾸면 거꾸로 정렬하는것과 같은 상태가 된다. (큰것은 왼쪽 작은것은 오른쪽)
- postorder을 사용한 이유
	- invertTree 함수가 왼쪽 하위 루트부터 순회하며 오른쪽 노드와 왼쪽 노드를 바꿔치기한다.
	- 아래쪽부터 순차적으로 바꿔줘야 오류가 생기지 않을것같았음..
	- 해결답을 보니 preorder 순회를 해도 되는듯하다.






