<details>
<summary>ENG (English Version)</summary>

# useEffect Hook

## 1. Component Lifecycle

Class components use lifecycle methods depending on the stage of the component.

### Mounting

Executed when the component first appears.

```text
constructor()
    ↓
render()
    ↓
componentDidMount()
```

`componentDidMount()` is commonly used for:

- API requests
- Event registration
- Initialization after the DOM is ready

### Updating

Executed when props or state change.

```text
props / state change
        ↓
      render()
        ↓
componentDidUpdate()
```

### Unmounting

Executed before a component disappears.

```text
componentWillUnmount()
```

It can be used to:

- Clear timers
- Remove events
- Release resources

---

## 2. What is `useEffect`?

`useEffect()` is a React Hook used to handle **side effects** in functional components.

Typical examples include:

- API requests
- Console logging
- Timers
- DOM-related work
- localStorage synchronization
- Resource cleanup

Basic syntax:

```jsx
useEffect(() => {
  // effect code
}, [dependencies]);
```

The effect runs after rendering.

---

## 3. Lifecycle and `useEffect`

`useEffect` can perform roles similar to class lifecycle methods.

```text
Class Component             Functional Component

componentDidMount()   →     useEffect()
componentDidUpdate()  →     useEffect()
componentWillUnmount() →    cleanup function
```

---

## 4. Dependency Array

The dependency array determines when an effect runs.

### No Dependency Array

```jsx
useEffect(() => {
  console.log("Effect executed");
});
```

The effect runs after every render.

```text
Initial Render
    ↓
useEffect
    ↓
State / Props Change
    ↓
Re-render
    ↓
useEffect
```

---

## 5. Empty Dependency Array

An empty dependency array runs the effect when the component is initially mounted.

```jsx
useEffect(() => {
  console.log(
    "Component mounted"
  );
}, []);
```

This pattern can be used when initialization work should run on the first mount.

Example:

```jsx
function FetchData() {
  useEffect(() => {
    console.log(
      "Initial API request"
    );
  }, []);

  return (
    <div>
      Loading data...
    </div>
  );
}
```

---

## 6. Dependency with a Value

When a value is included in the dependency array, the effect runs on the initial render and whenever that value changes.

```jsx
useEffect(() => {
  console.log(
    `userId changed: ${userId}`
  );
}, [userId]);
```

Example:

```jsx
function UserProfile({
  userId,
}) {
  const [
    profile,
    setProfile,
  ] = useState(null);

  useEffect(() => {
    console.log(
      `Load profile for user ${userId}`
    );
  }, [userId]);

  return (
    <div>
      {profile ? (
        <h1>
          {profile.name}
        </h1>
      ) : (
        <h1>
          Loading...
        </h1>
      )}
    </div>
  );
}
```

---

## 7. Dependency Array Summary

```text
useEffect(() => {
  ...
});
```

Runs after every render.

```text
useEffect(() => {
  ...
}, []);
```

Runs when the component is first mounted.

```text
useEffect(() => {
  ...
}, [value]);
```

Runs when `value` changes.

---

## 8. `useEffect` Cleanup

An effect can return a cleanup function.

```jsx
useEffect(() => {
  // effect

  return () => {
    // cleanup
  };
}, []);
```

Cleanup can be used for:

- Clearing timers
- Removing event listeners
- Releasing resources

The cleanup function runs:

- When the component unmounts
- Before the effect runs again after a dependency changes

---

## 9. Timer Cleanup

Example:

```jsx
function Timer() {
  useEffect(() => {
    const timerId =
      setInterval(() => {
        console.log(
          "Timer running"
        );
      }, 1000);

    return () => {
      clearInterval(
        timerId
      );
    };
  }, []);

  return (
    <div>
      Timer is running.
    </div>
  );
}
```

This prevents unnecessary timer execution after the component disappears.

---

## 10. Render and Effect Execution

The component function executes during rendering, while `useEffect` runs after rendering.

