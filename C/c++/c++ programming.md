
# 원시타입

## ignore

- `ingnore()` 은 지정한 문자를 만날때 입력버퍼에 대한 모든 데이터들을 없에준준다.
- <span style="background:#fff88f">cin과 getline을 혼용해서 쓸때 필요하다.</span>
- count: 읽을 개수 , char: 종료할 문자
```cpp
cin.ignore(count, char);
```

## 축소 변환

- 축소 변환
	- 도메인이 작은 타입으로 변환
- { } 초기화
	- 단순화, 강건성을 높임
	- <span style="background:#fff88f">축소변환에 해당하면 ERROR! 가 발생한다. </span>
ex) 
```c++
int number{10};

float a = 2.4 // => WARNING! 

int a{10};
float b{a} // => ERROR! 

```

## auto

- 초기값을 이용하여 자동으로 타입 추론
- usage
	-  매개변수 타입, 반환 타입 등
ex) 
```c++
int sum{0};
for (auto n; array){
	sum += n;
}

int a{10};
auto p{&a}; //type : int*
```

## 변수의 수명

- 자동 (auto)
	- stack 영역
	- 일반 지역 변수
- 정적  (static)
	- heap 영역
	- static 지역 변수
		- 함수가 실행되면 공간이 할당되고, 프로그램 종료때까지 수명이 유지됨
- 동적 (dynamic)
	- heap 영역
	- 동적할당된 변수
		- 공간 delete 될때까지 할당됨

## 가시영역 충돌
- c++은 허용함
- 가시영역 충돌이 일어나면 더<font color="#4f81bd"> 작은 scope의 변수</font>가 적용됨. 

## overflow
- 397번 leet code
	- 재귀함수 이용하기
	- max_int일때 오류남.
- 타입크기 최대, 최소 판별 
	- 헤더 상수 이용
		- std::numeric_limits::min() 
		- std::numeric_limits::max()
	
<span style="background:#affad1">### 주의점 </span>
if(bool == true) 와 같은 형태로 쓰지 X
=> if (bool) 와같이 써라

---

# 표현식

## signed vs unsigned
- signed 와 unsigned 가 충돌하면 rank 가 큰쪽으로 변환
	- signed => unsigned 경우 , unsigned 가 음수이면 big error!
	- <span style="background:#fff88f">가급적이면 signed, unsigned 혼합해서 사용하지 X </span>
```c++
std::vector<int> v;
for(int i =0; i < v.size(); i++) -> error!
```
-  i = signed 
- but, v.size() = unsigned 
	- => error
- 거꾸로 돌아가는 for문에서도 에러가 발생한다.

## Lvalue vs Rvalue
- Lvalue, Rvalue는 표현식의 분류
- Lvalue
	- 대입문의 왼쪽에 올수있는 것
	- ex) a,b 는 Lvalue, a * b 는 not Lvalue
- Rvalue
	- Lvalue가 아닌것

## 연산자 

- 산술연산자
	- a%b = a − (a/b) × b

- 홀수 판별식
```c++
(m%1) == 1; 
//or
(m&1) == 1;
```

- ++ or --
	- 가독성을 해칠수있음
		- 변수는 한 문장에서 한번만 바뀌는것이 ok
		- clean code is better than clever code
	- 독립적으로 써라
	- <span style="background:#fff88f">i++ 보다 ++i 가 더 효율적인 아이</span>
		- why? 
	- => `++i`  써라!
	  
## 지연 평가 
- 왜 if else 는 지연평가가 ㄴㄴ? => 애초에 지연 평가가 적용되지 않는 대상이다

## a ^ b (XOR)
- a 와 b 가 다를때 => T

##  비트 연산자
- 비트 뒤집기
```c++
mask <<= j;
mask = ~mask
return n & mask
```
- 특정비트 1 => or 
- 0 => and


## 고민해볼점
- for 문을 빠져나올때, break 를 써서 빠져나오는건 그리 효과적이지 X ? (중간에 return도 마찬가지)
- 비트 표현을 사용해서 출력해라 => 비트 연산자 활용하라
ex) 
```c++
return (n & (n-1)) == 0
```

---

# 배열, 문자열, 열거형

## 배열 
```c++
int n[5]{} // 0으로 초기화
```
- 최대한 라이브러리를 활용하는것이 좋다
- 연속 공간
	- studentScores 변수의 주소 값이 100이라고 가정 sizeof(int)가 4바이트이라고 가정 
	- studentScores[3]의 주소는? 100 + 3 × sizeof(int) ⟹ 112

## sliding window
- 

