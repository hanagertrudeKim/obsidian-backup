---
tags:
  - FrontEnd
  - typeScript
---
# Class-typescript

- Class
    - ES6 에서의 typescript 적용
    
    ```tsx
    class Car {
    	color: string;   //color의 type을 지정해야 error 가 발생하지 않음.
    	constructor(color: string) {
    		this.color = color;
    	}
    	start(){
    		colsole.log("hello")
    	}
    }
    ```
    
- 접근 제한자 ( access modifier )
    - public → 자식클래스, 클래스 인스턴스 사용 가능
    - private → 해당 클래스 내부에서만 접근 가능
    - protected  → 자식클래스에서 접근 가능
    
    ```tsx
    // 접근제한자(Access modifier) - public
    
    class Car {
    	public name: string = "car"; // public 시, 자식요소에 type이 상속됨 
    	color: string;   //color의 type을 지정해야 error 가 발생하지 않음.
    	constructor(color: string) {
    		this.color = color;
    	}
    	start(){
    		console.log("hello")
    		console.log(this.name) //car
    	}
    }
    
    class Bmw extends Car {
    	constructor(color: string) {
    		super(color)
    	}
    	showName(){
    		console.log(super);
    	}
    }
    
    const myCar = new Bmw("black")
    ```
    
    ```tsx
    // 접근제한자(Access modifier) - private
    
    class Car {
    	private name: string = "car"; // private 시, 자식요소에 type이 상속되지 않음
     // #name: string = "car";   // private을 #으로 대체할수있음 --> this.#name으로 써야함
    	color: string;   //color의 type을 지정해야 error 가 발생하지 않음.
    	constructor(color: string) {
    		this.color = color;
    	}
    	start(){
    		console.log("hello")
    		console.log(this.name) // or this.#name 
    	}
    }
    
    class Bmw extends Car {
    	constructor(color: string) {
    		super(color)
    	}
    	showName(){
    		console.log(super);
    	}
    }
    
    const myCar = new Bmw("black")
    ```
    
    ```tsx
    // 접근제한자(Access modifier) - protected
    
    class Car {
    	protected name: string = "car"; // protected 시, 자식요소를 제외한 class instance에서 주관할수없음.
    	color: string;  
    	constructor(color: string) {
    		this.color = color;
    	}
    	start(){
    		console.log("hello")
    		console.log(this.name)
    	}
    }
    
    class Bmw extends Car {
    	constructor(color: string) {
    		super(color)
    	}
    	showName(){
    		console.log(super);
    	}
    }
    
    const myCar = new Bmw("black")
    console.log(super.name) // error
    ```
    
- static
    - 정적 메소드
    - 접근하려고 할때 class명을 통해 접근해야한다.
    
    ```tsx
    class Car {
    	name: string = "car";
    	color: string;  
    	static wheels = 4;
    	constructor(color: string, name) {
    		this.color = color;
    		this.name = name
    	}
    	start(){
    		console.log("hello")
    		console.log(this.name)
    		console.log(Car.wheels) // 4
    	}
    }
    
    class Bmw extends Car {
    	constructor(color: string) {
    		super(color)
    	}
    	showName(){
    		console.log(super);
    	}
    }
    
    const myCar = new Bmw("black")
    console.log(super.name)
    ```
    
- 추상 클래스
    - 상속을 통해서만 class 사용이 가능
    - 추상클래스 안의 추상클래스
        - 추상클래스가 기본 틀을 잡아주고, extends를 통해 여러가지 기능을 구현할수있음
        - = 세부적인 내용만 다르게하고싶다면 abstract를 사용해야한다.
    
    ```tsx
    abstract class Car {
    	color: string;  
    	constructor(color: string, name) {
    		this.color = color
    	}
    	start(){
    		console.log("hello")
    	}
    	abstract doSomething(): void;  //추상클래스 안의 추상클래스 
    }
    
    class Bmw extends Car {
    	constructor(color: string) {
    		super(color)
    	}
    	showName(){
    		console.log(super);
    	}
    	doSomething(){
    		console.log(3);
    	}
    } // 상속을 통해서만 사용할수있다. 
    
    //const myCar = new Bmw("black") -> new 를 통해 객체 생성 불가함
    ```