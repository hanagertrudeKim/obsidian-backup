# 함수(function)



## 함수 type

- 인수의 optional 성격을 보장할수있다.
    
    ```tsx
    const Hello = (name?: string) => {
    	return `Hello, ${name || "world"}`
    }
    
    const result = Hello() // Hello, world
    //type의 인수를 optional로 지정했기 때문에, 인자가 없으면 undefined로 지정됨.
    
    const result2 = Hello("hana")   // Hello, hana
    
    const result3 = Hello(13)       // error 
    ```
    
    - 좀더 간단히 함수를 작성할수있음
    
    ```tsx
    const Hello = (name = "world") => {
    	return `Hello, ${name}`
    }
    ```
    

- 필수적,  선택적 인자를 같이 사용
    - 필수적 인자가 선택적 인자보다 앞에 와야함
    
    ```tsx
    /* 올바른 예시 */
    
    const hello = (name: string, age?: number): string => {
    	if (age !== undefined){
    		return `hello, ${name}, you are ${age}`
    	} else {
    		return `hello, ${name}`
    	}
    }
    
    console.log(hello("hana", 20))    //hello hana, you are 20
    console.log(hello("hana"))        // hello, hana
    ```
    
    ```tsx
    /* 잘못된 예시 */
    
    const hello = (age?: number ,name: string): string => {
    	if (age !=== undefined){
    		return `hello, ${name}, you are ${age}`
    	} else {
    		return `hello, ${name}`
    	}
    }   // eroor! 
    
    -------------------------------------------------------------------------------
    
    /* 수정된 예시 */
    
    const hello = (age: number | undefined ,name: string): string => {
    	if (age !=== undefined){
    		return `hello, ${name}, you are ${age}`
    	} else {
    		return `hello, ${name}`
    	}
    }  
    
    console.log(hello(20, "hana"))           // hello hana, you are 20
    console.log(hello(undefined, "hana"))    // hello hana
    ```
    

- 인자가 개수가 여러개인 함수인 경우
    - …(인자) 는 자동적으로 배열로 받아옴.
    - 따라서 Type 선언도 배열방식으로 해야한다.
    
    ```tsx
    const add = (...nums : number[]) => {
    	return nums.reduce((result, num) => result + num, 0);
    }
    
    add(1,2,3) //6
    add(1,2,3,4,5,6,7,8,9,10) // 55
    ```
    

- Overload
    - 함수의 type을 여러개로 지정해줘야하는 경우에는 overload를 사용할수있다.
    
    ```tsx
    
    interface User {
    	name: string;
    	age: number;
    }
    
    function join(name: string, age: number | string): User | string {
    	if (typeOf age === "number") {
    		return {name, age}
    	} else {
    		return "나이는 숫자로 입력해주세요"
    	}
    }
    
    const sam: User = join("hana", 20) //error
    
    ```
    
    ```tsx
    /*overload 사용시*/
    
    function join(name: string, age: number): User;
    function join(name: string, age: string): string;
    function join(name: string, age: number | string): User | string {
    	if (typeOf age === "number") {
    		return {name, age}
    	} else {
    		return "나이는 숫자로 입력해주세요"
    	}
    }
    
    const sam: User = join("hana", 20)
    console.log(sam) //hana, 20
    
    ```