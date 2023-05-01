# http 구조


참고: [https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP)

[https://novlog.tistory.com/245](https://novlog.tistory.com/245)

[https://hanamon.kr/네트워크-http-메세지-message-요청과-응답-구조/](https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-http-%EB%A9%94%EC%84%B8%EC%A7%80-message-%EC%9A%94%EC%B2%AD%EA%B3%BC-%EC%9D%91%EB%8B%B5-%EA%B5%AC%EC%A1%B0/)

> **http (**hypertext transfer protocol**)** 란 ? 
→ 하이퍼텍스트 (html) 문서를 교환하기 위한 프로토콜
= 웹상에서 서버끼리 통신을 할때 어떤 형식으로 서로 통신하자고 하는 통신 형식 or 통신 구조
> 
---

## http 유형

- 요청 (Requests)
- 응답 (Responses)

---

## http 구조

### start line

- 요청이나 응답의 상태를 나타낸다
- 항상 첫번쨰 줄
- 응답에서는 status line 이라고 부름

### http headers

- 요청을 지정, 메시지에 포함된 본문을 설명하는 헤더의 집합

### body

- 요청과 응답에 관련된 데이터나 문서를 포함
- 요청과 응답의 유형에 따라 선택적으로 사용함 (ex.  GET → body x)

* empty line : 헤더와 본문을 구별하는 빈 줄

**<font color="#1f497d"> head = start line + http headers  / payload = body</font>

---

## http Request구조

- Start line
    1. http method
        - http method를 나타냄
        - 수행할 작업 (GET, PUT, POST 등 )이나 방식(HEAD, OPTIONS)등을 설명함
    2. URL or 절대경로
        - 요청 대상(url) 또는 프로토콜, 포트, 도메인의 절대 경로는 요청 컨텍스트에 작성
            - origin 형식
                - ?와 쿼리 문자열 붙는 절대 경로
                - post, get, put, options 등의 method와 함께 사용
                - 예) **`POST / HTTP 1.1GET /background.png HTTP/1.0HEAD /test.html?query=alibaba HTTP/1.1OPTIONS /anypage.html HTTP/1.0`**
            - absoulte 형식
                - 완전한 url 형식으로, get method와 함께 사용
                - 예) **`GET <http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages> HTTP/1.1`**
            - authority 형식
                - 도메인 이름과 포트 번호로 이루어진 url의 authority component
                - http 터널 구축하는 경우, connet와 함께 사용할수있음
                - 예) **`CONNECT developer.mozilla.org:80 HTTP/1.1`**
            - asterisk 형식
                - options와 함께 별표(*) 하나로 서버 전체를 표현
                - 예) **`OPTIONS * HTTP/1.1`**
    3. http 버전
        - http 버전은 메시지의 다른 구조를 결정
- Headers
    - 요청의 Header는 기본구조를 따름
    - key : value로 이루어짐

        - Request headers
            - 요청을 보다 구체화 함
            - 컨텍스트를 제공하거나 조건에 따라 제약 추가
        - General headers
            - 메시지 전체에 적용됨
        - Entity headers
            - content-length은 boby에 적용
            - body가 비어있으면 entity headers는 전송되지 않는다.
- Body
    - 요청의 본문은 http 구조의 마지막에 위치한다.
    - 모든 요청에 body 필요하지 x
        - get, head, delete, options 처럼 서버에 리소스 요청하는 경우는 본문이 필요하지 x
    - post, put 같은 요청은 데이터를 업데이트하기 위해 사용

---

## http Response구조

- Status line ( 응답의 첫줄 )
    - 현재 프로토콜의 버전 (HTTP/1.1)
    - 상태 코드( 401, 404, 등등)
    - 상태 텍스트 (상태 코드에 대한 설명)
    - 예) **`HTTP/1.1 404 Not Found.`**
- Headers
    - Request header와 동일한 구조 (key: value)
        
        - General headers
            - 메시지 전체제 적용
        - Response headers
            - 상태 줄에 넣기 어려운 추가 정보들 제공
        - Entity headers
            - content-length 같은 헤더는 body에 적용
            - body가 비어있으면 entity headers는 전송X
        
- Body
    - 모든 응답에 body 필요하지X ( 201, 204 같은 상태 코드는 본문이 필요하지 X)
    - 응답의 body
        - Single-resource bodies
            - 길이가 알려진 리소스 본문 → 두개의 헤더로 정의(content-type, content-length)
            - 길이를 모르는 리소스 본문 → 파일이 chunk로 나뉘어 인코딩 되어있다.
        - Multiple-resource bodies
            - 다른 정보를 담고있는 body

---

## http stateless(무상태성)

- 말그대로 상태를 가지지 않음
- http 가 클라이언트나 서버의 상태를 확인 X
- 클라이언트에서 발생한 모든 상태를 추적 X
    
    ⇒ 필요에 따라서 다른 방법(쿠키-세션, API 등 ) 으로 상태 확인
    
    - ex) 쇼필몰에서 카트에 담기 버튼을 눌렀을때 카트에 담긴 상품 정보(=상태)를 저장해둬야함 → http는 통신규약이고 상태저장 x → so, 쿠키 or 세션을 이용해 상태를 확인함.