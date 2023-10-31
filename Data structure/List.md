## Linked List (연결 리스트)
- 연결리스트는 여러 구조체의 인스턴스를 체인처럼 줄줄이 포인터로 연결한 자료구조
	- 컨테이너라고도함
- 연결에 사용된 포인터 숫자가 한개
	- 자기 다음을 가르키는 것이 특징

- 연결구조란 고정된 공간을 미리 확보하여 사용하지 않고, 필요할 때마다 동적으로 확보하여 사용하는 구조를 말함
- 공간부족이 낭비가 발생하지않음
- 연속적이지 않음
	- 연결구조에서 데이터와 다음 연결정보(link)의 쌍을 노드라 함

### 코딩 순서
1. 전체 리스트를 출력하는 함수 작성
2. 새 노드 추가 함수 작성
	- 왠만하면, 절차를 글로 먼저 작성한다
3. 전체 리스트 삭제 함수 작성
4. 각 함수 작성할때마다 main() 에서 테스트 코드 실행
### 코딩 요령
- 구조체를 배열로 테스트한다
	- 리스트는 흔히 말하는 DB에 해당함. DB는 데이터로 이루어져 있음
		- 데이터는 구조체로 다룸
- 연결 순서를 임의로 변경해본다 
	- 디버거 과정
- 리스트 출력은 별도 함수로 분리한다
- 디버거로 노드 하나씩 따라가며 메모리 위치를 확인한다

### 연결 리스트 vs 배열 

- C1. 유지해야 할 요소의 개수를 사전에 알 수 있는가? 알 수 있으면 배열이 효과적임. 데이터가 초기에 모두 제공되는 것이 아니면 효과적이지 않을 수 있음 
- C2. 어떤 방식의 데이터 삽입과 추출이 필요한가? 앞 뒤에서 주로 접근하면 연결구조도 좋은 대안이 될 수 있음 한쪽으로만 이루어지면 C1에 따라 결정해야 함 색인 기반 연산을 주로 사용하면 배열 기반이 더 효과적임 
- C3. 저장된 데이터를 이용하여 어떤 작업을 주로 수행해야 하는가? 위치에 의한 접근이 많이 필요하면 무조건 임의 접근이 가능한 배열이 더 좋은 방법임 (색인 기반 접근) 중간에 있는 데이터의 삭제가 자주 일어나면 이중 연결구조+맵, 집합 자료구조 등을 사용할 필요가 있음


## Single linked list
- 선형적 구조
- node 와 next 의 정보 존재
	- node : data
	- next : pointer

### 생성과정

#### 기초 골격 이해하기
- struct 구조체를 선언해보자
	- typedef를 써서 형 재선언을 할수있음
```c
typedef struct NODE {
	//관리될 데이터
	int nData;
	//데이터 관리를 위한 포인터
	struct NODE* next;
} NODE;

int main(){
	//구조체 선언 (배열 형태로)
	NODE list[5] = { 0 };

	//단순 무식하게 접근해보면
	list[0].next = &list[1];
	list[1].next = &list[2];
	list[2].next = &list[3];
	list[3].next = &list[4];
	list[4].next = nullptr;
	return 0;
}
```

- 만약에 리스트에 각각 12345를 할당하고, 출력을 하려면 아래처럼 코드를 쓰면 된다.
```c
for(int i = 0; i < 5; ++i){
	std::cout << list[i] << "\n";
}
```

- 이 상태에서 데이터를 포인터를 사용하여 출력해보자.
	- 이전에 데이터의 주소값을 각각 next라는 포인터에 할당해주었으니, 포인터에 접근하여 nData를 출력하고
	- pTmp 를 사용하여 주소값을 갱신해준다.
```c
for(int i = 0; i < 5; ++i){
	NODE* pTmp = &list[0];

	while(pTmp != NULL){
		std::cout << pTmp -> nData << "\n"
		//pTmp의 주소값 overWrite
		pTmp = pTmp -> next
	}
}
```

#### 단일 연결 구조 구현 #1 기본
-  단일 연결 구조를 구현하는 방법은 첫번째 데이터를 어떻게 할거냐 로 2가지 방법으로 나뉘어진다.
	- 1. **pHead를 생성**하여 첫번째 데이터를 가르키는 방법
	- 2. **Dummy** 노드를 만들어서 (데이터 저장용이 아닌 관리용) 데이터를 관리하는 방법

---
이제 1번의 방법으로 단일 연결 구조를 구현해보자
- 우선 구조체를 선언해주고, g_pHead 포인터를 선언해준다.
```c
typedef struct NODE {
	char szdata[64];
	struct NODE* next;
} NODE;

// NODE* pTmp = &list[0]와 같은 역할
NODE* g_pHead = NULL;
```