## 2차원 배열 
- 두개의 일차원 배열을 선언해서 사용하는것도 하나의 방법 
- 가독성 신경써라..

## vector
[[vector in C++]]

## string
`std::string`은 널 문자를 문자 중 하나로 유지할 수 없다??

## string_view
- 인자
	- string &(참조) 이용
		- 전달비용이 없음
		- 객체를 생성하지않음
		- 데이터 소유가 필요할때
	- string_view 이용
		- 값 전달
		- 시작 주소 + 메모리크기로 새 객체 생성
		- 데이터 소유가 필요하지않을때
- <span style="background:#fff88f">문자열 앞과 뒤에 있는 문자열을 효과적으로 할수있다.</span> ( string을 이용하는것보다..)
	- = 앞주소 뒷주소만 바꾸면댐
	- `remove_prefix` , `remove_suffix`

## syntatic sugar??

---
# pointer


## pointer 
- 주소를 저장하는 변수
- 한번에 두개 선언  X 
- * 가 앞으로 붙든 뒤로 붙든 별로 상관 x
- 포인터 타입과 데이터 타입이 일치해야한다.
```c++
int* const p //포인터 자체가 const (값은 변경 가능, 참조값 변경 불가능)
const int* p // 포인터가 가르키는 데이터가 const (수정 불가 포인터, 값 변경 불가능, 참조값 변경 가능)
const int* const p // 둘다 const (값, 참조값 둘다 걍 불가능)
```

- nullptr 
	- 포인터는 적절한 주소로 초기화하는것이 바람직함
- <span style="background:#fff88f">포인터가 가리키는 데이터를 접근할때는 * 연산자를 사용</span>


## 포인터와 배열
	- 포인터 배열
- 배열 포인터
	- 다차원 배열과 연계해서 사용
```c++
int *p[3]; => int포인터를 담고있는 배열
int (*q)[3]; => 배열을 가르키는 int 포인터

int a[3] = {1,2,3};
p = a;
// p = &a[0], p+1 = &a[1], p+2 = &a[2]

q = &a
// q = &a[0], q+1 = &a[3]

//둘의 차이점 알기
```

## pointer 와 auto
- 선언타입 순서
	- 선언할때 타입을 수식하는 수식어는 순서를 바꿔도 동일함
```c++
int const v{0}
const int v{0} // 두개가 같다
```
- auto & const
```c++
int a{0}
int b{0}
//참조주소 const
auto const p1{&a}
auto* const p2{&a}
const auto p3{&a}

p1 = &b //=> ERROR!

//참조값 const
auto const *p4{&a}
const auto *p5{&a}

*p4 = 0 // => ERROR!
```

## nullPtr
- 초기화 할 주소가 없으면 nullPtr 이용
```c++
int *p{nullptr}
if(p){} // p = false
```


## pointer의 필요성
- 주소 전달
	- 무거운 데이터( 원시 타입이 아니면 무거움 ) 효율성 
	- 수정 필요성?
- 동적 할당 => (실제로는 라이브러리가 다 구현해줌)
	- 언제 필요?
		- 필요한 크기의 편차가 클 경우
			- 편자가 크지않다면 최대값으로 공간을 잡음
		- 동적 자료구조 구현
		- <span style="background:#fff88f">원래 타입과 전혀 다른 타입으로 접근해야할때 (엄청 예외적)</span>
		- 복합타입에서 조작할때 사용
			- 배열을 정렬할떄, 값을 움직이는것보다 주소를 움직이는 것이 더 효율적이다
		- <span style="background:#fff88f">다형성을 활용하는 클래스 정의 (다른 객체를 멤버로 유지)</span>
		- 포린터 필요한 라이브러리 기능 사용할떄 
	- 확보한 새 공간의 주소 return (포인터이용)

## 동적할당
- 값의 편차가 클때 사용
- delete를 안하면 프로그램 종료 시점으로 알아서 판단(상당히 비효율적일수있음, 그다지 좋은 방법x)
- 동적할당 실수
	- 이미 반납한 포인터 사용
	- 반납 여러번
	- 해결 -> <span style="background:#fff88f">반납 후 해당 포인터에 `nullPtr` 사용함</span>


## 연결구조
- 필요할때마다 동적으로 확보해서 사용하는 구조
- 나중에 자료구조 linked list, 트리 할때 쓰임

## Lvalue참조변수 , Rvalue참조변수
- Lvalue
	- 참조 & 로 쓰임 => 왼쪽 변수가 오른쪽 변수를 부르는 또다른 이름
