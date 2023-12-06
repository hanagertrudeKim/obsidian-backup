
## what is the Graph
graph = nodes + edges

graph has a two type, directed graph and undirected graph.
- directed graph : 방향이 정해져있다.
- undirected graph : 방향이 정해져있지 않다. 

![](https://i.imgur.com/YflD43k.png)

- neighbor node
in the case directed graph, we call b or c that a can go way(?) is neighbor node.

## traversal
1. depth first traversal
깊이를 먼저 확인하는 traversal
use Stack, somthing where you add to the top, and remove from the top.
stack 의 구조 특이점을 생각하고 코드를 보면 이해가 더 될것이다.

```c++
void depthFirstPrint(graph, source){
	std::stack<Node> stack;
	stack.push(graph.at(source));

	while (stack.length > 0){
		Node current = stack.top();
		stack.pop();
		std::cout << current.value << endl;

		for(char neigbor : current.neighbors) {
			stack.push(graph.at(neighbor));
		}
	}
};

Node graph = {
	a: ['c', 'b'],
	b: ['d'],
	c: ['e'],
	d: ['f'],
	e: [],
	f: [],
}

depthFirstPrint(graph, 'a');
```

이 코드를 recursion 을 이용하여 더 짧은 코드로 작성할수있다.
```c++
void depthFirstPrint(const std::vector<std::vector<int>>& graph, int source){
	std::cout << source << endl;
	
	for (int neighbor : gragh[source]){
		depthFirstPrint(graph, neighbor);
	}
}
```

이 코드를 출력해보면 **a, b, d, f, c, e**  순으로 출력이 될것이다.

2. breadth first traversal
가까운 노드를 확인하는 traversal
use Queue, somhting where you add to the back and remove from the front. 

lets see the code
```c++
void breathFirstPrint(graph, source){
	std::queue<char> queue;
	queue.push(source);
	
	while (!queue.empty()){
		char current = queue.front();
		queue.pop()
		std::cout << current << endl;
		
		for(char neighbor : graph.at(curretn)){
			queue.push(neighbor);
		}
	}
}
```

![](https://i.imgur.com/2t6bZn3.png)


## 인접 리스트와 인접 행렬의 차이
인접행렬의 장점은 구현이 쉽다는 접이다.
특정 노드 사이와 노드를 확인할때도 o(1) 시간복잡도를 갖는다는 장점이 있다.
그렇지만 단점도잇다.
노드i 에 연결된 노드를 확인하려면 o(v)의 시간이 거린다.

인접 리스트는 간선의 개수에 비례하는 메모리만 차지하기때문에 절약된다.
이를 전체 노드에 대한 탐색을 수행하는 상황으로 확장해서 생각해보면, 인접 행렬의 경우 o(v * v)의 시간복잡도가 걸리고, 인접 리스트의 경우 o(E)의 시간복잡도를 가진다는 장점이 있다.
단점은 노드 i와 노드 j 가 연결되어있는지 확인하고싶다면 o(v) 가 걸릴것이다. (자리가 지정되어있지않고 순회해야하므로)



