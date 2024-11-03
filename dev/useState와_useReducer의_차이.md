<!-- 제목 -->

## useState와 useReducer의 선택 기준

<br/>

<!-- 해당 질문을 가져온 스터디원 -->
<img src ="https://img.shields.io/badge/👼 천산-293241.svg?&style=for-the-badge"/>

<br/>

React에서 컴포넌트의 상태를 관리할 때 `useState`와 `useReducer` 훅을 선택하는 기준은 상태의 복잡성과 업데이트 로직의 필요성에 따라 달라집니다.

1. **useState**

   - **설명** :가장 기본적인 상태 관리 훅으로, 상태값과 그 값을 업데이트하는 함수를 제공
   - **적합 상황**: 단순한 상태 관리(예: 토글, 숫자 증가/감소 등)에 적합.
   - **장점**: 상태가 자주 업데이트되거나 구조가 단순할 때 사용하기 용이하며, 빠르고 쉽게 구현 가능.

2. **useReducer**
   - **설명**: 상태 업데이트 로직이 복잡하거나, 여러 액션에 따라 상태가 다양한 방식으로 변해야 할 때 유용. **상태 변화 로직을 분리해 리듀서 함수로 정의**하고, 이를 통해 상태 관리를 명확하게 할 수 있음.
     리덕스에서 이용되고 있는 훅 !
   - **적합 상황**: 복잡한 상태 로직이나 다양한 액션(예: 추가, 삭제, 수정 등)이 필요한 경우에 유리.
   - **장점**: 코드 구조가 명확해지며, 상태 변화가 복잡한 컴포넌트에서 유리.

<br/>

<details>
  <summary><b>useReducer를 활용한 todo list 예제</b></summary>

```js
import React, { useReducer } from "react";

// 초기 상태
const initialState = [];

// 리듀서 함수
function todoReducer(state, action) {
  switch (action.type) {
    case "ADD":
      return [...state, { id: Date.now(), text: action.payload }];
    case "REMOVE":
      return state.filter((todo) => todo.id !== action.payload);
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}

function TodoList() {
  const [state, dispatch] = useReducer(todoReducer, initialState);
  const [newTodo, setNewTodo] = useState("");

  const handleAddTodo = () => {
    dispatch({ type: "ADD", payload: newTodo });
    setNewTodo("");
  };

  return (
    <div>
      <input value={newTodo} onChange={(e) => setNewTodo(e.target.value)} />
      <button onClick={handleAddTodo}>Add</button>
      <ul>
        {state.map((todo) => (
          <li key={todo.id}>
            {todo.text}
            <button
              onClick={() => dispatch({ type: "REMOVE", payload: todo.id })}>
              Remove
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

</details>

<br/>

<br/>

<br/>

> **참고 아티클**: [useState vs useReducer 비교](https://www.philly.im/blog/use-state-vs-use-reducer)

만약 state 변경이 독릭접으로 이뤄진다면 - 여러 useState로 분리하세요.

동시에 변경되거나 한 번에 하나의 필드만 변경(ex. 폼) 된다면 - 하나의 useState와 객체를 사용하세요.

사용자의 인터렉션이 state의 다른 부분을 변경한다면 (state가 서로 의존한다면) - useReducer를 사용하세요.

<br/>

<br/>

<br/>

> **답변**:  
> **useState**는 리액트에서 상태를 관리하는 기본적인 훅으로, 상태값과 그 값을 업데이트할 수 있는 set함수를 제공해줍니다. 상태가 토글이나 카운터와 같이 단순할 때 적합합니다.  
> 반면, **useReducer**는 상태 변화가 복잡한 경우에 유리합니다. 예를 들어, **상태 자체가 복잡해지거나**이나 CRUD 같이 **액션별 상태 분기**가 필요한 상황 등에 조금 더 적합할 수 있습니다. useReducer 함수는 리듀서 함수로 상태 변화 로직을 완전 분리해놓기 때문에 복잡한 로직을 다룰 때 더 명확하고 유지보수가 좋게 작성이 가능합니다. 하지만 필요하지 않을 때 이 훅을 사용하면 오히려 코드가 필요이상으로 복잡해지고 무거워질 수 있기 때문에 상황에 맞게 사용해야합니다.
