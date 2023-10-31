---
tags:
  - algorithm
---

# sliding window 알고리즘
- 고정된 사이즈의 윈도우가 이동하면서 윈도우 내에 있는 데이터를 이용해 문제를 풀어야할때 사용
- **교집합의 정보를 공유하고, 차이가 나는 양쪽 끝 원소만 갱신**
- 배열이나 리스트의 요소의 일정 범위의 값을 비교할때 유용
- two pointer 알고리즘과 연동하여 많이 쓰임.
	- two pointer : 1차원 배열이 있고 이 배열에서 각자 다른 원소를 가리키는 2개의 포인터를 조작하며 원하는 값을 얻는 알고리즘
	- two pointer는 구간의 넓이에 따라 유동적으로 변함 / sliding window는 구간의 넓이가 고정값


### 예제 0 
문제 : 숫자 배열에서 3길이의 합의 최대값 구하기

<풀이 요약>
- 범위를 한칸씩 오른쪽으로 움직임
	- 한칸 이동한 왼쪽값은 빼줌
	- 한칸 이동한 오른쪽값은 더해줌
- 값을 계속해서 비교함 => 최대값만 resutl 저장
- result 출력
```c++
int num[10] = {1,2,3,4,5,6,7,8,9,10}
int maxNum = 0;
int curNum = 0;

for(int i=0; i < 8; ++i){
	//1번째 값은 default 값으로 구함
	if(i == 0){
		curNum = num[i] + num[i+1] + num[i+2];
		maxNum = curNum;
	}
	//다음 번호부터 한칸 이동한 왼쪽값은 빼고 오른쪽값은 더함
	else {
		curNum = curNum - num[i-1] + num[i+3];
		if(curNum > maxNum) maxNum = curNum;
	}
	curNum = 0;
}

return maxNum;

```


### 에제 1
- leetcode 1456

```c++
class Solution {
	public:
		int isVowel(char c){
		return c == 'a' | c == 'e' | c == 'i' | c == 'o' | c == 'u';
}
  
int maxVowels(string s, int k) {
int maxCount = 0;
int curCount = 0;
  
for (int i = 0; i < (s.length() - k) + 1; ++i) {
	if(i == 0) {
	for(int j = 0; j < k; ++j){
		if (isVowel(s[j])) ++curCount;
	}
	maxCount = curCount;
	}
	else {
		if(isVowel(s[i-1])) --curCount;
		if(isVowel(s[i+k-1])) ++curCount;
	  
		if(curCount > maxCount) maxCount = curCount;
		if(maxCount == k) return maxCount;
		}
	}
	return maxCount;
	}
};
```