```jsx
function Hello() {
  useEffect(() => {
    console.log(
      "1. useEffect"
    );
  }, []);

  console.log(
    "2. Component function"
  );

  return (
    <h1>Hello</h1>
  );
}
```

Conceptually:

```text
Component Function
      ↓
JSX Render
      ↓
useEffect
```

---

## 11. Watching State Changes

`useEffect` can respond only to a specific state value.

```jsx
function CounterWatcher() {
  const [
    count,
    setCount,
  ] = useState(0);

  const [
    cnt,
    setCnt,
  ] = useState(100);

  useEffect(() => {
    console.log(
      `Current count: ${count}`
    );
  }, [count]);

  return (
    <div>
      <p>
        count: {count}
      </p>

      <button
        onClick={() =>
          setCount(
            count + 1
          )
        }
      >
        +1
      </button>

      <p>
        cnt: {cnt}
      </p>

      <button
        onClick={() =>
          setCnt(
            cnt + 100
          )
        }
      >
        +100
      </button>
    </div>
  );
}
```

Only changes to `count` trigger this effect.

---

## 12. Mount and Unmount Practice

Conditional rendering can be used to observe mounting and unmounting.

```jsx
function Message() {
  useEffect(() => {
    console.log(
      "Component mounted"
    );

    return () => {
      console.log(
        "Component unmounted"
      );
    };
  }, []);

  return (
    <h3>
      Message Component
    </h3>
  );
}
```

```jsx
function MessageApp() {
  const [
    show,
    setShow,
  ] = useState(true);

  return (
    <div>
      <button
        onClick={() =>
          setShow(!show)
        }
      >
        {show
          ? "Hide"
          : "Show"}
      </button>

      {show && <Message />}
    </div>
  );
}
```

---

## 13. Todo CRUD with `useState`

The Todo example implements the basic CRUD operations.

```text
Create
Add a todo

Read
Render todos with map()

Update
Toggle completed state

Delete
Remove a todo with filter()
```

State:

```jsx
const [
  todos,
  setTodos,
] = useState([]);

const [
  newTodo,
  setNewTodo,
] = useState("");
```

---

## 14. Create

A new todo can be added to the previous array.

```jsx
const handleCreateTodo =
  () => {
    if (
      newTodo.trim() === ""
    ) {
      return;
    }

    setTodos([
      ...todos,
      {
        id: Date.now(),
        text: newTodo,
        completed: false,
      },
    ]);

    setNewTodo("");
  };
```

---

## 15. Read

Todos are rendered using `map()`.

```jsx
<ul>
  {todos.map(todo => (
    <li key={todo.id}>
      {todo.text}
    </li>
  ))}
</ul>
```

---

## 16. Update

A todo can be updated with `map()`.

```jsx
const handleToggleComplete =
  id => {
    setTodos(
      todos.map(
        todo =>
          todo.id === id
            ? {
                ...todo,
                completed:
                  !todo.completed,
              }
            : todo
      )
    );
  };
```

---

## 17. Delete

A todo can be removed using `filter()`.

```jsx
const handleDeleteTodo =
  id => {
    setTodos(
      todos.filter(
        todo =>
          todo.id !== id
      )
    );
  };
```

---

## 18. Todo Rendering

```jsx
<ul>
  {todos.map(todo => (
    <li
      key={todo.id}
      style={{
        textDecoration:
          todo.completed
            ? "line-through"
            : "none",
      }}
    >
      <span>
        {todo.text}
      </span>

      <button
        onClick={() =>
          handleToggleComplete(
            todo.id
          )
        }
      >
        {todo.completed
          ? "Incomplete"
          : "Complete"}
      </button>

      <button
        onClick={() =>
          handleDeleteTodo(
            todo.id
          )
        }
      >
        Delete
      </button>
    </li>
  ))}
</ul>
```

This combines:

- `useState`
- `map()`
- `filter()`
- Spread syntax
- Event handling
- Conditional styling

---

## 19. localStorage

`localStorage` is a browser storage object that stores data as key-value pairs.

Main characteristics:

