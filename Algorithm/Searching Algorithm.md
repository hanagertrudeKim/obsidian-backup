---
tags:
  - algorithm
---

## intro
- 컴퓨터는 한번에 데이터를 파악할 수 없다. 
-  데이터를 하나씩 확인해야함
-  [[big-O]] 표기법, [[big-O#big-Omega | big-Omega]] 표기법


## 선형 검색 

### 정의 
- linear search
- 처음부터 순차적으로 검색하는 방법
- 언제 선형 탐색을 쓰나?
	- 정렬되어있지 않은 상태 
	- 그 어떤 정보도 없이 값을 찾아야할때

## 효율성
- O(n)

### 의사 코드
```
for i from 0 to n-1

if i'th elemnet is 50
	return true;

return false;
```


## 이진 탐색

### 정의
-  binary search 
- 두 구간으로 나누어서 탐색

### 효율성 
- O(log n)

```
if no item 
	return false

if middle item is 50
	return true

else if middle item < 50
	search left items

else if middle item > 50
	search right items


```