- Rvalue
	- 주소 & 로 쓰임 => 이미 선언된 변수나 값에 대한 주소를 까준다
	

---

# 조건문과 반복문

## if문
- 조건문
	- 정수형이 온다면
		- 0 == false, others == true
	- 포인터가 온다면
		- nullptr == false, others == true
 - dangling-else 문제
	 - else 는 가장 가까운 If 와 연관된다
- 다중 선택
	- <span style="background:#fff88f">if 문 나열보다 else-if 문을 활용하는게 더 효율적임</span>
	- <span style="background:#fff88f">데이터에 따라 다중선택의 순서를 다르게하면 효율성을 높일수있음</span>
		- 확률이 높은 순으로 위로 올리기


## 삼항연산자
- std::cout 안에서도 입출력으로 사용할수있음
```c++
std::cout << (grade >= 60) ? "passed" : "failed";
```
- 삼항연산자 내에서 또 삼항연잔자 쓸수 있음


## switch문
- break 문을 쓰지 않으면 다음 case로 넘어간다.
```c++
std::string word;
int vowelCount{0};
… 
for(size_t i{0}; i < word.length(); ++i){
switch(word[i]){
	case 'a': case 'e': case 'i': 
	case 'o': case 'u': 
	++vowelCount; break;
 }
```

## foreach for loop
- 배열과 같은 복합타입에 저장된 모든 요소를 차례로 방문하고자 할 때 사용함
```c++
int list[5]{2,5,7,3,6};
int sum{0};
for(int n: list){
	sum += n;
}
```


## break , goto
- break 는 해당 문이 포함되어있는 가장 안쪽 루프에만 적용됨
- <span style="background:#fff88f">goto 를  쓰면 내부 루프에서 중첩 루프를 전체 종료해줄수있따</span>.


## 반복문 작성 tip
- <span style="background:#fff88f">반복문 안에서 if문을 최대한 지양해라...</span> 
	- 조건문이 필요한 경우, 조건문 없이 작성이 가능한지 살펴봐야함
	- ex) 구간 분리 
- 반복을 일찍 끝낼 가능성으로 중간에 if 문 넣고 하는게 생각보다 효율적이지 못하다
  (그냥 for문으로 한번에 쭉 가는게 더 효율성 높음)

---

# 함수

## 함수 선언
- ODR - (one definition rule) 
- 값 전달 방식에서 const 는 의미 없다
	- but, 참조 전달할때는 const 가 의미 있다!
- [[Recursion]] 
	- **재귀 종료 조건이 젤 중요하다~
	- 함수호출은 저렴한 연산은 아니여서, for문이 더 효율적일수있다. 
	- 재귀호출로 함수 stack이 쌓인다
		- 공간 복잡도에 영향 줌
	- 메모이제이션
		- 중복 호출을 방지하기 위한 동적프로그래밍 방법
	- 꼬리 재귀

## 인자 전달
- string 인자로 받을때
	- Rvalue를 참조로 인자를 받으면  수정 가능(&&)


## 람다표현식
- 함수를 일반 데이터와 동일하게 처리할수있음
```c++
std::function <bool(int)> // int를 인자로 받아 bool을 반환
```

## 라이브러리의 활용
- `<algorithm>` 에 정의된 다양한 함수 잘 알아야한다.
- 라이브러리의 함수를 쓰면서 성능 생각해야한다.

---

# 객체와 클래스

## 클래스
- private 
	- 강건성 이유( 오류 최소화 )
	- getter , setter가 메서드로 필요한 이유
		- getName(), setName()
- 메서드의 const
	- 객체 상태를 변경하지 않겠다는 것을 컴파일러에게 알림
	- 컴파일러의 지능이 올라간다
	- const 메서드로 멤버 변수 수정하려하면 => error!
- 크기는 정하지 말고
	- 개발 환경에 따라 달라질수있음
	- sizeof() 이용하기
- getter
```c++
<1>
B getB() {
return B;
} //-> 비용이 많이 듬 (X)

<2>
B& getB(){
return B;
}

&B b =a.getB();
b.setB()// --> 캡슐화 위배 (X)

<3>
cosnt B& getB() const {
	return B;
}

cosnt &B b = a.getB();
b.setB() //-> 자동으로 error 처리해줌 (o)
```

## 객체지향
- OCP
	- open closed principle
	- 클래스는 수정에 대해서 닫혀있지만 확장은 열려있다
- 객체 지향의 장점
	- 한 클래스가 있을때 해당 클래스 수정 x, 확장하는 기본적인 방법
	- 방법  
		- 1. 상속
		- 2. 포함 관계
	- 