- Data can remain after page refresh
- Data is stored by key and value
- Values are stored as strings

Basic methods:

```javascript
localStorage.setItem(
  key,
  value
);

localStorage.getItem(
  key
);

localStorage.removeItem(
  key
);

localStorage.clear();
```

---

## 20. Storing Objects and Arrays

Because localStorage stores strings, objects and arrays can be converted using JSON.

### Store

```javascript
const userProfile = {
  name: "Alice",
  age: 25,
};

localStorage.setItem(
  "userProfile",
  JSON.stringify(
    userProfile
  )
);
```

### Load

```javascript
const userProfileString =
  localStorage.getItem(
    "userProfile"
  );

const userProfile =
  JSON.parse(
    userProfileString
  );
```

---

## 21. Todo Data with localStorage

Initial data can be loaded from localStorage.

```jsx
const initialTodos =
  JSON.parse(
    localStorage.getItem(
      "todos"
    )
  ) || [];
```

Whenever `todos` changes, the array can be stored again.

```jsx
useEffect(() => {
  localStorage.setItem(
    "todos",
    JSON.stringify(todos)
  );
}, [todos]);
```

The flow is:

```text
App Start
   ↓
localStorage
   ↓
Initial State
   ↓
User Changes Todos
   ↓
setTodos()
   ↓
Re-render
   ↓
useEffect([todos])
   ↓
Save to localStorage
```

---

## 22. Persistent Todo List

The Todo application combines `useState`, `useEffect`, and localStorage.

Main features:

- Add todo
- Toggle completion
- Delete todo
- Save data locally
- Restore data after page refresh

This demonstrates how `useEffect` can synchronize React state with an external browser API.

---

## 23. `reduce()`

The branch also reviews `reduce()`.

`reduce()` converts an array into a single result.

Basic syntax:

```javascript
array.reduce(
  (
    accumulator,
    currentValue
  ) => {
    return newValue;
  },
  initialValue
);
```

Example:

```javascript
const numbers = [
  1,
  2,
  3,
  4,
];

const sum =
  numbers.reduce(
    (
      accumulator,
      currentNumber
    ) => {
      return (
        accumulator +
        currentNumber
      );
    },
    0
  );
```

Result:

```javascript
10
```

---

## 24. Shopping Cart Total with `reduce()`

A shopping cart total can be calculated with `reduce()`.

```jsx
const totalPrice =
  cart.reduce(
    (
      total,
      item
    ) =>
      total +
      (
        item.price *
        item.quantity
      ),
    0
  );
```

Example:

```jsx
function ShoppingCartReduceEx() {
  const [
    cart,
    setCart,
  ] = useState([
    {
      id: 1,
      name: "Apple",
      price: 1000,
      quantity: 2,
    },
    {
      id: 2,
      name: "Banana",
      price: 1500,
      quantity: 1,
    },
    {
      id: 3,
      name: "Strawberry",
      price: 2000,
      quantity: 3,
    },
  ]);

  const totalPrice =
    cart.reduce(
      (
        total,
        item
      ) =>
        total +
        item.price *
          item.quantity,
      0
    );

  return (
    <div>
      <h2>
        Shopping Cart
      </h2>

      <ul>
        {cart.map(
          item => (
            <li
              key={item.id}
            >
              {item.name}
              ({item.quantity})
              {" - "}
              {item.price *
                item.quantity}
            </li>
          )
        )}
      </ul>

      <h3>
        Total:
        {totalPrice}
      </h3>
    </div>
  );
}
```

---

## 25. Shopping Cart Practice

The shopping cart exercise combines:

- Product list
- Add to cart
- Remove from cart
- Quantity management
- Total price calculation
- localStorage
- `useState`
- `useEffect`
- `reduce()`

The cart data can be stored in localStorage so that it remains available after a page refresh.

---

## 26. Product Review Practice

The branch also includes a product review example.

Initial product data:

```jsx
const PRODUCT = {
  id: 1,
  name: "Eco Tumbler",
  description:
    "A tumbler for an eco-friendly lifestyle",
  reviews: [],
};
```

