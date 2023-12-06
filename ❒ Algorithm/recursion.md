---
tags:
  - algorithm
---
## 정의
- 함수가 **자기 스스로를 호출**
- 함수내에서 자기 자신을 호출하여 반복하는것
- 무한 루프에 빠질수있음
	- => 자기 자신을 호출하는 종료 case가 있어야함. 

## 재귀 알고리즘 설계
- 적어도 하나 이상의 종료 case가 존재
- 모든 case는 종료 case로 수렴
- 매개 변수를 최대한 일반화 하기(??뭔소린지 아직 잘 모르겟음)

## 재귀함수와 스택 메모리
재귀 함수를 이용할때 함수가 거꾸로 호출되어 출력되는 이유는 무엇일까?

재귀 함수는 동일한 함수를 계속해서 호출할때 함수를 위한 [[memory]] 계속해서 할당된다.
함수가 호출될때 사용되는 임시 저장 메모리를 스택이라고 부른다.([[Stack]])
stack 은 LIFO (후입선출) 구조이기 때문에 가장 마지막에 호출된 함수를 먼저 실행하게 된다.

재귀 함수를 이용하면 함수가 종료되지않고, 계속해서 호출될수있는데, 이 경우 스택 공간이 초과되는 overflow 가 발생할수있다. 그렇기 때문에 재귀 사용시 스택 메모리가 초과되지 않도록 주의해야한다.


## 예시

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
	- 종료 조건( n== 0) 에 수렴하도록 구조를 짜야한다.


## 예시 2

### no recursion

```cpp
output: 
# 
## 
### 
####

---
void draw(int h){
	for(int i=0; i < h; i++){
		for(int j=0; j < i; j ++){
			std::cout << '#';
		}
	}
	std::cout << '\n';
}

int main(void)
{
    // 사용자로부터 피라미드의 높이를 입력 받아 저장
    int height = get_int("Height: ");

    // 피라미드 그리기
    draw(height);
}

```

### use recursion

```c++
void draw(int h)
{
    // 높이가 0이라면 (그릴 필요가 없다면)
    if (h == 0)
    {
        return;
    }

    // 높이가 h-1인 피라미드 그리기
    draw(h - 1);

    // 피라미드에서 폭이 h인 한 층 그리기
    for (int i = 0; i < h; i++)
    {
        printf("#");
    }
    printf("\n");
}

int main(void)
{
    int height = get_int("Height: ");

    draw(height);
}

```