- 그리고 리스트를 출력하는 함수를 만들어준다.
```c
void PrintList(void){
	while(g_pHead != NULL){
		std::cout << g_pHead << g_pHead->szData << g_pHead->next
		g_pHead = g_pHead -> next
	}
}
```

- 이제 새 노드를 생성하고, 적절한 자리에 할당해주는 함수를 만들어주자.
	- 여기서 주의할것이, g_pHead가 NULL 이 아닐때인데 이 말인 즉슨, 기존의 가르키던 노드가 있다는 말이다
	- 원하는목적은 기존의 가르키던 노드 와 pHead 사이에 새로운 노드인 pNode 를 추가해야한다.
	- 바로 pHead 에 새로운 pNode 를 overwrite하면 기존 data를 유실할 가능성이 있다.
	-  => 그래서 잃지않도록 새로운 pNode 의 next 에다가 기존 데이터의 주소를 옮겨주고
	-  => 그리고나서 g_pHead에 pNode를 할당해준다.
- 근데 여기서 위의 PrintList 와 같은 g_pHead를 공유하고있으므로 값이 이상해지니까, 여기서는 다른 head pointer 를 쓸수있도록 조정해준다.
```c
int InsertNewNode(char* pszData){
	//새로운 pHead 할당
	NODE* pHead = g_pHead;

	//pNode 생성
	NODE* pNode = (NODE*)malloc(sizeof(NODE));
	//매개변수로 넘어온 szData를 복사
	strCopy(pNode -> szData, pszData);

	// g_pHead가 NULL일때 pNode 할당
	if(g_pHead == NULL)
		g_pHead = pNode;

	else{
		pNode.next = g_pHead;
		g_pHead = pNode;
	}
}
```

#### 단일 연결 구조 구현 #2
##### 소멸자
- c++ 에서 단일 연결 리스트를 구현하는데 왜 소멸자가 필요할까?
	- 노드 삭제할떄는 노드를 삭제시키는게 아니라, 단순히 head pointer 의 값의 주소값을 바꾸면 된다.
	- => 이럴때, 삭제된 노드는 여전히 메모리에 할당되어있게된다. 
	- 자바나 파이썬은 이를 자동적으로 처리해주지만, c++은 안되기때문에 메모리에 할당된 삭제 노드를 소멸자를 통해서 해제시키는것이 핵심이다

- 그럼 이제 구현해보자. 우선 기초 골격으로 node 를 선언해준다.
```c
struct Node{
	int item{0};
	Node* next{nullptr};
	Node() = default;
	Node(int item, Node* next = nullptr): item{item}, next{next}{}
};
private:
	Node* head{nullptr};
	Node* tail{nullptr};
	size_t numItems{0};
```

- 그리고 소멸자 함수를 정의해준다 (=초기화 함수)
- 대략적인 의사 코드
	- curr 노드 생성
	- curr 노드를 deleteNode 에 할당 
	- curr pointer 가 null 이 될때까지 루프 반복
		- curr 노드를 deleteNode에 할당(temp 역할)
		- deleteNode 메모리 할당 삭제
		- curr갱신
	- head, tail, numItems 초기화
```c++
void clear() noexcept{
	Node* curr{head};
	while(curr){
		Node* deleteNode{curr};
		curr = curr->next;
		delete deleteNode;
	}
	head = tail = nullptr;
	numItems = 0;
}
```

##### 상태 조회 메서드
- 상태를 조회할수있는 메서드는 두가지로 정의된다.
	- ** 연결구조기반은 메모리가 부족할일이 없으므로, isFull() 을 따로 정의하지않아도 된다.
- isEmpty()
	- 노드가 없는 빈 상태인지 조회
```c
bool isEmpty() const noexcept{
	// return !head;
	// return numItems == 0;
	return head == nullptr;
}
```
- size()
	- 노드의 수를 조회
```c
size_t size() const noexcept{
	return numItems;
}
```


##### 색인(index) 기반 메서드
- 연결구조 리스트는 순차 접근만 가능하므로 딱히 get, set 메서드가 효율적이진 못하다.
	- 따라서 순차적으로 index만큼 하나씩 이동하여 색인에 해당하는 노드에 접근한다.

반환 메서드
- getItem()
- 대략적인 의사 코드
	- index전까지 curr.next를 옆으로 옮겨가면서 할당한다
	- 마지막 index번째 item return한다
```c
int& getItem(size_t index) const{
	Node* curr{head};
	for(size_t i{0}; i < index; ++i) curr = curr->next;
	return curr-> item;
}
```

삽입 메서드
- ?? 예시코드에는 추가되지않았다.. 왜지

##### 찾기 연산 메서드
- find()
- 대략적인 의사코드
	- curr의 item과 찾으려는 item 이 비슷하면 true 반환
	- curr 를 하나씩 옆으로 옮겨감
```c
bool find(int item) const noexcept{
	Node* curr{head}
	while(curr){
		if(curr->item == item) return true;
		curr = curr -> next;
	}
	return false;
}
```


