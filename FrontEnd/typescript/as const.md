
- [참고 링크1](https://blog.toycrane.xyz/typescript%EC%97%90%EC%84%9C-%ED%9A%A8%EA%B3%BC%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%83%81%EC%88%98-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0-e926db079f9)
- 리터럴 타입 [[유니온（＆） 교차 타입#Literal type]]


## 타입 추론

**`typescript 3.4` 에서 부터 컴파일러가 자동으로 타입을 추론해줌.**

### let 선언

- let으로 선언할 경우
	- 변수의 값을 언제든지 바꿀수 있기 때문에 좀 더 포괄적인 [[literal & primitive type]] (string) 으로 추론함.
```typescript
	let title = "typescript"
	
 	-> title의 type을 자동으로 string으로 추론함
```


### const 선언

- but, const로 선언할 경우엔 
	- 변수의 값이 변경 x 때문에  [[#literal type]] 으로 추론함. 
```typescript
	const title = "typescript";
	
  -> title의 type을 "typescript"로 추론함	
```


---


## const assertion (타입 단언하기)

- let 도 const처럼 literal type으로 추론해줄수있음 -> `as const`  이용 
```typescript
	let title = "typescript" as const
	
	-> title 의 type이 "typescript" 로 지정됨.
```



### const assertion을 활용한 상수 관리하기

```typescript
const Colors = {  
	red: "#FF0000",  
	blue: "#0000FF",  
	green: "#008000"  
	}
```

- Colors 내부에 추론된 값을 보면 각각의 값들이 primitive type으로 추론됨.
	- why? -> const로 선언되었지만 object 내부의 값들은 바뀔수있기 때문에.


```typescript 
const Colors = {  
	red: "#FF0000",  
	blue: "#0000FF",  
	green: "#008000"  
	} as const
```

- as const를 이용해서 선언해주면 내부의 값 type이 literal type으로 추론될 수 있다. 