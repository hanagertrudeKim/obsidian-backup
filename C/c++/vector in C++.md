---
tags:
  - Cpp
  - vector
  - array
---
# vector

---

## 1. what is vector
c++에서의 vector는 dynamic array로, 배열의 크기가 자유롭게 조정이 가능하다.
배열의 상위버전이라고 생각하면 쉽다. 


## 2. how to use vector
기본적으로 `#include <vector>` 라이브러리를 사용해야한다.

### declare
vector을 선언할때는 타입과 함께 이름을 지정해주면 된다.
```C++
vetor<type>vector_name;
```


<font color="#4f81bd">## push_back()</font>
`push_back()` 을 통해 배열에 element를 추가하게 되면, vector의 끝에 element가 추가된다.
예를 들어,
```c++
int main(){
	vactor<int>my_vector; //용량0, 크기0 (색인연산자 x)

	my_vactor.puch_back(1);
	my_vactor.puch_back(2);
	my_vactor.puch_back(3);
}

// my_vactor = {1,2,3};
```


### emplace_back()
`emplace_back()` 를 `push_back()` 대신에 쓰게되면, 임의로 vactor class를 생성해 구조체에 자리를 지정해 push 할 수 있다.
예를들어, 
```C++
my_vector.emplace_back(5);
```
와 같은 코드를 작성했을때 -> vector의 구조체에 5번째 자리에 element가 추가된다.


### size()
`size()` 를 사용하면 vector의 element의 수를 알수있다.

### empty()
`empty()` 를 사용하면 vector가 비어있는상태인지 아닌지에 대해 알수있다.
만약, vector가 비어있을 경우엔 **true** 를 반환하고 아닐 경우엔 **false** 를 반환한다.



---
## c++ programming tip
- 거의 배열 대신에 vector 를 쓴다
- 
