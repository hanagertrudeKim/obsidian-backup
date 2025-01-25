---
tags:
  - algorithm
---
## 버블정렬
### 정의
- bubble sort 
-  옆에 있는 숫자와 비교해서 크다면 자리를 바꾸고, 아니라면 그만둠 
- 장점
	- 직관적이다 
### 의사 코드
```c++
repeat n-1 time
	for i from 0 to n-2
		if i'th and i+1'th person out of order
		swap them
```
### 효율성
```ad-summary
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
```c
for i from 0 to n-1
	find smallest item between i'th and last item
	swap smallest with i'th item 
```
### 효율성
```ad-summary
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

## selection sort (선택 정렬)
### Stable Selection sort
```ad-question
Implement a stable version of the selection sort algorithm.
```
문제 
- 안정적인 버전의 선택 정렬 알고리즘을 구현
- 선택정렬 
	- 아직 정렬되지 않은 배열을 반복하여 가장 낮은 키를 찾음
	- 그 값을 정렬된 배열의 맨 끝 위치로 스왑
```c++
// 안정적인 선택 정렬을 사용하여 배열을 정렬합니다.
void selectionSortStable(int[] data) {
    for (int start = 0; start < data.length - 1; ++start) {
        insert(data, start, findMinimumIndex(data, start));
    }
}

// 데이터를 배열에 삽입하고 필요에 따라 배열을 이동합니다.
void insert(int[] data, int start, int minIndex) {
    if (minIndex > start) {
        int tmp = data[minIndex];
        for (int i = minIndex; i > start; --i) {
            data[i] = data[i - 1];
        }
        data[start] = tmp;
    }
}
```
=> 시간 복잡도를 계산해보면
- 삽입 / 삭제 => O(n)
- 최소값을 찾기 위한 배열 순환 => O(n)
- 그래서 총 O(n^2)

but, 삽입과 삭제의 연산을 O(1) 로 처리할수있는 링크드리스트를 이용하면, O(n) 을 이용할수있다.

```c++
#include <iostream>
#include <list>

void selectionSortStable(std::list<int> &data) {
  auto begin = data.begin();
  auto end = data.end();

  while (begin != end) {
    auto minIt = begin;
    auto current = begin;
    ++current;

    while (current != end) {
      if (*current < *minIt) {
        minIt = current;
      }
      ++current;
    }
    std::iter_swap(begin, minIt);
    ++begin;
  }
}
```

### optimize Quicksort
```ad-question
Implement an efficient, in-place version of the quicksort algorithm.
```
- 효율적인 inplace 버전의 quicksort 알고리즘을 구현하라

- 일단 quicksort 알고리즘은
	- 정렬해야하는 요소들 중에 피벗 값을 선택한다.
		- 피벗은 그냥 중간값, 맨끝값중에서 설정
	- 나머지 요소들을 피벗을 기준으로
		- 작은값 => left 리스트로 나누고
		- 큰값 => right 리스트로 나눔
	- 재귀적으로 위 과정을 반복하여 정렬한다.
	- 재귀 호출이 완료된 후에 left, 피벗, right 를 이어붙인다

- 여기서 추가 메모리 할당을 없애려면 어떻게 해야하냐?

메인 함수인 `quick_sort()`는 크게 `sort()`와 `partition()` 2개의 내부 함수로 나눠집니다. 

`sort()` 함수는 재귀 함수이며 정렬 범위를 시작 인덱스와 끝 인덱스로 인자로 받습니다.
`partition()` 함수는 정렬 범위를 인자로 받으며 다음 로직을 따라서 좌우측의 값들을 정렬하고 분할 기준점의 인덱스를 리턴합니다. 이 분할 기준점(mid)는 `sort()`를 재귀적으로 호출할 때 우측 리스트의 시작 인덱스로 사용됩니다.

- 리스트의 정 가운데 있는 값을 pivot 값을 선택합니다.
- 시작 인덱스(low)는 계속 증가 시키고, 끝 인덱스(high)는 계속 감소 시키기위한 while 루프를 두 인덱스가 서로 교차해서 지나칠 때까지 반복시킵니다.
    - 시작 인덱스(low)가 가리키는 값과 pivot 값을 비교해서 더 작은 경우 반복해서 시작 인덱스 값을 증가시킵니다. (pivot 값보다 큰데 좌측에 있는 값을 찾기 위해)
    - 끝 인덱스(high)가 가리키는 값과 pivot 값을 비교해서 더 작은 경우 반복해서 끝 인덱스 값을 감소시킵니다. (pivot 값보다 작은데 우측에 있는 값을 찾기 위해)
    - 두 인덱스가 아직 서로 교차해서 지나치치 않았다면 시작 인덱스(low)가 가리키는 값과 끝 인덱스(high)가 가리키는 값을 상호 교대(swap) 시킵니다. (잘못된 위치에 있는 두 값의 위치를 바꾸기 위해)
    - 상호 교차 후, 다음 값을 가리키기 위해 두 인덱스를 각자 진행 방향으로 한 칸씩 이동 시킵니다.
- 두 인덱스가 서로 교차해서 지나치게 되어 while 루프를 빠져나왔다면 다음 재귀 호출의 분할 기준점이될 시작 인덱스를 리턴합니다.


```python
#include <iostream>
#include <vector>

void swap(int &a, int &b) {
  int temp = a;
  a = b;
  b = temp;
}

int partition(std::vector<int> &arr, int low, int high) {
  int pivot = arr[(low + high) / 2];
  while (low <= high) {
    while (arr[low] < pivot) {
      low++;
    }
    while (arr[high] > pivot) {
      high--;
    }
    if (low <= high) {
      swap(arr[low], arr[high]);
      low++;
      high--;
    }
  }
  return low;
}

void quickSort(std::vector<int> &arr, int low, int high) {
  if (high <= low) {
    return;
  }

  int mid = partition(arr, low, high);
  quickSort(arr, low, mid - 1);
  quickSort(arr, mid, high);
}
```





