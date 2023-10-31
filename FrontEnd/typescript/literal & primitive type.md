---
tags:
  - FrontEnd
  - typeScript
---

# literal type

 - type 이 고정적으로 정해짐
        - const → 재할당 불가 , type 이 바뀌지 않음
        - let → 재할당 가능, type 이 바뀔수있음
            - 다른 type을 재할당할때는 type선언을 추가적으로 해줘야한다.
    
    ```tsx
     /* Literal type 예시 */ 
    
    const user1 = "Bob"; // Literal type ->  const로 선언하는순간, type이 고정됨.
    let user2 = "hana";  // Literal type X  --> 자동적으로 type : string 으로 인식 
    
    user2 = 13 // error..
    ```
    
    ```tsx
    type Job = "police" | "doctor" | "guard"
    
    interface User {
    	name: string;
    	job: Job;
    }
    
    const user: User = {
    	name: "hana";
    	job: "student"; // type error 
    } 
    ```


#  primitive type

