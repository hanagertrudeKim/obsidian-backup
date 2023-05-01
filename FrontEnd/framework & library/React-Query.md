# React-Query

---

### FE-상태관리에 대해

- 관리하는 상태가 많아짐 (react 경우만 해도, uiux 를 고려하기 때문에 상태가 늘어나는 환경)

---

- 상태 관리란?
    - 상태를 관리하는 방법에 관한 것 → 프로덕트가 커짐에 따라 어려움도 커짐
    - 상태들은 시간에 따라 변함
    - react에서는 단방향 바인딩이므로 props drilling 같은 이슈 발생 → [[Redux]] 와 MobX 와 같은 라이브러리를 활용해 해결하기도 함.
    
- 기존의 redux의 문제
    - api 코드가 모두 store에 들어감
    - 반복되는 isFetching, isError 등 api 상태 관리
    - 비슷한 구조의 api 통신 코드
    

---

### **React Query의 도입**

 

![Untitled](Untitled.png)

- **Queries**
    - 보통 get으로 받아올 대부분의 api에 사용할 아이
    - 데이터 fetching 용?
    
    ```jsx
    import { useQuery } from 'react-query'
    
    funcion App() {
    	const info = useQuery('todos', fetchTodoList)
    } 
    ```
    
    - **Query Key**
        - key, value 맵핑구조
        - 말그대로 query caching의 query key 값
        - string, array 형태가 있음
    - **Query Function**
        - promise를 반환하는 함수 → 데이터 resolve 하거나 error를 throw 함.
        - fetch, axios 등등 생각하면됨.
    
    - useQuery 가 반환하는것.
        - 대표적으로,
            - data → resolved 데이터
            - error → error 발생시 반환하는 데이터
            - isFetching → Request가 in-flight 중일 때 true
            - status, isLoading, isSuccess 등등 → 현재 query의 상태
            - refetch → 해당 query refetch 하는 함수 제공
                - 약간 재검색 같은거 할때, refetch사용함.
            - remove → 해당 query cache에서 지우는 함수 제공.
    
    - **useQuery option**
        
        ```jsx
        import { useQuery } from 'react-query'
        
        funcion App() {
        	const info = useQuery('todos', fetchTodoList, option)
        } 
        // useQuery Option추가
        ```
        
        - 대표적으로,
            - onSuccess, onError, onSettled
            - enabled → 자동으로 query 실행시킬지 말지 여부
            - retry → query 동작 실패 시, 자동으로 retry 할지 결정하는 옵션
            - select → 성공 시 가져온 data를 가공해서 전달
            - keepPreviousData → 새롭게 fetching 시 이전 데이터 유지 여부
            - refetchInterval → 주기적으로 refetch 할지 결정하는 옵션
    
    ---
    
    ### 💡tip
    
    - quries 파일 분리 추천
    
    ![Untitled](Untitled%201.png)
    
    - query 가 여러개일땐?
        - 알아서 잘됨.
        
         
        

---

- mutation

    데이터 업데이트 시 사용되는 아이