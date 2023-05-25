

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
- big-Omega => Ω(n^2)


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
- big-Omega => Ω(n^2)


## 병합 정렬

### 정의
- 원소가 한 개가 될떄까지 계속해서 반으로 나누다가 다시 합쳐나가며 정렬하는 방식
- 재귀적으로 구현됨
- 메모리가 추가적으로 더 필요함

### 의사 코드
```
On input of n elements
	if n < 2
		return 
	else 
		sort left half of elements
		sort right half of elements
		merge sorted halves

```

```
7 | 4 | 5 | 2 | 6 | 3 | 8 | 1 → 가장 작은 부분 (숫자 1개)으로 나눠진 결과입니다.

4   7 | 2   5 | 3   6 | 1   8 → 숫자 1개씩을 정렬하여 병합한 결과입니다.

2   4   5   7 | 1   3   6   8 → 숫자 2개씩을 정렬하여 병합한 결과입니다.

1   2   3   4   5   6   7   8 → 마지막으로 숫자 4개씩을 정렬하여 병합한 결과입니다.
```


### 효율성
- 숫자를 반으로 나누는 효율 => O(log n)
- 반으로 나눈 부분을 다시 정렬 => O(n)

- big-O => O(n log n)
- big-Omega => Ω(n log n)

