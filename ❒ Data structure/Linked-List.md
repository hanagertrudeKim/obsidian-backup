## about List
우리가 자료구조를 배우는 이유중의 하나는 자료들을 저장하여 1.삽입 2. 삭제 3. 탑색 이 세가지의 연산을 빠르게 하려고 하기 때문이다. 각 상황에 맞는 작업을 하기 위해 다양한 자료구조가 있는데 List도 그중에 하나이다. List도 그 중 하나의 자료구조이다.
List는 선형적으로 (= 순서가 존재한다)값들을 가지고 있는 자료구조이다.
두가지 종류로 구분할수있는데 가장 흔히 알고있는 배열과 linked-list 이다.
이 둘은 장단점이 명확하고, 시간복잡도에 큰 영향을 미친다.

## 왜  linked-list 인가?
연결 리스트는 단순한 편이기 때문에 약 한시간에 두 세문제를 내야하는 인터뷰 특성에도 잘 맞아 떨어진다.
연결 리스트를 완벽하게 구현하기에는 보통 10분이 걸리지 않기 때문에 문제를 푸는 시간은 충분하다.

## 연결리스트의 종류
연결 리스트에는 **단일 연결 리스트, 이중 연결 리스트, 원형 연결 리스트** 이렇게 세가지 기본 유형이 있다.
면접에는 대부분 단일 연결 리스트 문제가 나오는 편.

#### 단일 연결 리스트
리스트는 각각 노드들로 구조를 이루고있다. 
노드는 두가지의 정보를 가지고있는데,
1. 자기 자신의 값
2. 다음 노드의 Pointer(주솟값) 이다.
다음노드의 주솟값이 없다면 Nullpointer가 된다.

