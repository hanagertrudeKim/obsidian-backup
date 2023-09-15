
## 데이터와 데이터 추상화

### 데이터
- 데이터를 표현하고 유지하는 형태로 소프트웨어 성능 향상
	- => **자료구조 학습 이유**
- 상황에 따라 **추상화** 생각

### 타입 ( = 자료 유형)
- 프로그래밍에서 데이터 종류
- 해당 타입의 범위(도메인) + 할수있는 연산을 통해 정의
- 추상 데이터 타입 (ADT)?
	- int 는 ADT x : 정수의 구현, 범위 제한적임

### 타입의 종류
- 원시 타입 (primitive type)
	- 값 하나만 유지 ( = atomic type)
	- int float 등
- 복합 타입 (composite type)
	- 여러개 값 유지
	- 배열, 문자열 등
- 값 타입 vs 참조 타입
	- 값 : 갑 자체 유지 
	- 참조 : 값의 위치 정보 유지 (ex, c++ pointer)


### 자료구조
- 복합 타입의 구체적 구현
- 특징
	- 개별 요소로 나눌수있음
	- 접근방법 결정
	- 추상화
- ADT vs 자료구조
	- ADT: 논리적 구조 => 공간 복잡성, 연산 복잡성 계산 x
	- 자료구조: 구체적 구현 => 공간 복잡성, 연산 복잡성 계산 o


### 자료구조의 종류
- 필수 자료구조 8개
	- [[Array]]
	- [[Linked List]]
	- [[Stack]]
	- [[Queue]]
	- [[Hash table]]
	- [[Graph]]
	- [[Tree]]
	- [[Heap]]

- 선형 vs 비선형
	- 선형
		- 리스트, 스택, 큐(우선순위 큐 제외)
	- 비선형
		- 트리, 그래프
- 동질구조 vs 비동질구조
- 정적 vs 동적
	- 정적 자료구조인 배열은 처음 범위 선언이 중요함

### 범용 자료구조
- 데이터 타입에 제한이 없는 자료구조
- c++ template
```c++
// Use of template

template <typename T> T myMax(T x, T y)
{
	return (x > y) ? x : y;
}

int main()
{
	// Call myMax for int
	cout << myMax<int>(3, 7) << endl;
	// call myMax for double
	cout << myMax<double>(3.0, 7.0) << endl;
	// call myMax for char
	cout << myMax<char>('g', 'e') << endl;

	return 0;
}

```


### 자료구조와 알고리즘의 상관관계
- 매우 밀접한 관게를 맺고있음
- 자료구조의 선택 -> 효율적인 알고리즘의 선택