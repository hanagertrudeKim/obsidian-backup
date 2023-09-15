# json

- Javascript Object Notation
- 경량의 data 교환 형식
- 데이터의 용량이 작아서 데이터 전송에 많이 사용한다.
- javascript의 객체 형식에서 대려와서 형태가 매우 유사함
- server와 client 사이에서 많이 이용된다. 
```json
{
  "apple" : {
          "name" : "김사과", "password" : "1111"  
            }
  }
```



## json의 형식을 javascript object로 변환하기

```javascript
JSON.parse()
// JSON 형식의 텍스트를 자바스크립트 객체로 변환

JSON.stringify()
// 자바스크립트 객체를 JSON의 형식으로 변환
```

