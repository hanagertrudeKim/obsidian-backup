# recursion (재귀)

- 함수내에서 자기 자신을 호출하여 반복하는것
- 무한 루프에 빠질수있음
	- => 자기 자신을 호출하는 종료 case가 있어야함. 
- 예제 
```c++
int function(int a){
	if(a == 0) return 0;
	if(a != 0) return a+ function(a - 1);
}

int main {
	int result = function(10);
}
```
- 종료 조건 
	- `n == 0` 
- `return a + funtion(a-1)` 
	- `n+1` 이였다면, 무한 루프에 빠진다. (끝도없이 증가함)
	- <span style="background:#affad1">종료 조건( n== 0) 에 수렴하도록 구조를 짜야한다.</span>

## 재귀 알고리즘 설계

- 적어도 하나 이상의 종료 case가 존재
- 모든 case는 종료 case로 수렴
- 매개 변수를 최대한 일반화 하기(??뭔소린지 아직 잘 모르겟음)
