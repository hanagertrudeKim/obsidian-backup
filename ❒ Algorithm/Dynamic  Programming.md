---
tags:
  - algorithm
  - big-o
---
## define
- 주어진 문제를 여러개의 부분 문제들로 나누어 푼 다음, 그 결과들로 주어진 문제를 푼다.
- = **자신보다 작은 부분 문제 답을 모두 알면 이번 문제를 빨리 풀 수 있다.**
- DP는 겹치는 문제가 발생하기 때문에 메모이제이션이 필요하다
	- 디피 =/ 메모이제이션 이지만 둘의 연관성 깊음


## concept
아래 세가지의 컨셉을 이용할 것
1. [[recursion]] (Top-down)
	1. 가장 호출하는 문제는 가장 큰 문제이기 때문에 큰 문제부터 방문
	2. 가독성이 좋고, 본래 점화식을 이해하기 쉽다
2. store (memoize)
3. bnottom-up
	1. 가장 작은 문제들부터 차례차례 답을 쌓아올려감
	2. 함수를 별개로 부르지않아 시간과 메모리를 소량 더 절약할 수 있음

```ad-info
피보나치 수열
1, 1, 2, 3, 5, 8 => 점점 더해가는 수열

fib(n)
n = 3 => 2
n = 4 => 3
```
![](https://i.imgur.com/pen9rUD.png)


- fib(5)를 호출했을때, 각각 Recursion 되는 함수를 그래프로 나타내면 다음과 같다.
- 보면, fib(3) 그래프와 fib(2), fib(1) 그래프가 중복되는 것을 볼수있다.
- 이러한 비효율을 없애기 위해 도입한것이 store 컨셉이다.


## memoized solution

```python
def fib(n, memo)
	if memo[n] !=  null;
		return memo[n]
	if n==1 or n==2
		result = 1
	else:
		result = fib(n-1) + fib(n-2)
	mmemo[n] = result
	return result;
```
- memo 에 n인덱스의 값이 Null인지 체크
	- 값이 있다면
	- = **계산한 적이 있다면 추가 재귀 없이 그값을 바로 리턴한다.**
- 새로운 계산을 할때 memo 배열의 N 인덱스에 값을  저장한다
	- ![](https://i.imgur.com/xRpOxJy.png)


### 시간복잡도 비교
- dp를 사용하지 않았을 때 => o(n^2)
	- 기하급수적으로 늘어나는 호출 횟수로 지수 시간복잡도가 발생한다
	- = 큰 수는 구하기 힘들다
- dp를 사용했을 때  => o(2n+1) * o(1) = o(n)
	- O(2n+1) => 새로운 Result 값을 계산할때 드는 연산
	- O(1) => n번째 인덱스 찾기, n=1 Or n=2 연산

## Bottom-up Approach
```python 
def fib_bottom_up(n):
	if n==1  or n==2:
		return 1
	bottom_up = new int[n+1]
	bottom_up[1] = 1
	bottom_up[2] = 1
	for i from 3 upto n:
		bottom_up[i] = bottom_up[i-1] + bottom_up[i-2]
	return bottom_up[n]
```

### 시간복잡도 비교
- Bottom-up approach 사용했을때
	- O(n) => for문에서 N-2 만큼 반복하기 때문에


[동적 계획법(Dynamic Programmin.. : 네이버블로그](https://blog.naver.com/kks227/220777103650) => 관련 문제 풀기


##  B1463 - 1로 만들기 문제
나의 답 - 오류
```c++
int solution(int N) {
  int count{0};

  while (N != 1) {
    if ((N - 1) % 3 == 0) {
      N = (N - 1) / 3;
      count++;
    } else if (N % 3 == 0)
      N = N / 3;
    else if (N % 2 == 0)
      N = N / 2;
    else
      N = N - 1;
    count++;
  }
  return count;
}
```
- 나는 그리디 알고리즘을 적용해 큰수로 처음부터 계속 나누는 것이 제일 빨리 1로 도달할거라 생각했다.
- 그러나 **[[Greedy Algorithm]] 로 풀었을때 10 - 9 - 3 - 1 로 만드는 방법이 최솟값이 된다는 반례가 존재**한다.
- ==> 그래서 동적 계획법을 이용해야 하는 것이다
- 동적 계획법으로는 1을 빼는 경우, 각각 2, 3 으로 나누는 경우 모든 경우의 수 중 최솟값을 찾아낼 수 있기 때문
-  = 정해진 최선의 연산 방법이 없기 때문에 **다 시도** 해야 한다라는 걸 전제로 깔고 가자.

```ad-check
경우가 세가지로 나뉘어질 경우 점화식을 써보자
f(N) 이 최솟값을 구하는 함수라고 칠떄

0 (N=1 일 경우)
f(N/3)+1 (N이 3의 배수일 경우)
f(N/2)+1 (Ndl 2의 배수일 경우)
f(N-1)+1 (둘다 아닐 경우)

min() 을 이용하여 이것들 중에서 제일 작은 연산을 골라 한다

![](https://i.imgur.com/tnpEmsQ.png)

```

- top-down 방식
```c++
int f(int n){
	if(n == 1) return 0; //base case
	if(dp[n] != -1) return dp[n];

	int result = f(n-1)+1;
	if(n%3 == 0) result = min(result, f(n/3) + 1);
	if(n%2 == 0) result = min(result, f(n/2) + 1);
	dp[n] = result;
	return result;
}
```
- base case : N=1이면 0 을 return
- other case : 가장 작은 값으로 result 를 갱신하고 dp 배열에 없데이트


영상 참고하기 : [Dynamic Programming - Learn to Solve Algorithmic Problems & Coding Challenges - YouTube](https://www.youtube.com/watch?v=oBt53YbR9Kk&list=PL9-eAg5PFfn5179QQUL2-g7a9euH9W2zN&index=1) 