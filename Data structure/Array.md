## 배열

### 배열 선언

- int 배열이름[배열길이]
- ex )
```c
int array[10];
float cost[12];
char name[10];
```


```ad-tip
배열의 크기를 선언할때 -> 기호상수를 이용한다.

(why? 배열의 크기를 변경하기 쉬워진다.)

ex) #define SIZE 10;
	int score[SIZE];
```


### 배열 지정

- 배열 선언할떄 [ ]  의 기호가 아닌 { } 의 기호를 써야한다.
```c
int array[5] = {1,2,3,4,5};
```


