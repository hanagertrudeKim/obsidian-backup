---
tags:
  - react
  - typeScript
---
# Generic

- Generic
    - type을 generic 하게 선언해줌
    
    ```tsx
    function getSize<T>(arr: T[]): number {
    	return arr.length
    }
    
    const num1 = [1,2,3]
    getSize(num1) // type: number[]
    
    const word1 = ["a", "b", "c"]
    getSize(word1) // type: string[]
    ```
    
- interface 에서의 Generic
    
    ```tsx
    interface Mobile<T> {
    	name: string;
    	price: number;
    	option: T
    }
    
    const m1: Mobile<object> {  // or <{color: string; coupon: boolean}>
    	name: "m1";
    	price: 10000;
    	option: {
    		color: "red";
    		coupon: false;
    	}
    } 
    
    const m2: Mobile<string> {
    	name: "m2";
    	price: 50000;
    	option: "good"
    } 
    ```