State:

```jsx
const [
  product,
  setProduct,
] = useState(PRODUCT);

const [
  newReview,
  setNewReview,
] = useState("");
```

The UI includes:

- Product information
- Review input
- Review registration
- Review list
- Empty review message
- Review CRUD extension

---

## 27. Invitation Practice

The final exercise combines several concepts.

Static information:

- Title
- Date
- Location
- Basic event information

Dynamic features:

- Participant counter
- Join / Cancel toggle
- localStorage persistence
- D-Day timer
- Reusable components
- Props
- `useState`
- `useEffect`

This practice combines state management and side effects in a single application.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# useEffect Hook

## 1. 컴포넌트 생명주기

클래스형 컴포넌트에서는 컴포넌트의 단계에 따라 생명주기 메소드가 실행됩니다.

### Mounting

컴포넌트가 처음 화면에 나타날 때 실행됩니다.

```text
constructor()
    ↓
render()
    ↓
componentDidMount()
```

`componentDidMount()`에서는 다음과 같은 작업을 할 수 있습니다.

- API 호출
- 이벤트 등록
- DOM 준비 이후 초기 작업

### Updating

Props 또는 State가 변경되었을 때 실행됩니다.

```text
Props / State 변경
        ↓
      render()
        ↓
componentDidUpdate()
```

### Unmounting

컴포넌트가 화면에서 사라지기 전에 실행됩니다.

```text
componentWillUnmount()
```

다음과 같은 작업에 사용할 수 있습니다.

- 타이머 해제
- 이벤트 제거
- 리소스 정리

---

## 2. `useEffect`란?

`useEffect()`는 함수형 컴포넌트에서 **Side Effect(부수 효과)**를 처리하기 위한 Hook입니다.

대표적인 사용 예:

- API 호출
- Console 로그
- 타이머
- DOM 관련 작업
- localStorage 동기화
- 리소스 정리

기본 문법:

```jsx
useEffect(() => {
  // 실행할 코드
}, [의존성]);
```

Effect 함수는 렌더링 이후 실행됩니다.

---

## 3. 생명주기와 `useEffect`

함수형 컴포넌트의 `useEffect`는 클래스형 컴포넌트의 생명주기 메소드와 유사한 역할을 수행할 수 있습니다.

```text
클래스형 컴포넌트         함수형 컴포넌트

componentDidMount()   →   useEffect()
componentDidUpdate()  →   useEffect()
componentWillUnmount() →  cleanup 함수
```

---

## 4. 의존성 배열

의존성 배열은 Effect의 실행 시점을 결정합니다.

### 의존성 배열이 없는 경우

```jsx
useEffect(() => {
  console.log(
    "Effect 실행"
  );
});
```

컴포넌트가 렌더링될 때마다 Effect가 실행됩니다.

```text
최초 렌더링
    ↓
useEffect
    ↓
State / Props 변경
    ↓
재렌더링
    ↓
useEffect
```

---

## 5. 빈 의존성 배열

빈 배열을 사용하면 컴포넌트가 처음 마운트될 때 Effect가 실행됩니다.

```jsx
useEffect(() => {
  console.log(
    "컴포넌트 마운트"
  );
}, []);
```

예시:

```jsx
function FetchData() {
  useEffect(() => {
    console.log(
      "최초 API 호출"
    );
  }, []);

  return (
    <div>
      데이터를 로딩 중...
    </div>
  );
}
```

---

## 6. 특정 값을 의존성으로 지정

의존성 배열에 특정 값을 넣으면 최초 렌더링과 해당 값이 변경될 때 Effect가 실행됩니다.

```jsx
useEffect(() => {
  console.log(
    `userId 변경: ${userId}`
  );
}, [userId]);
```

예시:

```jsx
function UserProfile({
  userId,
}) {
  const [
    profile,
    setProfile,
  ] = useState(null);

  useEffect(() => {
    console.log(
      `${userId}번 사용자 정보 불러오기`
    );
  }, [userId]);

  return (
    <div>
      {profile ? (
        <h1>
          {profile.name}
        </h1>
      ) : (
        <h1>
          로딩 중...
        </h1>
      )}
    </div>
  );
}
```

---

## 7. 의존성 배열 정리

```jsx
useEffect(() => {
  ...
});
```

모든 렌더링 이후 실행

```jsx
useEffect(() => {
  ...
}, []);
```

컴포넌트가 처음 마운트될 때 실행

```jsx
useEffect(() => {
  ...
}, [value]);
```

`value`가 변경될 때 실행

---

## 8. Cleanup 함수

`useEffect`에서는 함수를 반환하여 Cleanup 작업을 정의할 수 있습니다.

```jsx
useEffect(() => {
  // Effect

  return () => {
    // Cleanup
  };
}, []);
```

Cleanup 함수는 다음과 같은 작업에 사용할 수 있습니다.

- 타이머 해제
- 이벤트 제거
- 리소스 정리

실행 시점:

- 컴포넌트가 Unmount될 때
- 의존성 값이 변경되어 Effect가 다시 실행되기 직전

---

## 9. 타이머 Cleanup

```jsx
function Timer() {
  useEffect(() => {
    const timerId =
      setInterval(() => {
        console.log(
          "타이머 실행 중"
        );
      }, 1000);

    return () => {
      clearInterval(
        timerId
      );
    };
  }, []);

  return (
    <div>
      타이머가 실행되고 있습니다.
    </div>
  );
}
```

컴포넌트가 사라진 뒤에도 타이머가 계속 실행되는 상황을 방지할 수 있습니다.

---

## 10. 렌더링과 Effect 실행 순서

컴포넌트 함수는 렌더링 과정에서 실행되고 `useEffect`는 렌더링 이후 실행됩니다.

```jsx
function Hello() {
  useEffect(() => {
    console.log(
      "1. useEffect"
    );
  }, []);

  console.log(
    "2. 컴포넌트 함수"
  );

  return (
    <h1>Hello</h1>
  );
}
```

개념적인 흐름:

```text
컴포넌트 함수 실행
      ↓
JSX 렌더링
      ↓
useEffect 실행
```

---

## 11. State 변화 감지

특정 State를 의존성 배열에 넣으면 해당 State가 변경될 때 Effect가 실행됩니다.

```jsx
function CounterWatcher() {
  const [
    count,
    setCount,
  ] = useState(0);

  const [
    cnt,
    setCnt,
  ] = useState(100);

  useEffect(() => {
    console.log(
      `현재 count: ${count}`
    );
  }, [count]);

  return (
    <div>
      <p>
        count: {count}
      </p>

      <button
        onClick={() =>
          setCount(
            count + 1
          )
        }
      >
        +1
      </button>

      <p>
        cnt: {cnt}
      </p>

      <button
        onClick={() =>
          setCnt(
            cnt + 100
          )
        }
      >
        +100
      </button>
    </div>
  );
}
```

이 Effect는 `count`가 변경될 때 실행됩니다.

---

## 12. Mount / Unmount 실습

조건부 렌더링을 통해 컴포넌트의 Mount와 Unmount를 확인할 수 있습니다.

```jsx
function Message() {
  useEffect(() => {
    console.log(
      "컴포넌트 마운트"
    );

    return () => {
      console.log(
        "컴포넌트 언마운트"
      );
    };
  }, []);

  return (
    <h3>
      Message 컴포넌트
    </h3>
  );
}
```

```jsx
function MessageApp() {
  const [
    show,
    setShow,
  ] = useState(true);

  return (
    <div>
      <button
        onClick={() =>
          setShow(!show)
        }
      >
        {show
          ? "숨기기"
          : "보이기"}
      </button>

      {show && <Message />}
    </div>
  );
}
```

---

## 13. `useState`를 이용한 Todo CRUD

Todo 예제에서는 기본 CRUD 기능을 구현합니다.

