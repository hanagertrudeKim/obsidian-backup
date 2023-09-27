---
tags:
  - FrontEnd
  - typeScript
---
# type 정리

- type 정의

```tsx
let who:string = "hana"; //문자열 type 정의 

let age:number = 20;  //number type 정의

let isAdult:boolean = true; //boolean type 정의
```

- 배열 type 정의
    - type[ ]
    - Array<type>

 
```tsx
let a2:number[] = [1,2,3]; //number array type 정의

let week:string[] = ['mon', 'tue', 'wed']; //string array type 정의

let week1:Array<string> = ['mon', 'tue', 'wed']; //string array type 정의
```

- 튜플 (tuple)
    - index 순서별로 고정 type을 정할때

```tsx
let a:[number,string]
a = [1,"hello"]

let b:[string,boolean] = ["hi", true];
```

- void
    - 함수에서 아무것도 반환하지 않을때 사용

```tsx
function sayHello():void(
	console.log("hello");
)
```

- never
    - 에러를 반환하거나 영원히 끝나지 않는 함수에서 사용

```tsx
function showError():never{
	throw new Error()
};

function infLoop():never{
	while (true){
		//do somthing
	}
}
```

- enum
    - 특정값들이 입력되도록 강조하고싶을때, 그 값들을 강조하고 싶을때
    - 자동으로 0부터 1씩 증가하면서 할당됨,
    - but, 직접 할당해주면 변경됨
        - 직접 할당해주는 경우에 양방향 랩핑이 됨.

```tsx
enum Os {
	window,
	Ios,
	Android
}
```

- null, undefined

```tsx
let a:null = null; //null type 선언

let b:undefined = undefined; //undefined type 선언
```