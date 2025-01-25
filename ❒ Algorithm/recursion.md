---
tags:
  - algorithm
---

[야, 너두 재귀할 수 있어: 재귀가 풀리는 4단계 접근법](https://velog.io/@eddy_song/you-can-solve-recursion#0%EA%B3%BC-1%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%9D%B8%ED%92%8B) => 이거 읽고 도움이 많이 되었다.
## Understanding Recursion
- 재귀란 함수가 **자기 스스로를 호출**
- 알고리즘 공부의 통곡의 벽.. 
	- 이게 안되면 다이내믹프로그래밍 다 안됨
- 정렬, 검색, 종주 문제에서는 recursion을 적용했을때, 쉽게 풀린다.
- **recutsion 알고리즘은 두가지의 케이스를 가진다.**
	- 함수가 반복되지 않는 경우 -> base 케이스 (종료 조건)
	- 수가 반복되는 경우 -> 재귀 케이스
```c++
int factorial( int n ){  
	if (n > 1) { /* Recursive case */
		return factorial(n-1) * n;
	} else { /* Base case */
		return 1; 
	}
}
```
위와 같은 factorial 함수의 인수로 4를 넣었다면, n이 1이 될때까지 재귀 함수를 호출할것이다.
그리고선 종료 조건 (n=1)이 된다면, 함수를 종료할것이다.
만약, 함수를 base case에 도달하지 못하게 설정한다면, 함수는 무한 루프를 돌것이고 stack overflow가 일어난다.
그러니 **항상 base case에 수렴할수있게 주의하자.**
![](https://i.imgur.com/Y0UaLEz.png)


## wrapper function??
```c#
int[] allFactorials( int n ){ /* Wrapper function */ 
	int[] results = new int[ n == 0 ? 1 : n ]; 
	doAllFactorials( n, results, 0 );  
	return results;
}  

int doAllFactorials( int n, int[] results, int level ){
	if( n > 1 ){ /* Recursive case */  
		results[level] = n * doAllFactorials( n - 1, results, level + 1 ); 
		return results[level];

	} else { /* Base case */ 
		results[level] = 1;  
		return 1;
	} 
}
```
만약 팩토리얼 과정을 배열에 기록해야할때 wrapper함수를 쓰면, 배열에 할당하는 작업과, 재귀 작업을 추적하면서 recursion 함수를 깔끔하게 관리할수있다고 한다.
세부적인 설계는 잘 모르겠지만 내가 공감한 장점은 아래와 같다.

**모듈화와 은닉 (Modularity and Encapsulation):** Wrapper 함수는 코드를 모듈화하고 각 부분을 캡슐화하는 데 도움이 됩니다. `allFactorials` 함수는 외부에서는 간단한 배열을 반환하며 내부에서는 실제로 계산이 이루어지는 `doAllFactorials` 함수를 호출합니다. 이렇게 하면 코드가 더 읽기 쉽고 유지보수하기 쉬워집니다.

**재귀적인 작업 추상화:** `doAllFactorials` 함수는 재귀적인 작업을 처리하고 중간 결과를 배열에 저장합니다. Wrapper 함수를 통해 이러한 재귀적인 작업을 추상화하고 외부에서는 단순히 결과 배열을 얻을 수 있습니다. 이로써 코드의 의도가 명확해지고 외부 사용자에게는 불필요한 세부 사항이 숨겨집니다.

### recursion vs literative function
- recursion을 사용하면 무조건 좋은것은 아니다.
	- 큰 오버헤드를 초래할수있기 때문
		- 오버헤드(overhead) : 함수를 실행하는데 필요한 추가적인 자원, 비용
		- 시간 오버헤드, 메모리 오버헤드, 통신 오버헤드, 처리기 오버헤드
- 따라서 반복적인 함수에서 반복자 구조를 쓰는것이 더 효율적일수도?

=> **반복 솔루션은 재귀 솔루션보다 대부분 더 효율적이다~**
```c++
int factorial( int n ){ 
	int i, val = 1;
	for( i = n; i > 1; i-- ) 
		val *= i;
	return val; 
}
```
위와 같은 경우도 추가적인 함수 호출을 하지 않기 때문에 더 효율적인 케이스.

```ad-note
interview 에서 만약 재귀적 알고리즘을 쓴다면, 재귀적인 해결책의 비효율성에 대해서도 언급하는것이 좋다.
만약 재귀적과 반복적 두 해결책의 복잡성이 동일할 경우에는, 두가지 해결책을 언급하고 반복적인 해결책을 써서 해결하는것이 좋겠다.
```
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
### 1. binary search
```ad-question
1. 이진탐색을 이용해서 정렬된 array의 주어진 인덱스의 Integer를 찾는 함수를 구현하라.
2. 이 탐색의 효율성을 말하고, 다른 탐색 메소드와 같이 비교해라
```

binary search
- 정렬된 공간에서 반으로 나눠서 탐색하는 방법
	- 만약 비교값이 중간의 값보다 작으면 => 중간의 위의값들을 배열에서 제거
	- 만약 비교값이 중간의 값보다 크면 => 중간의 아래값들을 배열에서 제거
	- 만약 비교값이 중간의 값과 같다면 => 탐색을 중지함.

recursion을 이용하여 binary search 구현
- binary search 에서 재귀 사용이 효율적인 이유는 연속적으로 작은 부분들에서 이진 탐색을 수행하기 때문
- 고려 요소?
	- 탐색 배열, 배열 사이즈, 인덱스
	- 탐색 공간의 크기 : 하한을 상한에서 뺀 다음 나누기 2 하한에서 상한을 뺀 나머지?
	- case
		- 중간 인덱스보다 큼 => 하한 = 하한 + 1
		- 중간 인덱스보다 작음 => 상한 = 상한 -1
		- 중간인덱스와 동일함 => 종료 반환
- 에러 상황 고려
	- array 가 정렬되지 않을수도있음 => 하한이 상한보다 커지는 경우
	- 찾고자하는 요소가 배열안에 없을 경우 => 하한 상한이 같아지면, 에러 throw
- 시간복잡도 
	- 아래와 같은 코드에서 시간복잡도는 O(log n) 이 된다.
	- 계속해서 반을 나누기 때문에.. 2의 제곱의 반대라고 생각하면 됨
```c++
int findInterger(int* array,int size, int number){
  int result{0};
  int startIdx{0}, endIdx{size};

  if(startIdx > endIdx) throw Error("new Error!");

  while(true){
    int searchIdx = (startIdx + endIdx) / 2;
	
	if(searchIdx == 0 && array[startIdx] != number) 
		throw Error("can't find number");
	if(array[startIdx] > array[endIdx])
		throw Error("array is not sorted");
    
    if(number < array[searchIdx]){
      endIdx = searchIdx - 1;
    } else if(number > array[searchIdx]){
      startIdx = searchIdx + 1;
    } else {
      return array[searchIdx];
    }
  }
  
  return 1;
}

```
```ad-note
대체로 재귀보다 반복연산자를 사용하는게 더 효율적.재귀솔루션을 사용하지않고 이진탐색 문제를 풀었을때 대체로 해설과 코드가 똑같았다.

but, 오류를 제대로 고려하지않았다. 당연히 배열은 정렬되어 들어오겠지, 당연히 찾는게 배열 안에 있겠지 등의 생각에서 기인해, 체크하는것을 가볍게 무시했던것같다.
다음부터는 가정의 상황에서 당연히 이렇게 될거라는 태도보다는 하나하나 꼼꼼히 의심하면서 가는게 중요할것같다. 오류 파악을 까먹지 말자.  
```

### 2. Permutations of a String
```ad-question
1. 문자열에서 가능한 모든 문자 순서를 인쇄하는 루틴을 구현
예를 들어, 문자열 "hat" => "tha", "aht", "tah", "ath", "hta", "hat" 
2. 문자열 "aaa"를 감안할 때, 당신의 루틴은 "aaa"를 여섯 번 인쇄해야 합니다.
```

만약에 word 3글자일때 가능한 가지의 수 : 3 * 2 * 1 = 6
따라서 글자수가 N 일때, 가능한 경우의 수는 N!
재귀를 사용해서 구현? or 반복자로?

만약 hat이라는 문자열이 주어졌을때, h a t 이 세 알파벳을 조합해서 나올수있는 경우의 수를 계산해야함
1. 첫글자가 h 일때
	1. 두번째가 a 일때
	2. 두번째가 t 일때
2. 첫글자가 a 일때
3. 첫글자가 t 일때

책 solve
- reverse engineering 을 알고리즘에 적용
	- revese engineering(역공학) : 정상적인 설계과정과 반대로, 생산된 제품을 분해하여 숨은 아이디어를 찾아내고 설계 도면을 뽑아내는 과정
- 만약 abcd 라는 문자열이 주어졌을때,
	- 직접 가능한 문자를 a부터 나열해봄
	- 가능한 가지의 수 : first position * second position * third position * fourth position
	     = 4 * 3 * 2 * 1 = 4! =  24
	 - 첫번째 글자를 set하고 그것이 변경되기 전에 가능한 모든 케이스를 뽑음
		 - 두번째 글자를 set하고 그것이 변경되기 전에 가능한 ~
			 - 세번째 글자 ~
				 - 네번째 글자 ~
		- => 재귀 속성과 동일한 과정
- 그렇다면 재귀로 짤때 base case와 recursion case는 무엇이 될까?
	- base case -> n만큼 문자열이 차면 종료
	- recursion case -> n 위치에 가능한 모든 문자열을 배치하고, n + 1에 가능한 케이스를 모조리 찾는다.
- 한번 사용한 문자에 대해서는 사용하면 안됨
	- 그래서 bool value로 그 여부를 확인할수있게 설정

```c++
#include <iostream>
#include <vector>

class Permutations {
private:
    std::vector<bool> used;
    std::string out;
    const std::string in;

public:
    Permutations(const std::string &str) : in(str) {
        used.resize(in.length(), false);
    }

    void permute() {
        if (out.length() == in.length()) {
            std::cout << out << std::endl;
            return;
        }

        for (int i = 0; i < in.length(); ++i) {
            if (used[i]) continue;

            out.push_back(in[i]);
            used[i] = true;
            permute();
            used[i] = false;
            out.pop_back();
        }
    }
};

int main() {
    Permutations permutations("abc");
    permutations.permute();

    return 0;
}
```

1. **초기 상태:**
    - `out`은 빈 문자열, `used`는 모든 요소가 `false`인 배열입니다.
2. **첫 번째 호출:**
    - `out`에 "a"가 추가되고, `used`에서 "a"에 해당하는 위치는 `true`로 표시됩니다.
    - 재귀 호출: 현재 `out`은 "a", `used`는 {"true", "false", "false"}로 남은 문자는 "b"와 "c"입니다.
3. **두 번째 호출 (안쪽 재귀):**
    - `out`에 "ab"가 추가되고, `used`에서 "b"에 해당하는 위치는 `true`로 표시됩니다.
    - 재귀 호출: 현재 `out`은 "ab", `used`는 {"true", "true", "false"}로 남은 문자는 "c"입니다.
4. **세 번째 호출 (안쪽 재귀):**
    - `out`에 "abc"가 추가되고, `used`에서 "c"에 해당하는 위치는 `true`로 표시됩니다.
    - 기본 경우: `out`의 길이와 `in`의 길이가 같으므로 "abc"를 출력하고 함수 종료.
5. **다섯 번째 호출 (바깥쪽 재귀):**
    - "c"를 사용했던 표시를 취소하고, `out`에서 마지막 문자 "b"를 제거합니다.
    - 남은 문자 "c"에 대해 다음 반복을 수행.
6. **네 번째 호출 (바깥쪽 재귀):**
    - "b"를 사용했던 표시를 취소하고, `out`에서 마지막 문자 "a"를 제거합니다.
    - 남은 문자 "b"에 대해 다음 반복을 수행.
7. **여섯 번째 호출 (바깥쪽 재귀):**
    - "a"를 사용했던 표시를 취소하고, `out`에서 마지막 문자 ""를 제거합니다.
    - 남은 문자 "a"에 대해 다음 반복을 수행.
8. **마지막 상태:**
    - 모든 재귀 호출이 완료되면, 모든 가능한 순열이 출력되고 함수 종료.

### 3. Combination of a String
```ad-question
1. 문자열에 가능한 모든 문자 조합을 인쇄하는 기능을 구현하세요. 
2. 이 조합의 길이는 1에서 문자열의 길이까지 다양하다. 
3. 캐릭터의 순서만 다른 두 가지 조합은 같은 조합이다.
   Ex)"12"와 "31"은 입력 문자열 "123"과 다른 조합이지만, "21"은 "12"과 같다.
```

예를 들어서 "wxyz" 와 같은 문자열이 주어졌을때 아래와 같이 자리수별로 가능한 케이스를 나눌수있다.
![](https://i.imgur.com/h8YeqQ5.png)

but, 이것을 다르게 재배열할수있다.
재배열이 유용한지 확인하는것은 알고리즘을 찾는데 도움이 될 수 있다.

![](https://i.imgur.com/Zq1SOp4.png)


- x 로 시작하는 세트를 봐라. 그리고 오른쪽 y 세트와 비교했을때 x가 붙은 형태로 다시 반복된다는것을 알수있다.
- 오 그러네?
- 



### 4. telephone words
```ad-question
1. 주어진 전화번호에 대해 가능한 모든 문자 조합을 찾는 함수를 작성하는 것이 이 문제의 목표
![](https://i.imgur.com/0W8utX3.png)

2. 도우미 함수 char getCharKey( int telephoneKey, int place ) 이용

```

- 각 숫자별로 3개씩의 문자가 있음
	- => 3가지의 경우가 가능함 => n자릴수는 3^n 의 가짓수가 생겨남

- 책에서 7자리수를 조합해서 나오는 단어의 개수 상한값이 3^7

두가지 경로로 문자를 바꿀수있는데
1. 첫번째 문자로 시작하여 오른쪽 문자에 영향을 주거나
2. 마지막 문자로 시작하여 왼쪽 문자에 영향을 주거나

1번방법을 선택한다면,
1. i 위치의 문자를 변경할때, i+1 위치의 문자가 해당 값을 순환한다....
2. 










```c++
class TelephoneNumber {
private:
    static const int PHONE_NUMBER_LENGTH = 7;
    const int* phoneNum;
    char result[PHONE_NUMBER_LENGTH];

public:
    TelephoneNumber(const int* n) : phoneNum(n) {}
    void printWords() { printWords(0); }

private:
    void printWords(int curDigit) {
        if (curDigit == PHONE_NUMBER_LENGTH) {
            std::cout << result << std::endl;
            return;
        }

        for (int i = 1; i <= 3; ++i) {
            result[curDigit] = getCharKey(phoneNum[curDigit], i);
            printWords(curDigit + 1);
            if (phoneNum[curDigit] == 0 || phoneNum[curDigit] == 1) return;
        }
    }

    // A부터 시작하여 숫자에 따라 알파벳을 매핑
    char getCharKey(int digit, int index) {
      if (digit >= 2 && digit <= 9) {
        return 'A' + (digit - 2) * 3 + index - 1;
      }
      return ' ';
    }
};
```