```text
Create
할 일 추가

Read
map()으로 목록 렌더링

Update
완료 상태 변경

Delete
filter()로 항목 삭제
```

State:

```jsx
const [
  todos,
  setTodos,
] = useState([]);

const [
  newTodo,
  setNewTodo,
] = useState("");
```

---

## 14. Create

새로운 Todo 객체를 기존 배열에 추가합니다.

```jsx
const handleCreateTodo =
  () => {
    if (
      newTodo.trim() === ""
    ) {
      return;
    }

    setTodos([
      ...todos,
      {
        id: Date.now(),
        text: newTodo,
        completed: false,
      },
    ]);

    setNewTodo("");
  };
```

---

## 15. Read

`map()`으로 Todo 목록을 렌더링합니다.

```jsx
<ul>
  {todos.map(todo => (
    <li key={todo.id}>
      {todo.text}
    </li>
  ))}
</ul>
```

---

## 16. Update

`map()`으로 특정 Todo의 완료 상태를 변경합니다.

```jsx
const handleToggleComplete =
  id => {
    setTodos(
      todos.map(
        todo =>
          todo.id === id
            ? {
                ...todo,
                completed:
                  !todo.completed,
              }
            : todo
      )
    );
  };
```

---

## 17. Delete

`filter()`를 이용하여 해당 ID의 항목을 제외합니다.

```jsx
const handleDeleteTodo =
  id => {
    setTodos(
      todos.filter(
        todo =>
          todo.id !== id
      )
    );
  };
```

---

## 18. Todo 목록 렌더링

```jsx
<ul>
  {todos.map(todo => (
    <li
      key={todo.id}
      style={{
        textDecoration:
          todo.completed
            ? "line-through"
            : "none",
      }}
    >
      <span>
        {todo.text}
      </span>

      <button
        onClick={() =>
          handleToggleComplete(
            todo.id
          )
        }
      >
        {todo.completed
          ? "미완료"
          : "완료"}
      </button>

      <button
        onClick={() =>
          handleDeleteTodo(
            todo.id
          )
        }
      >
        삭제
      </button>
    </li>
  ))}
</ul>
```

이 예제에서는 다음 개념이 함께 사용됩니다.

- `useState`
- `map()`
- `filter()`
- Spread Syntax
- Event Handling
- 조건부 스타일링

---

## 19. localStorage

`localStorage`는 브라우저가 제공하는 Web Storage 객체 중 하나입니다.

주요 특징:

- 새로고침 이후에도 데이터 유지 가능
- Key-Value 방식으로 저장
- 값은 문자열로 저장

주요 메소드:

```javascript
localStorage.setItem(
  key,
  value
);

localStorage.getItem(
  key
);

localStorage.removeItem(
  key
);

localStorage.clear();
```

---

## 20. 객체와 배열 저장

localStorage는 문자열만 저장하기 때문에 객체나 배열은 JSON 형식으로 변환합니다.

### 저장

```javascript
const userProfile = {
  name: "Alice",
  age: 25,
};

localStorage.setItem(
  "userProfile",
  JSON.stringify(
    userProfile
  )
);
```

### 불러오기

```javascript
const userProfileString =
  localStorage.getItem(
    "userProfile"
  );

const userProfile =
  JSON.parse(
    userProfileString
  );
```

---

## 21. Todo와 localStorage

초기 Todo 데이터를 localStorage에서 불러올 수 있습니다.

```jsx
const initialTodos =
  JSON.parse(
    localStorage.getItem(
      "todos"
    )
  ) || [];
```

`todos`가 변경될 때마다 localStorage에 저장합니다.

```jsx
useEffect(() => {
  localStorage.setItem(
    "todos",
    JSON.stringify(todos)
  );
}, [todos]);
```

전체 흐름:

```text
애플리케이션 실행
      ↓
localStorage
      ↓
초기 State
      ↓
Todo 변경
      ↓
setTodos()
      ↓
재렌더링
      ↓
useEffect([todos])
      ↓
localStorage 저장
```

