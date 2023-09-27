---
tags:
  - FrontEnd
  - store
---
# Redux 기본

- 바닐라 자바스크립트 + redux 사용하기
    - redux 규칙
        - createStore 안에 reducer 이 들어가야한다.
        - reducer 은 무조건 함수 형태여야 한다.
            - reducer 함수는 나의 data (store에 저장될 값) 를 바꾸어준다.
    
    ```jsx
    import { createStore } from "redux"
    
    const reducer = () => {};
    const store = createStore(reducer);
    ```
    

- data initializing 하는 법

```jsx
const countModifier = (state = 0) => {
	return state
}

const countStore = createStore(countModifier);
console.log(countStore.getState()); // 0 
```

- countModifier에 메세지를 보내는법

```jsx
const countModifier = (count = 0, action) => {
  if (action.type === "ADD") {
    console.log("hello");
  } 
  return count;
};

const countStore = createStore(countModifier); 

countStore.dispatch({ type: "ADD" }); // dispatch 를 이용해 action에게 메시지를 보낸다.

//그러면 받은 action을 가지고, 체크해보면 된다. 그리고 그 조건에 맞는 함수 실행하기!!  
```

- subscribe 기능 활용하여 counter 완성하기
    - data.subscribe 는 변화를 감지하여 변화가 일어날때마다 안의 함수를 실행해줌.

```jsx
const countStore = createStore(countModifier);

const onChange = () => {
  number.innerText = countStore.getState();
};
countStore.subscribe(onChange);

add.addEventListener("click", () => countStore.dispatch({ type: "ADD" }));
minus.addEventListener("click", () => countStore.dispatch({ type: "MINUS" }));
```

- switch 를 통해 코드 개선하기

```jsx
const countModifier = (count = 0, action) => {
  switch (action.type) {
    case "ADD":
      return count + 1;
    case "MINUS":
      return count - 1;
    default:
      return count;
  }
};
```

switch를 사용하면 if 문을 굳이 안쓰고도 case를 사용하여서 여러 조건의 실행문을 실행시킬수있다. 

- type 이 string 일때 개선점

—> 만약 action에 “ADD”같이 문자열을 그대로 넣으면 오타실수로 오류가 떠도 어디서 오류가 났는지 알수 없다. 그래서 먼저 ADD = “ADD” 와 같이 정의해준다음에 사용하는것이 좋다. 

```jsx
const ADD = "ADD";
const MINUS = "MINUS";

add.addEventListener("click", () => countStore.dispatch({ type: ADD }));
minus.addEventListener("click", () => countStore.dispatch({ type: MINUS }));
```

- store.getState( )  —> 현재 state의 값을 알려준다.

```jsx
const store = createStore("hello")
store.getState(); // hello
```

- 저장된 store을 index.js에서 배포하기
    - store.js 에서 store을 reducer을 통해 수정하고 관리
    - index.js에서 provider 컴포넌트에 props로 한번 전달하면 그 아래 전 하위 컴포넌트 사용가능함.

```jsx
//index.js

import store from "./store"

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
); // store을 app 컴포넌트와 그 하위 컴포넌트내에서 사용할수있음.
```

- mapStateToProps —> state를 대려오는 방법
    - mapStateToProps를 이용해 현재 state를 가져올수있고 Home의 props로 보내준다.
    - * 이때 함수 이름은 “mapStateToProps” 여야 한다. (공식문서)

```jsx
function mapStateToProps(state, ownProps) {
	return {state};
}

export default connect(mapStateToProps)(Home); 
//Home의 props에 state를 추가함.
```


# Redux toolkit

- 리덕스 툴킷 설치

```jsx
npm install @reduxjs/tookit
```

### createAction

```jsx
const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

console.log(addToDo(), deleteToDO()) 
// {type:ADD, payload: }, {type:DELETE, payload: }

console.log(addToDo.type, deleteToDo.type)
// ADD,DELETE
```

- 우리가 action을 정의하지않아도 된다는 장점이 있음! (매우 간편)
- action 은 무조건 type과 payload 를 가지고있다. (관행임으로 통일해야함)

### createReducer

```jsx
const reducer = createReducer([], {
  [addToDo]: (state, action) => {
    state.push({ text: action.payload, id: Date.now() });
  },
  [deleteToDo]: (state, action) => {
    state.filter((toDo) => toDo !== action.payload);
  },
});
```

- switch, case를 사용할 필요가 없다. + state의 mutate 가능함.
- (공식문서에 따르면) state argument 를 mutate 하거나 new state를 리턴해야함..!
    - why 이게 가능해??? (특히나 mutate!) 
    —> redux 툴킷과 immer 가 가능해주게함.

### configureStore

```jsx
const store = configureStore({ reducer });
```

- configureStore( ) 을 사용하면 Redux Developer Tools를 사용할수있음.

### createSlice

```jsx
const toDos = createSlice({
  name: "toDoReducer",
  initialState: [],
  reducers: {
    add: (state, action) => {
      state.push({ text: action.payload, id: Date.now() });
    },
    remove: (state, action) => {
      state.filter((toDo) => toDo !== action.payload);
    },
  },
});

const store = configureStore({ reducer: toDos.reducer });

export const { add, remove } = toDos.actions;
```

- 저 위에 모든 과정을 함축해내는 놀라운 기능을함.
- 아래 정의된 add를 dispatch(add) 이렇게 보내서 사용할수있음.
    - 왜냐면 state와 action을 두개 다 담고 있기 때문!