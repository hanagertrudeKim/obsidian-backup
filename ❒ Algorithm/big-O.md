---
tags:
  - algorithm
  - data structure
  - big-o
---
참고 : 
[Big-O Algorithm Complexity Cheat Sheet](https://www.bigocheatsheet.com/)
## 시간 복잡도

- 대부분의 알고리즘에서는 big-O 표기법을 씀.
	- **big-O, big-Omega, big-theta**
	- 각각 **상한, 하한, 딱 맞는 수행 시간**을 의미함. 
		- **실행 시간 상한**
			-   O(n^2): 선택 정렬, 버블 정렬
			-   O(n log n): 병합 정렬
			-   O(n): 선형 검색
			-   O(log n): 이진 검색
				- 무언가를 절반으로 계속 나눈다면 로그함수로 표현할수있다. 
			-   O(1)
		- **실행 시간 하한**
			-   Ω(n^2): 선택 정렬
			-   Ω(n log n): 병합 정렬
			-   Ω(n): 버블 정렬
			-   Ω(log n)
			-   Ω(1): 선형 검색, 이진 검색
	- 업계에서는 big-O를 쓴다 (상한값만을 쓴다.)


## 공간 복잡도

- 입력의 크기와 알고리즘이 내부적으로 사용하는 공간의 크기를 합함
	- = input space + **auxiliary space**
- 대표적 예시) containDuplicates
- 재귀호출 알고리즘 -> 함수 스택 공간을 공간 복잡도에 포함함
	- 함수 스택 : 매개변수 1개 유지
	- 재귀 호출 깊이 : n -> 공간복잡도 **O(n)**
```c++
sum(n): 
	if n = 1: return 1 
	return n + sum(n – 1)
```




## big-O notiation

- 알고리즘의 효율성을 나타내는 지표
	- 시간복잡도, 공간복잡도 분석에 모두 사용 가능
- O( ) 함수안에 하나의 수식을 넣어서 사용
	- ex) O(1), O(n), O(nm), O(n^2)
- **big-o는 n이 충분히 클 경우에만 의미가 있음**
	- type of measurement
		- **worst-case**(기준)
		- best-case
		- everage-case
- big-O 함수의 계수는 비교에 영향을 줄수없음
	- but, 빅O가 같으면 생략된 계수와 항까지 비교해야 함
- 입력의 크기
	- 정수가 커질수록 비용 증가 (=비트수 증가)
	- =  컴퓨터에 표현하기 위한 문자 수 
- **순서**
	- **O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n) < O(n!)** 등
	- 왼쪽부터 시간이 가장 짧은 순


### O(N), O(1)

- o(1) : constant time
- o(N) : linear time


> [!example]
> 문제 : 1부터 n 까지 합을 구하라.

**(1)번 방법**
```c++
int main()
{
    int n, sum = 0;
    scanf("%d", &n);
    for(int i=1; i<=n; i++)
        sum += i;
        
    printf("%d",sum);
}
```
- 이에 대한 시간 복잡도를 big-O 표기법으로 나타내보면
	- for 문에서 총 3번의 연산이 필요하다.
		- `i <= n `
		- `i++ `
		- `sum += i`
	- 따라서, n 에 따라서 3N번의 연산이 필요함
	   -> O(3N) => **O(N)**

**(2)번 방법 -> 공식 이용하여 시간복잡도 줄이기**
```c++
int main(){
    int N, sum;
    scanf("%d", &N);
    sum = N*(N+1)/2;
    printf("%d\n", sum);
}
```
- 이에 대한 시간 복잡도를 big-O 표기법으로 나타내보면
	- 총합을 구하는 식을 통해 N에 상관없이 연산 1번이 필요함
	  => **O(1)**



### O(NM), O(N^2)

```c++
int main(){
    int N, M;
    scanf("%d %d", &N, &M);
    for(int i=0; i<N; i++)
        for(int j=0; j<M; j++)
            printf("%d %d\n", i, j);
}
```
- 2중 for문일 경우,
	- (처음 for문 N번 반복) * (나중 for문 M번 반복)
	**=> 시간 복잡도는 O(NM) 이 된다.**
	
```c++
int main(){
    int N;
    scanf("%d", &N);
    for(int i=0; i<N; i++)
        for(int j=0; j<N; j++)
            printf("%d %d\n", i, j);
}
```
- 이 경우엔, N이 for문에 중복해서 쓰였다.
	- (처음 for문 N번 반복)* (나중 for문 N번 반복)
	**=> 시간 복잡도는 O(N^2) 이 된다.**


## big-Omega (lower bound)

- big-o 와 반대
- 최선의 경우를 나타내는 지표
- 선형 탐색
	- big-O => O(n)
	- big-Omega => Ω(1)
		- 최선의 경우: 1번
- 이진 탐색
	- big-O => O(log n)
	- big-Omega => Ω(1)
		- 최선의 경우: 1번



## big-Theta (both)

- 알고리즘의 **상한선(big-O)과 하한선(big-Omega)이 같을때** 사용




###  알고리즘과 시간복잡도 관계

- 알고리즘 효율성에 가장 영향을 많이 주는 요소는 **반복문** 이다. 
- **시간복잡도를 통해 연산에 드는 시간 판별이 가능**
	- 만약, O(NM)시간복잡도, N의 최대는 100, M의 최대는 1000이다. 
		=> 최대 100000번의 연산 과정이 든다.
		=> 제한시간 1초를 넘기지 않음
	- but, O(NM) 시간복잡도, N의 최대는 10000, M의 최대는 10000
		=> 최대 1억번의 연산 과정이 든다.
		=> 제한 시간을 초과함
		
> [!note]
> big-O의 시점에서 볼때, 차수를 줄여야 효율이 좋아졌다고 볼수있음
> 
> 알고리즘 문제에서 시간초과가 뜰때 => 차수를 줄여야한다.