---

## 22. Todo 데이터 영구 저장

Todo App에서 다음 기능을 함께 구현합니다.

- Todo 추가
- 완료 상태 변경
- Todo 삭제
- localStorage 저장
- 새로고침 후 데이터 유지

이 예제에서는 React State와 브라우저의 localStorage를 `useEffect`로 연결합니다.

---

## 23. `reduce()`

이 Branch에서는 `reduce()`도 다시 다룹니다.

`reduce()`는 배열의 여러 값을 하나의 결과값으로 줄이는 메소드입니다.

기본 구조:

```javascript
array.reduce(
  (
    accumulator,
    currentValue
  ) => {
    return newValue;
  },
  initialValue
);
```

예시:

```javascript
const numbers = [
  1,
  2,
  3,
  4,
];

const sum =
  numbers.reduce(
    (
      accumulator,
      currentNumber
    ) => {
      return (
        accumulator +
        currentNumber
      );
    },
    0
  );
```

결과:

```javascript
10
```

---

## 24. `reduce()`를 이용한 장바구니 총액 계산

상품 가격과 수량을 이용하여 장바구니 전체 금액을 계산합니다.

```jsx
const totalPrice =
  cart.reduce(
    (
      total,
      item
    ) =>
      total +
      (
        item.price *
        item.quantity
      ),
    0
  );
```

예시:

```jsx
function ShoppingCartReduceEx() {
  const [
    cart,
    setCart,
  ] = useState([
    {
      id: 1,
      name: "사과",
      price: 1000,
      quantity: 2,
    },
    {
      id: 2,
      name: "바나나",
      price: 1500,
      quantity: 1,
    },
    {
      id: 3,
      name: "딸기",
      price: 2000,
      quantity: 3,
    },
  ]);

  const totalPrice =
    cart.reduce(
      (
        total,
        item
      ) =>
        total +
        item.price *
          item.quantity,
      0
    );

  return (
    <div>
      <h2>장바구니</h2>

      <ul>
        {cart.map(
          item => (
            <li
              key={item.id}
            >
              {item.name}
              ({item.quantity}개)
              {" - "}
              {item.price *
                item.quantity}
              원
            </li>
          )
        )}
      </ul>

      <h3>
        총 가격:
        {totalPrice}원
      </h3>
    </div>
  );
}
```

---

## 25. 장바구니 실습

장바구니 실습에서는 다음 개념을 함께 활용합니다.

- 상품 목록
- 장바구니 추가
- 장바구니 삭제
- 수량 관리
- 총액 계산
- localStorage
- `useState`
- `useEffect`
- `reduce()`

장바구니 데이터도 localStorage에 저장하여 새로고침 이후 유지할 수 있습니다.

---

## 26. 상품 리뷰 실습

상품 리뷰 기능을 구현합니다.

초기 상품 데이터:

```jsx
const PRODUCT = {
  id: 1,
  name: "에코 텀블러",
  description:
    "환경을 생각하는 당신을 위한 텀블러",
  reviews: [],
};
```

State:

```jsx
const [
  product,
  setProduct,
] = useState(PRODUCT);

const [
  newReview,
  setNewReview,
] = useState("");
```

UI에는 다음 요소가 포함됩니다.

- 상품 정보
- 리뷰 입력
- 리뷰 등록
- 리뷰 목록
- 리뷰가 없을 때 안내 메시지
- 리뷰 CRUD 확장

---

## 27. 초대장 실습

마지막 실습에서는 지금까지의 개념을 종합하여 간단한 초대장을 만듭니다.

정적 정보:

- 제목
- 날짜
- 장소
- 기본 안내 정보

동적 기능:

- 참여 인원 카운터
- 참여 / 취소 상태 토글
- localStorage 저장
- D-Day 타이머
- 컴포넌트 재사용
- Props
- `useState`
- `useEffect`

State 관리와 Side Effect를 하나의 애플리케이션에서 함께 적용하는 종합 실습입니다.

</details>
