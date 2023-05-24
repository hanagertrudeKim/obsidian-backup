
## 버블정렬

### 정의
- bubble sort 
-  옆에 있는 숫자와 비교해서 크다면 자리를 바꾸고, 아니라면 그만둠 
- 장점
	- 직관적이다 

### 의사 코드
```
repeat n-1 time
	for i from 0 to n-2
		if i'th and i+1'th person out of order
		swap them
```

### 효율성
```math
= (n-1) * (n-1)
= (n-1)^2
= n^2 - 2n + 1

= O(n^2)
```
- big-O => O(n^2)
- big-Omega => O(n^2)


---
## 선택 정렬

### 정의
-  selection sort
- 가장 작은 숫자만을 선택해서 (하나씩) 앞으로 보내기

### 의사 코드
```
for i from 0 to n-1
	find smallest item between i'th and last item
	swap smallest with i'th item 
```

### 효율성
```math
= n + (n-1) + (n-2) + (n-3) + ... + 1
= n(n+1)/2
= n^2/2 + n/2

= O(n^2)
```

- big-O => O(n^2)
- big-Omega => O(n^2)
