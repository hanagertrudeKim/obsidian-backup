# Interface

---

- interface
    - 객체의 type을 지정할수있다.
    
    ```tsx
    interface User {
    	name : string;
    	age : number;
    	gender? : string;  //선택적으로 들어오는 값에는 ? 처리를 해줌
    	readonly birthYear : number;   //readonly를 사용하면 수정이 불가능하다. 
    }
    
    let user : user = {
    	name : "hana";
    	age : 20;
    	birthYear : 2003;
    }
    
    user.age = 21
    user.gender = "male"
    
    console.log(user.name) // hana
    ```
    

- interface 안에서의 여러 중복의 type 지정
    - [key : type] : type
    
    ```tsx
    interface Students {
    	name : string;
    	[score : number] : string;
    }
    
    const students : Students = {
    	name : 'hana'
    
     /*중복되는 key와 value 설정 가능*/
    	1 : "A",
    	2 : "B" 
    }
    ```
    
    - 만약 “A”와 “B” 같이 값이 string중에서도 값이 고정되어있는 경우 따로 type 처럼 정해줄수있다.
    
    ```tsx
    type Score = 'A'|'B'|'C'|'F'
    
    interface Students {
    	name : string;
    	[score : number] : Score;
    }
    
    const students : Students = {
    	name : 'hana'
    
     /*중복되는 key와 value 설정 가능*/
    	1 : "A",
    	2 : "B" 
    	3 : "a" // error
    }
    ```
    

---

### Interface 를 이용한 함수 지정

- 함수의 인자와 return 값에 type을 지정할수있다.

```tsx
interface Add {
	(num1:number, num2:number) : number
}

const add : Add = (x, y) = {
	return x + y
}
```

또다른 예시

```tsx
interface isAdult {
	(age:number) : boolean
}

const adult : isAdult = (age) => {
	return age > 19; //true or false 를 반한한다.
}  
```

### interface 확장

- extends 를 이용하여 interface를 확장할수있다.

```tsx
interface Cars {
	color: string;
	wheels: number;
	start(): void;
}

interface Benz extends Cars {
	door: number;
	stop(): void;
}
```

- 여러개의 interface를  extends 하는법

```tsx
interface Car {
	color: string;
	wheels: number; 
	start(): void;
}

interface Toy {
	name: string;
}

//interface 확장

interface ToyCar extends Car, Toy {
	price: number;
}
```