##### 맨 앞과 맨 뒤 삽입/추출 연산
- 연결구조 기반 리스트 자료구조는 맨 앞 삽입과 추출은 효과적이다
	- 꼬리 포인터를 유지하면 맨 뒤 삽입도 효과적으로 할 수 있다
- pushFront()
	- 맨 앞 삽입 메서드
	- 대략적인 의사코드
		- 새로운 newNode 생성 (새로운 item정보 저장)
		- newNode 의 next 주소에 기존의 head 정보 옮기기
		- head 에 새로운 newNode 할당하기!
	- 여기서도 주의해야할것이 기존의 head 의 주소가 유실되지 않게 한다
```c
void pushFront(int item){
	Node* newNode{new Node(item)};
	//기존의 head정보 newNode.next 에 옮기기 => head에 새로운 node 할당
	newNode->next = head;
	head = newNode;
	++numItems;
}
```

- popFront
	- 맨 앞 노드 뽑아내기(?)
	- 대략적인 의사코드
		- isEmpty 검사 => empty 상태라면 에러 출력!
			- 이전에 정의한 isEmpty() 이용
		- head(맨앞) 의 item 저장
		- deleteNode 에 head 복사 (이건 curr 할필요 없음 진짜 삭제라서)
		- deleteNode 메모리에서 삭제
		- head 갱신
		- numItems 에서 - 1
		- item 반환
```c
int popFront(){
	if(isEmpty()) throw std::runtime_error("popFront: empty state");
	int result {head->item};
	Node* deleteNode {head};
	delete [] deleteNode;
	head = head->next;
	--numItems;
	return result;
}
```


- pushBack()
	- 대략적인 의사 코드
		- newNode 생성
		- list 가 empty 상태면 tail = head
		- getTail 로 tailnode.next 데려와서 newNode 할당
			- 더미 노드를 사용하면 항상 마지막에 head을 dummy.next로 갱신해 주어야 한다.
		- numItems 증가
```c
void pushBack(int item){
	Node* newNode{new Node(item)};
	if(isEmpty()) head = tail = newNode;
	else {
		tail->next = tail;
		tail = newNode;
	}
	++numItems;
}
```

- popBack() 
	- 대략적인 의사 코드
		- isEmpty 검사로 일단 비었는지 확인하기
		- dummy node, prev, curr 정의
		- prev포인터를 통해 마지막 노드를 찾기
			- 노드의 next의 next가 null 인것을 확인하기 위함.. 
		- numItems 감소
```c
int popBack(){
	if(isEmpty()) throw std::runtime_error("error");
	Node dummy{-1, head};
	Node* prev{&dummy};
	
	//head의 주소값을 저장하기 위한 curr추가
	Node* curr{head};
	while(prev->next->next){
		prev = curr;
		curr = curr->next;
	}
	int result{curr->item};
	prev->next = nullptr;
	head = dummy.next;
	tail = head? prev : nullptr;
	delete curr;
	--numItems;
	return result;
}
```

##### 삭제 연산 메서드
- removeFirst()
	- item 을 받아 해당되는 첫번째 노드를 삭제함
	- 삭제 연산자는 삭제할 노드를 찾은 후에 이전 노드의 연결정보를 삭제할 노드의 후속 노드로 바꿔주어야한다.
		- 그래야  후속 노드의 정보가 유실되지않음
	- 대략적인 의사코드
		- dummy 노드와 prev 노드를 정의해준다.
			- dummy 노드를 이용하는것이 간편하다. (후속노드의 정보값이 필요하기 때문에)
		- prev의 next가 null일때까지
			- dummy 노드가 item과 일치한다면 삭제
			- prev 갱신
		- head 를 dummy 의 next로 갱신
- removeNode() 를 정의해준다.
```c
void removeNode(Node* prev, Node* curr){
	//curr가 마지막 노드라면
	if(curr == tail) tail = (curr == head? nullptr: prev;)
	
	prev->next = curr->next;
	delete curr;
	--numItems;
}
```

```c
void removeFirst(int item) noexcept{
	Node dummy{-1, head};
	Node* prev{&dummy};
	Node* curr{head};
	while(curr){
		if(curr->item == item){
			removeNode(prev, curr)
			break;
		}
	//prev 갱신
	prev = curr;
	curr = curr->next;
	}
	head = dummy.next;
}
```

- removeAll()
	- removeFirst()에서 break 문만 없애주면된다.
```c
void removeAll(int item) noexcept{
	Node dummy{-1, head};
	Node* prev{&dummy};
	Node* curr{head};
	while(curr){
		if(curr->item == item){
			Node* next{curr->next};
			removeNode(prev, curr);
			curr = next;
		} else {
			prev = curr;
			curr = curr->next;
		}
	}
	head = dummy.next;
}
```

