# React Hook

## react hook의 역사
- 훅의 역사는 reccmpose에서 시작됨.
    - recompose 라이브러리(지금의 함수형 컴포넌트, state와 흡사) 는 리액트 팀에 의해 인수됨.


## useInput
- useInput
    - input 안에 많은 실행조건 넣는 방법
        - 예를들어 input 의 태그 안에 입력값을 띄어주고, 입력 기본값을 지정해주고, 입력값의 길이를 제한해주는 등의 기능들을 넣고 싶다고 하자. 
        —> 1. 한 변수 안에 useInput을 정의해준다.
        
        ```jsx
        ex)
        const name = useInput("mr.", maxvalue);
        ```
        
        - 만약 useInput 을 내가 직접 만들때, 
        useInput의 받은 인자로 무엇을 실핼시킬수있는지 고려해야한다.
        - 위의 예시로는 첫번째로 받은 인자 "mr. " 로 기본값을 지정할것이고, 두번째로 받는 인자로는 value값을 검사해주는 기능을 하는 함수를 넣을것이다. 
        이와같은 설계를 따라 useInput hook을 만들어보자면
        
        ```jsx
        
        const useInput = (initailValue, validator) => {
          const [value, setvalue] = useState(initailValue);
          const onChange = (e) => {
            const {
              target: { value }
            } = e;
            setvalue(value);
          };
          let willUpdate = true;
          if (typeof validator === "function") {
            willUpdate = validator(value);
          }
          if (willUpdate) {
            setvalue(value);
          }
          return { value, onChange };
        };
        ```
        
        위와 같이 만들수 있다. 
        ** 주의점 - 함수에서 인자로 함수를 넘겨줄때는 `return {onChange}` 같이 함수명만 쓴다.

## useTabs
- useTabs
    - 버튼 2개를 만들어서 각 버튼마다 다른 내용이 뜨게 만들고싶은 컨셉
        
        ```jsx
        import { useState } from "react";
        import "./styles.css";
        
        const content = [
          {
            tab: "section 1",
            content: "I'm the content of the section 1"
          },
          {
            tab: "section 2",
            content: "I'm the content of the section 2"
          }
        ];
        
        const useTabs = (initialTab, allTabs) => {
          if (!allTabs || !Array.isArray(allTabs)) {
            return;
          }
          const [currentIndex, setCurrentIndex] = useState(initialTab);
          return {
            currentItem: allTabs[currentIndex],
            changeItem: setCurrentIndex
          };
        };
        
        const App = () => {
          const { currentItem, changeItem } = useTabs(0, content);
          return (
            <div className="App">
              {content.map((section, index) => (
                <button onClick={() => changeItem(index)}>{section.tab}</button>
              ))}
              <div>{currentItem.content}</div>
            </div>
          );
        };
        
        export default App;
        ```
        
        - const idx = useTabs ( );
            - idx.currentItem → allTabs[currentIndex]
            - idx.changeItem → setCurrentIndex


## useEffect
- useEffect
    
    ```jsx
    useEffect(sayHello, []);   // 처음 렌더링때만 sayHello 가 실행된다. 
    useEffect(sayHello)  // 모든 렌더링때 실행된다 .
    useEffect(sayHello, [name]) //name이라는 변수가 변경될시에 sayHello가 실행된다. 
    ```


## useTitle

- usetitle

```jsx
import { useEffect, useState } from "react";
import "./styles.css";

const useTitle = (initialTitle) => {
  const [title, setTitle] = useState(initialTitle);
  const updateTitle = () => {
    const htmlTitle = document.querySelector("title");
    htmlTitle.innerText = title;
  };
  useEffect(updateTitle, [title]);
  return setTitle;
};

const App = () => {
  const titleUpdater = useTitle("Loading....");
  setTimeout(() => titleUpdater("Home"));
  return (
    <div className="App">
      <div>Hi</div>
    </div>
  );
};

export default App;
```

—> 쿼리셀렉터로 title을 선택후 innerText를 이용해 글자를 새로 넣어준다. 그리고 나서 settimeout 을 통해 home이라고 title을 변경해준다. 
* html 을 조작해야할때 title 이외에도 활용할수있을거같음


## useClick


- useClick

```jsx
import "./styles.css";

useConfirm = (massage, callback, rejection) => {
  if (!callback || typeof callback !== "function") {
    return;
  }
  if (!rejection || typeof rejection !== "function") {
    return;
  }
  const confirmAction = () => {
    if (confirm(massage)) {
      callback();
    } else {
      rejection();
    }
  };
  return confirmAction;
};

export default function App() {
  const deleteWorld = () => console.log("Deleting the world...");
  const reject = () => console.log("rejected....");
  const confirmDelete = useConfirm("Are you sure delete?", deleteWorld, reject);
  return (
    <div className="App">
      <button onClick={confirmDelete}>Delete the word</button>
    </div>
  );
}
```

---

—>창을 끄기 전에  알림창으로 확인메시지를 보내주는 기능을 한다. 
**confirm( massage )
- massage = 경고 대화상자에 표시될 문자열 

-반환값은 true 아니면 false (기본값은 false)

-예시

```jsx
if (window.confirm("Do you really want to leave?")) {
  window.open("exit.html", "Thanks for Visiting!");
}
```