![](https://i.imgur.com/OakQejq.png)

단일 연결 리스트의 첫번째 원소는 리스트의 head, 마지막 원소는 tail 이 된다.
단일 연결 리스트에 있는 연결고리는 다음 노드를 가리키는 포인터나 레퍼런스로 구성되기때문에 앞으로만 종주할수있다. 따라서 리스트를 완전하게 종주하려면 항상 첫번째 노드부터 시작해야한다. 
이러한 head의 포인터나 레퍼런스는 보통 별도의 자료구조에 저장한다.

c++에서의 리스트 원소 클래스는 다음과 같이 정의할수있다.

```c++
template<typename T>

class ListNode{

public:

    T value;

    ListNode<T> *next; // 마지막 노드면 nullptr

    // 생성자
    ListNode<T>(): next(nullptr){}

    ListNode<T>(T value1, ListNode<T> *next1): value(value1), next(next1){}

};
```

(하지만 보통은 리스트 원소용 템플릿으로 정의하는 편이 낫다.)

## 삽입, 삭제, 순회와 시간복잡도
리스트에서 그럼 노드의 삽입은 어떻게 이루어질까? 대략적인 순서를 적어보면, 

- 두번째 노드와 세번째 노드 사이에 3을 집어넣어야한다.
1. 두번째 노드와 세번째 노드의 포인터를 끊는다.
2. 두번째 노드가 3원소를 가리키게한다.
3. 3이 세번재 노드를 가리키게한다.

이런식으로 될것이다.
그럼 삭제 연산은 어떻게 될까?

- 세번째 노드의 3을 다시 삭제한다.
1. 두번째 노드가 네번째 노드를 가리키게한다. 
   (두번째 노드의 Next = 세번째 노드의 Next)
2. 세번째 노드를 삭제해버린다.

이렇게 하면 노드를 삭제할수있다.
연결리스트의 순회는 단순히 매 노드의 반복문을 통해 순회하면서 구현할 수 있을것이다.

위와 같은 과정에 대해서 시간복잡도를 생각해보면, 
삽입, 삭제의 경우 시간 복잡도는 N개의 원소가 있다고 가정할때 O(N)이 된다. 
왜냐하면 특정한 원소를 가리키는 포인터가 없고 next포인터를 집어가면서 원소를 삽입, 삭제 해야하기 때문이다. 그러나 추가할 노드의 자리가 K번째의 자리에 있다면 O(K)가 될것이기 때문에, **맨 처음 노드의 연산 시간복잡도는 O(1)** 이 될것이다. (만약 tail 포인터가 있다면 맨 뒤도 마찬가지..)

만약 일반적인 배열이라면, K번째 자리에 원소를 추가하면 N-K개의 값들을 다 뒤로 밀어줘야하기 때문에 O(N-K), 삭제할때도 당겨줘야하므로 마찬가지다.
**따라서 맨뒤나 앞에서 동시에 삭제/연산이 O(1)이 되는 경우는 없다.**

하지만 배열에서도 랜덤 인덱스(random index) 가 가능하다는 점에서 장점이 존재한다.
랜덤 인덱스란 특정 인데스 값을 O(1)만에 참조하는것으로, 배열이라면 메모리상에서 값들이 일렬로 늘어져있으므로 가능하다. 근데 연결리스트는 K번 next포인터를 쫒아서 가야하므로 O(K) 가 걸린다.

이렇게 배열과 리스트의 차이점이 있다. 리스트는 비록 랜덤 인덱스는 안되지만, 만약 스택 덱 큐 등의 자료구조를 구현할때 삽입과 삭제 연산이 매우 유용하다. 
따라서 배열은 랜덤 인덱스가 자주 쓰이는곳에서 리스트는 삽입과 삭제 연산이 자주 이루어지는곳에서 유리하게 작용할수있다.

아래는 학부 자료구조 수업에서 배운 리스트와 배열을 선택할때 기준점이다. (참고로 보면 좋을것같다)

**Array vs List**
- C1. 유지해야 할 요소의 개수를 사전에 알 수 있는가? 알 수 있으면 배열이 효과적임. 데이터가 초기에 모두 제공되는 것이 아니면 효과적이지 않을 수 있음 
- C2. 어떤 방식의 데이터 삽입과 추출이 필요한가? 앞 뒤에서 주로 접근하면 연결구조도 좋은 대안이 될 수 있음 한쪽으로만 이루어지면 C1에 따라 결정해야 함 색인 기반 연산을 주로 사용하면 배열 기반이 더 효과적임 
- C3. 저장된 데이터를 이용하여 어떤 작업을 주로 수행해야 하는가? 위치에 의한 접근이 많이 필요하면 무조건 임의 접근이 가능한 배열이 더 좋은 방법임 (색인 기반 접근) 중간에 있는 데이터의 삭제가 자주 일어나면 이중 연결구조+맵, 집합 자료구조 등을 사용할 필요가 있음

#### 이중 연결 리스트
이중 연결 리스트는 각 원소마다 리스트에서 그 다음에 오는 원소에 대한 연결고리 + 그 앞에 있는 원소에 대한 연결고리도 들어있다. 이렇게 연결고리를 추가하면 어느 방향으로는 종주할수있다. 어떤 원소에서 시작하든 리스트 전체를 종주하는것이 가능하다. 
이중 연결 리스트는 면접 문제로 그리 많이 나오지는 않는다. (단일 연결 리스트를 쓰는 쪽이 더 어렵기 때문에..)
![](https://i.imgur.com/hWr4pUT.png)


#### 원형 연결 리스트
원형 연결 리스트에는 끝 (head, tail)이 없다. 모든 뭔소에서 다음 원소를 가르켜야하며 널포인트가 들어가지않는다. 원소가 하나밖에 없는 리스트라도 그냥 자기 자신을 가리키게된다.
원형 연결 리스트의 종주 문제는 사이클 회피 문제가 많이 나온다. 시작점을 제대로 추적하지 않으면 리스트에서 무한 루프를 돌 수 있다. 
가끔 쓸일이 있지만, 면접 문제로는 거의 나오지 않는 편이다.

## 코딩 순서
1. 전체 리스트를 출력하는 함수 작성
2. 새 노드 추가 함수 작성
	- 왠만하면, 절차를 글로 먼저 작성한다
3. 전체 리스트 삭제 함수 작성
4. 각 함수 작성할때마다 main() 에서 테스트 코드 실행
## 코딩 요령
- 구조체를 배열로 테스트한다
	- 리스트는 흔히 말하는 DB에 해당함. DB는 데이터로 이루어져 있음
		- 데이터는 구조체로 다룸
- 연결 순서를 임의로 변경해본다 
	- 디버거 과정
- 리스트 출력은 별도 함수로 분리한다
- 디버거로 노드 하나씩 따라가며 메모리 위치를 확인한다

## Single linked list

기본적인 개요는 이러하다.
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

## Traversing List
리스트를 종주해야할때, 보통 정의해준 head 포인터를 통해서 하나하나 순회할것이다.
여기서 주의해야할점은, 마지막 리스트인지 아닌지 항상 확인해야한다는 점이다.

```c
int find(ListElement head, int data){
	ListElement elem = head;
	while (elem.value() != data){
		elem = elem.next();
	} 
	return elem;
}
```
위와 같은 코드에서 무엇이 틀렸을까? 그건 바로 마지막 요소를 검색할때 오류를 뱉어낸다는 점이다.
마지막 node의 next는 NullPointer이므로, while 문 안에서는 에러가 난다. 따라서 조건을 추가해줘야한다.

```c
int find(ListElement head, int data){
	ListElement elem = head;
	while (elem != null && elem.value() != data){
		elem = elem.next();
	} 
	return elem;
}
```
```ad-note
Always test for the end of a linked list as you traverse it.
```

## Inserting and Deleting Elements
노드 중간에 삽입과 삭제 연산을 하려면 항상 이전 노드의 next pointer를 갱신해야한다.
만약에 너가 delete할 element만 있다면, head를 통한 종주를 통해서 그 element를 찾아야한다. 특별히 주의해야할 케이스는 그 삭제 element가 맨 앞에 잇는 Head 노드일때다.

```c
bool deleteElement(IntElement **head, IntElement *deleteMe){
	IntElement *elem;
	//check null pointer
	if(!head || !*head || !deleteMe) return false;

	elem = *head;
	
	//special case for head
	if(deleteMe == *head){
		*head = elem->next;
		free(deleteMe);
		return true;
	}

	while(elem){
		if (elem->next == deleteMe){
			//그 다음 주소로 갱신
			elem->next = deleteMe->next;
			free(deleteMe);
			return true;
		}
		elem = elem->next;
	}
	//deleteMe not found
	return false;
}
```
만약 List 전체를 삭제하고싶은 경우는 어떻게 할수있을까?
c 나 c++에서는 가비지컬렉션(동적으로 할당된 메모리중에 사용안하는것을 찾아서 자동적으로 삭제)이 없기 때문에 메모리 해제를 해줘야한다.
이 상황에서 노드를 하나하나 종주하면서 삭제를 한다고 가정해보자. 
요소 메모리를 먼저 해제(삭제)시킬것인가, next포인터를 먼저 삭제시킬건가?
둘다 불가하다. 
(메모리를 먼저 삭제시킴 => next로 갈수없음 , next포인터 먼저 삭제시킴 => 메모리 해제 불가함)
따라서 해결책으로 두가지의 포인터가 필요하다.
```c
void deleteList(IntElement **head){
	IntElemnt *deleteMe = *head;

	while(deleteMe){
		//next 정보를 먼저 저장하고
		IntElment *next = deleteMe->next;
		//메모리 해제
		free(deleteMe);
		//저장한 next정보 전달
		deleteMe = next;
	}
	*head = NULL;
}
```
```ad-note
리스트 삭제에는 항상 최소 두가지 pointer가 필요하다.
```


## Problems
### 1. Stack Implementation
```ad-question
1. 스택을 정의, 논의해라.
2. 스택을 linked list or dynamic array 로 실행하고 설명해라.
3. interface를 사용하기 쉽게 구성하고 완성해라.
```

1. 스택은 LIFO(last in first out) 자료구조이다.
	1. 가장 마지막에 들어간 원소가 가장 먼저 나온다는 소리임.
	2. 그래서 top 원소를 중심으로 삭제 삽입의 동작이 이루어진다.
		1. top 을 삭제하면 pop / top을 추가하면 push
		2. 만약 빈 원소값을 pop, push 하면 언더 플로우가 발생한다.
		   
2. dynamic array vs linked list 비교
	1. dynamic array 로 스택을 구성하는 법은 엘리먼트가 추가될때마다 배열의 사이즈를 바꿔야한다. 
	    => **시간복잡도 O(N)**
	2. dynamic array로 스택을 만들때의 메인 장점은
		1. random access가 가능하다.
			1. 원하는 index의 원소에 바로 접근할수있다.
	3. but, 끝의 원소만 다루는 stack 자료구조에서는 random access의 이점을 얻기가 어렵다.
	4. 그리고 스택이 커질때마다, resize를 해줘야하는데, 기존배열에서 새배열을 복사해서 진행하는 작업상 시간복잡도가 커질수있다.
	   
	5. linked list 로 스택을 구성하는 법은  head 포인터가 top 엘리먼트를 가리키게 하면된다.
		1. 특정 구간에 대해서만 pop push 가 이루어지기 때문에 O(1) 시간복잡도를 가진다.

3. 우선 코드를 작성하기 전에 디자인을 해보는 습관이 중요하다. 항상 무엇을 할건지 인터뷰어에게 말을 하는것이 좋다.

```c++
class Stack {  
	public:
		Stack() : head(NULL) {};
		~Stack();
		void push(void *data);
		void pop();
	
	protected:  
		class Element {
			public:
				Element(Element *t, void *d) : next(t), data(d) {}
				Element* getNext() const {return next;}
				void *getvalue() cosnt {return data;}
			private:
				Element *next;
				void *data;
		};
		
		Element *head;
	};

	//소멸자 정의
	Stack::~Stack() {
		while(head){
			Element *next = head->getNext();
			delete head;
			head = next;
		}
	}

	//요소 삽입
	void Stack::push( void *data ){
		Element *element = new Element(head, data);
		head = element;
	}

	void Stack::pop() {
		Element *popElement = head;
		void *data;

		if(head == NUll) throw StackError(E_EMPTY);

		data = head->getValue();
		head = head->getNext();
		delete popElement;
		return data;
	}
}

```

```ad-note
- dynamic array 와 linkedlist로 스택을 만드는 것에서의 장단점을 확실히 파악하는것이 중요할것같다. 상황에 따라 고려사항과 그에 따른 장단점이 확실하다.  
- 코드를 작성할때 먼저 디자인하는 습관이 중요하다는 것을 깨달았다. 특히나 면접을 진행할때, 또는 업무적인 코드를 작성할때에도 적용되는 개념인것같다. 실제 면접에서는 인터뷰어에게 내가 무엇을 하는지 계속해서 말하는 연습을 해야할것같다.
- 역시나 오류 처리는 다양한 방면으로 확인해야하는것같다. 코드에 장애가 생길 요소가 있는지 메모리 관점에서도 다시 생각해볼수있었다. 만약 종속되어있지않은 (class나 객체지향이 지원되지않는) 구조라면 이 값이 call by value 인지 call by Reference인지 확인해보는게 중요할것같다.

```

### 2. Maintain Linked List Tail Pointer
```c++
bool delete(Element *elem);
bool insertAfter(Element *elem, int data)
```
head, tail 포인터를 만들고 유지하는위 함수를 완성해봐라

head 는 요소의 맨 위 원소
tail 는 요소의 맨 끝 원소

- 포인터를 업데이트 해줘야하는경우
1. elem = head 일 경우 -> head 업데이트
2. elem = tail 일 경우 -> tail 업데이트
3. head, tail 도 아닌 중간 elemant 일때 -> 갱신해줄 필요 없습니다.

- 예외 처리
1. elem 가 없을 경우 return false
2. linked list가 null 일 경우 return false

```c++
bool delete(Element *elem) {
	if(!elem) return false;
	
	Element *current = head;

	// curr == head 인 경우
	if(head == elem){
		head = elem->next;
		delete elem;
		
		/* special case for 1 element list */
		if(!head) tail == Null;
		return ture;
	}

	// 중간 + tail 일 경우
	while(current){
		if(current->next == elem){
			current->next = elem->next;
			delete elem;

			//tail pointer 갱신
			if(current->next == Null) tail = current;
			return true;
		}
		current = current->next;
	}
	return false;
}
```

주어진 노드 뒤에 새로운 노드를 삽입하는 메소드
예외와 주의점은 위와 같다.

```c++
bool insertAfter(Element *elem, int data){
	Element *newNode;
	Element *current = head;

	newNode; = new Element(data);

	//메모리가 부족할 경우
	if(!newNode) return false;

	//elem가 Null로 주어진 경우에는 맨앞에 둬야함 => 리스트 처음에 삽입
	if(!elem){
		newNode->next = head;
		head = newNode;

		//이건 왜 여기서 해줌??
		if(!tail) tail = newNode;
		return true;
	}

	//리스트 중간이나 끝에 삽입
	while(current){
		if(current == elem){
			newNode->next = elem->next;
			elem->next = newNode;
			//current가 Tail 이었을때..	
			if(!(newNode->next)) tail = newNode;
			return true;
		}
		current = current->next;
	}

	//nothing found
	delete newNode;
	return false;
}
```

```ad-note
- 어떤 시점에서 각각의 포인터를 갱신해야하는지 먼저 시점을 파악하는게 포인트일듯
- 포인트 갱신 시점 + 예외처리 한번 정리해보고 => 코드 작성
```

### 3. Bugs in removeHead

```c
void removeHead(ListElement *head){
	delete head;
	head = head->next;
}
```
- head를 delete하기 전에 값을 임시로 저장할수있는 temp 저장소가 있어야할듯
- 그렇게 되면 head->next값을 head에 갱신시킬수있는 코드가 완성될것같다.

풀이 해설
1. 데이터가 함수에 제데로 들어오는지 확인 -> head를 통해 모든 노드를 접근할수있으므로 가능함
2. 함수를 한줄씩 분석해보면
	1. 헤드를 해제하면 head->next값을 불러올수없음  => 임시변수 사용
3. 함수가 값을 올바르게 반환하는지 확인
	1. 인수는 로컬 변수이기 때문에 변경사항이 전역적으로 적용되지않음 => 인수의 포인터를 전달하여 해결
4. 오류 조건 확인
	1. head가 null 이 될수있는가? => 오류처리해주기

## 5. List flattening
![](https://i.imgur.com/pupK1qJ.png)


```ad-question
- 리스트를 평탄화할수있는지가 핵심
- 중첩리스트를 풀어서 단순 1차원 리스트로 만들기
```

풀이 해설
- 리스트 평탄화(평면화) 문제는 여러가지 방법으로 풀수잇음
	- 어떤 알고리즘을 사용하느냐에 따라 평탄화 되는 순서가 달라짐 => 원하는 ouput에 맞출것
	- 그리고나서 효율적인 알고리즘을 구상
- 데이터 구조를 살펴보면 레벨이 존재함 + 단계가 있음
	- 트리와는 다른게 트리는 레벨이 있지만, 같은 레벨간의 링크가 존재하지는 않음
	- => 어쨋거나 트리 구조와 유사하니 트리 순회 알고리즘과 유사한 형태로 가능할듯 => o(n) 시간복잡도
- 직관적으로 보면, 노드를 순환하면서 중복된 구조의 list에 복사한다. (또다른 리스트 생성) 
	- 첫번째 계층의 노드 정렬 => 두번째 => 세번째
	- 특정 수준에서 검색하는 알고리즘을 쓰려면 넓이 우선 탐색 있음 but, 비효율적
	- 따라서 꼬리 포인터를 활용하는 방식을 함

결론 정리
- 주어진 리스트를 순회하면서 Child 리스트를 현재 리스트의 끝으로 보낸댜.
- Child리스트를 끝으로 보낼때마다 tail 포인터를 갱신
- 

```c++
/*
typedef struct Node {
	struct Node *next;
	struct Node *prev;
	struct Node *child;
	int value;
} Node;
*/

void flattenList(Node* head, Node** tail){
	Node *currentNode = head;
	while(currentNode){
		//curretnNode의 자식이 있는 경우
		if(currentNode->child){
			append(currentNode->child, tail);
		}
		currentNode = currentNode->next;
	}
}

void append(Node* child, Node** tail){
	Node *currentNode;
	
	//tail 뒤에 child리스트 추가
	*tail->next = child;
	child->prev = *tail;

	//child 끝을 찾아서 curretnNode 갱신해주기
	for(currentNode = child; currenNode->next; currentNode = currentNode->next){
		*tail = currentNode;
	}
}
```
사실상 위 append코드의 for문을 간소화하자면, for문의 body 를 비워둘수도있다. (어차피 currentNode는 갱신된다.)
```c++
for(currentNode = child; currenNode->next; currentNode = currentNode->next);
		
	*tail = currentNode;
```

```ad-note
- 이중 리스트나, 트리 모양의 리스트를 평탄화 하는 작업에서 고려해야할점은 순회 방법과 tail 포인터의 활용일것같다.
- 순회 방법에 따라 Output 구조가 달라진다. 따라서 어떤 순회를 하느냐도 중요한 요소중에 하나가 될것같음
- body를 비워둔 for문의 활용성을 깨달았다.
```