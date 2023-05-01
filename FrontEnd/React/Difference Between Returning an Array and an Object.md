#  Difference Between Returning an Array and an Object


```ad-quote
This small difference makes a big change in usability.
```

---

## { } 와 [ ] 의 차이

### hook 이 배열 [ ]을 반환하는 경우
- 변수 이름을 직접 지정하고 사용
-  hook의 여러 instance를 사용하려는 경우 배열을 반환하여 사용하는게 나음. 

### hook이 객체 { }를 반환하는 경우
- hook이 반환된 것과 동일한 변수 이름을 사용
- hook의 instance가 1개(또는 일부)만 있는 경우 객체 반환을 사용하는게 나음

ex) 
``` javascript
const {myVar: newVarName} = useExample(); //객체 반환  
const [newVarName] = useExample(); //배열 반환
```