## useLeave

- useLeave

```jsx
import "./styles.css";

const usePreventLeave = () => {
  const listener = (event) => {
    event.preventDefault();
    event.returnValue = " ";
  };
  const enablePrevent = () => {
    window.addEventListener("beforeunload", listener);
  };
  const disablePrevent = () => {
    window.addEventListener("beforeunload", listener);
  };
  return enablePrevent, disablePrevent;
};

export default function App() {
  const { enablePrevent, disablePrevent } = usePreventLeave();
  return (
    <div className="App">
      <button onClick={enablePrevent}>protect</button>
      <button onClick={disablePrevent}>unprotect</button>
    </div>
  );
}
```

—>**Window: beforeunload 이벤트**

사용자가 떠나기 전 진짜 떠날것인지 확인 대화상자를 띄움. 
확인 대화 상자를 표시하려면 이벤트의 `[preventDefault()](https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault)`를 호출해아함. 
예시 )

```jsx
window.addEventListener('beforeunload', (event) => {
  // 표준에 따라 기본 동작 방지
  event.preventDefault();
  // Chrome에서는 returnValue 설정이 필요함
  event.returnValue = '';
});

```



## useBeforeLeave

- useBeforeLeave

```jsx
import { useEffect } from "react";
import "./styles.css";

const useBeforeLeave = (onBefore) => {
  if (typeof onBefore !== "function") {
    return;
  }
  const handle = (event) => {
    if (event.clientY <= 10) {
      onBefore();
    }
  };
  useEffect(() => {
    document.addEventListener("mouseleave", handle);
    return () => document.removeEventListener("mouseleave", handle);
  }, []);
};

export default function App() {
  const begForLife = () => console.log("plaese Don't leave");
  useBeforeLeave(begForLife);
  return (
    <div className="App">
      <h1>hello</h1>
    </div>
  );

```

—> 마우스가 화면 밖을 떠나기 직전에 콘솔창에 알림을 뜨게해준다.
—> 마우스가 특정 구역을 벗어나면 특정 행동이 실행되도록해서 활용할수있을듯..?


## useFadin


- useFadeIn

```jsx
import { useEffect, useRef } from "react";
import "./styles.css";

const useFadeIn = (duration = 1, delay = 0) => {
  if ((typeof duration !== "number", typeof delay !== "number")) {
    return;
  }
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      const { current } = element;
      current.style.transition = `opacity ${duration}s ease-in-out ${delay}s`;
      current.style.opacity = 1;
    }
  });
  return { ref: element, style: { opacity: 0 } };
};

export default function App() {
  const fadeInH1 = useFadeIn(0, 2);
  const fadeInP = useFadeIn(3, 4);
  return (
    <div className="App">
      <h1 {...fadeInH1}>hello</h1>
      <p {...fadeInP}>what is your name?</p>
    </div>
  );
}
```

---

—> 특정 글자를 서서히 뜨게하는 fade in 효과를 줄수있다. 코드 구조는 생각보다 간단하지만 style을 리액트에서 이용하는것과 animation을 hook과 결합하여 사용할수있다. 이태까지 만든 hook 중에 가장 재미있게 만든거같다.

—> 정말 다방면으로 활용할수잇을거같다. 

*** CSS transition : css 속성을 변경할때 속도를 조절할수있는 방법


## useScroll

- useScroll

```jsx
import { useEffect, useState } from "react";
import "./styles.css";

const useScroll = () => {
  const [state, setState] = useState({
    x: 0,
    y: 0,
  });
  const onScroll = () => {
    setState({ y: window.scrollY, x: window.scrollX });
  };
  useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, []);
  return state;
};

export default function App() {
  const { y } = useScroll();
  return (
    <div className="App" style={{ height: "1000vh" }}>
      <h1 style={{ position: "fixed", color: y > 100 ? "red" : "blue" }}>hi</h1>
    </div>
  );
}
```

- 스크롤의 y 좌표가 100이 넘어가면 색깔이 바뀌는 식의 코드 구성
- 스크롤할때마다 특정 이벤트를 하는 경우가 있을까..? 생각을 좀 해봐야할듯하다.


## useFullScreen

- useFullScreen

```jsx
import { useRef } from "react";
import "./styles.css";

useFullscreen = () => {
  const element = useRef();
  const triggerFull = () => {
    if (element.current) {
      element.current.requestFullscreen();
    }
  };
  const exitFull = () => {
    const checkFullScreen = document.fullscreenElement;
    if (checkFullScreen !== null) {
      document.exitFullscreen();
    }
  };
  return { element, triggerFull, exitFull };
};

function App() {
  const { element, triggerFull, exitFull } = useFullscreen();
  return (
    <div className="App" style={{ height: "1000vh" }}>
      <img ref={element} src="https://i.ibb.co/R6RwNxx/grape.jpg" />
      <button onClick={triggerFull}>Make fullscreen</button>
      <button onClick={exitFull}>exit fullscreen</button>
    </div>
  );
}

export default App;
```

- 전체화면을 만들고 나가기
- 전체화면을 만들때에는 element. current로 하지만 전체화면을 종료할때는 window 에서 작업해야한다.