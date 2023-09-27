---
tags:
  - javascript
  - FrontEnd
  - typeScript
---
# Class

- 클래스란?
    - *클래스는 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀로, 객체를 정의하기 위한 상태(멤버 변수)와 메서드(함수)로 구성된다.*
    
    ```jsx
    class User{
        constructor(name){
            this.name = name;
        }
    
        Sayhi(){
            alert(this.name)
        }  //class 안에서 실행되는 함수를 메소드라고 한다. 
    }
    
    let user1 = new User("hana");
    user1.Sayhi() // hana
    ```
    
    - new User을 호출하면 새로운 객체가 생성된다.
    - 객체가 constructor 로 넘어간다. —> name의 인수가 this.name으로 할당됨.
    - user.Sayhi( ) 가 실행됨


# static

- static
    - class의 정적 메서드
    
    ```tsx
    static methodName() { ... }
    ```
    
- class 쓰는 이유
    - 비슷한 object 를 많이 만들어야할때 쓰면 좋다. (object 뽑는 기계?역할)