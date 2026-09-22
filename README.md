<details>
<summary>ENG (English Version)</summary>

# Dynamic List Rendering

## 1. What is List Rendering?

List rendering is a technique used to display multiple similar data items as a list on a web page.

Common examples include:

- Post lists
- Comment lists
- Product lists
- Schedule lists
- Todo lists

React commonly combines JavaScript's `map()` method with JSX to convert array data into React elements.

```jsx
const items = [
  "Jeju",
  "Hallyeosudo",
  "Daegwallyeong",
  "Geumgangsan",
];

return (
  <ul>
    {items.map(item => (
      <li>{item}</li>
    ))}
  </ul>
);
```

Instead of manually repeating JSX:

```jsx
<ul>
  <li>Jeju</li>
  <li>Hallyeosudo</li>
  <li>Daegwallyeong</li>
  <li>Geumgangsan</li>
</ul>
```

the UI can be generated from data.

---

## 2. `map()`

`map()` processes every element of an array and creates a new array.

Basic syntax:

```javascript
const newArray = array.map(
  (element, index) => {
    return transformedValue;
  }
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

const doubledNumbers =
  numbers.map(
    number => number * 2
  );

console.log(
  doubledNumbers
);
```

Result:

```javascript
[2, 4, 6, 8]
```

The original array remains unchanged.

---

## 3. Rendering JSX with `map()`

Array data can be converted into JSX elements.

Basic structure:

```jsx
{
  array.map(
    (item, index) => (
      <Element key={index}>
        {item}
      </Element>
    )
  )
}
```

The result of `map()` can first be stored in a variable.

```jsx
function MapRendering1() {
  const fruits = [
    "Apple",
    "Banana",
    "Grape",
  ];

  const fruitList =
    fruits.map(
      (fruit, index) => (
        <li key={index}>
          {fruit}
        </li>
      )
    );

  return (
    <ul>
      {fruitList}
    </ul>
  );
}
```

It can also be written directly inside JSX.

```jsx
function MapRendering2() {
  const fruits = [
    "Apple",
    "Banana",
    "Grape",
  ];

  return (
    <ul>
      {fruits.map(
        (fruit, index) => (
          <li key={index}>
            {fruit}
          </li>
        )
      )}
    </ul>
  );
}
```

---

## 4. Why `key` Props are Required

When rendering lists, each item should have a unique `key`.

```jsx
<li key={index}>
  {fruit}
</li>
```

React uses `key` to identify individual items efficiently.

Main purposes:

- Identify added items
- Identify removed items
- Handle reordered items
- Reduce unnecessary DOM updates
- Preserve component state correctly

Without a `key`, React displays a warning such as:

```text
Each child in a list should have a unique "key" prop.
```

---

## 5. Using Unique IDs as Keys

When list data contains a unique ID, that value can be used as the `key`.

```jsx
function ProductList() {
  const products = [
    {
      id: 1,
      name: "Apple",
      price: 2000,
    },
    {
      id: 2,
      name: "Banana",
      price: 1500,
    },
    {
      id: 3,
      name: "Strawberry",
      price: 3000,
    },
  ];

  return (
    <div>
      <h2>Product List</h2>

      <ul>
        {products.map(
          product => (
            <li
              key={product.id}
            >
              Product:
              {product.name},
              Price:
              {product.price}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

---

## 6. Todo List Rendering

Object arrays can also be rendered using `map()`.

```jsx
function MyTodos() {
  const myTodos = [
    {
      id: 1,
      text:
        "Prepare React lecture",
    },
    {
      id: 2,
      text: "Exercise",
    },
  ];

  return (
    <div>
      <h2>Todo List</h2>

      <ul>
        {myTodos.map(
          todo => (
            <li
              key={todo.id}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

The general process is:

```text
Array Data
    ↓
map()
    ↓
JSX Elements
    ↓
List Rendering
```

---

## 7. Conditional Rendering for Empty Lists

A list may contain no items.

Conditional rendering can provide a fallback message.

```jsx
{
  items.length > 0 ? (
    items.map(
      (item, index) => (
        <p key={index}>
          {item}
        </p>
      )
    )
  ) : (
    <p>
      Please add an item.
    </p>
  )
}
```

This allows the UI to respond to the current state of the array.

---

## 8. Rendering Object Arrays

The branch introduces two approaches for rendering object arrays.

```text
Object Array
   │
   ├── Render JSX directly
   │
   └── Separate each item
       into a component
```

---

## 9. Rendering with a Separate Component

Each item can be separated into its own component.

```jsx
const users = [
  {
    id: 1,
    name: "Jisu",
    email:
      "jisu@example.com",
  },
  {
    id: 2,
    name: "Minjun",
    email:
      "minjun@example.com",
  },
];
```

Item component:

```jsx
function User({
  name,
  email,
}) {
  return (
    <li>
      {name}: {email}
    </li>
  );
}
```

List component:

```jsx
function UserList() {
  return (
    <div>
      <h3>User List</h3>

      {users.map(
        user => (
          <User
            key={user.id}
            name={user.name}
            email={
              user.email
            }
          />
        )
      )}
    </div>
  );
}
```

The array data is converted into reusable `User` components.

---

## 10. Rendering in the Same Component

The list can also be rendered directly without separating each item into another component.

```jsx
function UserList() {
  return (
    <div>
      <h3>User List</h3>

      <ul>
        {users.map(
          user => (
            <li
              key={user.id}
            >
              {user.name}:
              {user.email}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

---

## 11. `filter()`

`filter()` creates a new array containing only elements that satisfy a condition.

Basic syntax:

```javascript
const newArray =
  array.filter(
    item => condition
  );
```

The original array is not directly modified.

Example:

```javascript
const numbers = [
  1,
  2,
  3,
  4,
  5,
  6,
  7,
  8,
  9,
  10,
];

const evenNumbers =
  numbers.filter(
    number =>
      number % 2 === 0
  );

console.log(
  evenNumbers
);
```

Result:

```javascript
[
  2,
  4,
  6,
  8,
  10
]
```

---

## 12. Filtering Object Arrays

`filter()` can also be used with arrays of objects.

```javascript
const users = [
  {
    name: "Alice",
    age: 25,
  },
  {
    name: "Bob",
    age: 17,
  },
  {
    name: "Charlie",
    age: 32,
  },
  {
    name: "David",
    age: 19,
  },
];

const adults =
  users.filter(
    user =>
      user.age >= 20
  );
```

Result:

```javascript
[
  {
    name: "Alice",
    age: 25,
  },
  {
    name: "Charlie",
    age: 32,
  }
]
```

---

## 13. Filtering Data in React

`filter()` can be combined with state and `map()`.

Example data:

```jsx
const initialTodos = [
  {
    id: 1,
    text:
      "Review React syntax",
    completed: true,
  },
  {
    id: 2,
    text:
      "Review props and state",
    completed: true,
  },
  {
    id: 3,
    text:
      "Review list rendering",
    completed: false,
  },
];
```

Display only incomplete todos:

```jsx
function TodoList() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const incompleteTodos =
    todos.filter(
      todo =>
        !todo.completed
    );

  return (
    <div>
      <h2>
        Incomplete Todos
      </h2>

      <ul>
        {incompleteTodos.map(
          todo => (
            <li
              key={todo.id}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

The process is:

```text
todos
  ↓
filter()
  ↓
Filtered Array
  ↓
map()
  ↓
JSX Rendering
```

---

## 14. Search with `filter()`

A search feature can be implemented by filtering data based on the user's input.

```jsx
function TodoSearch() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const [
    searchValue,
    setSearchValue,
  ] = useState("");

  const filteredTodos =
    todos.filter(
      todo =>
        todo.text
          .toLowerCase()
          .includes(
            searchValue
              .toLowerCase()
          )
    );

  return (
    <div>
      <h2>Todo Search</h2>

      <input
        type="text"
        placeholder="Search..."
        value={searchValue}
        onChange={e =>
          setSearchValue(
            e.target.value
          )
        }
      />

      <ul>
        {filteredTodos.map(
          todo => (
            <li
              key={todo.id}
              style={{
                textDecoration:
                  todo.completed
                    ? "line-through"
                    : "none",
              }}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

This combines:

```text
User Input
    ↓
State
    ↓
filter()
    ↓
map()
    ↓
Filtered List
```

---

## 15. Adding List Items

The branch combines `useState`, spread syntax, and unique IDs to add new items.

```jsx
const [
  todos,
  setTodos,
] = useState(
  initialTodos
);

const [
  newTodo,
  setNewTodo,
] = useState("");
```

Add function:

```jsx
const handleAddTodo = () => {
  if (
    newTodo.trim() === ""
  ) {
    return;
  }

  const newId =
    uuidv4();

  const newTodoItem = {
    id: newId,
    text: newTodo,
    completed: false,
  };

  setTodos(
    prevTodos => [
      ...prevTodos,
      newTodoItem,
    ]
  );

  setNewTodo("");
};
```

A new array is created instead of directly modifying the previous array.

---

## 16. Removing List Items

`filter()` can remove an item by keeping only items whose IDs do not match the selected ID.

```jsx
const handleRemoveTodo =
  id => {
    setTodos(
      prevTodos =>
        prevTodos.filter(
          todo =>
            todo.id !== id
        )
    );
  };
```

Conceptually:

```text
Original Array
    ↓
filter()
    ↓
Remove Matching ID
    ↓
New Array
    ↓
State Update
```

---

## 17. Add and Delete Todo Example

```jsx
function TodoList() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const [
    newTodo,
    setNewTodo,
  ] = useState("");

  const handleAddTodo =
    () => {
      if (
        newTodo.trim() === ""
      ) {
        return;
      }

      const newTodoItem = {
        id: uuidv4(),
        text: newTodo,
        completed: false,
      };

      setTodos(
        prevTodos => [
          ...prevTodos,
          newTodoItem,
        ]
      );

      setNewTodo("");
    };

  const handleRemoveTodo =
    id => {
      setTodos(
        prevTodos =>
          prevTodos.filter(
            todo =>
              todo.id !== id
          )
      );
    };

  return (
    <div>
      <h2>Todo List</h2>

      <input
        type="text"
        value={newTodo}
        onChange={e =>
          setNewTodo(
            e.target.value
          )
        }
        placeholder="Enter a new todo"
      />

      <button
        onClick={
          handleAddTodo
        }
      >
        Add
      </button>

      <ul>
        {todos.length > 0 ? (
          todos.map(
            todo => (
              <li
                key={todo.id}
              >
                {todo.text}

                <button
                  onClick={() =>
                    handleRemoveTodo(
                      todo.id
                    )
                  }
                >
                  Delete
                </button>
              </li>
            )
          )
        ) : (
          <p>
            Please add an item.
          </p>
        )}
      </ul>
    </div>
  );
}
```

This example connects several React concepts:

- `useState`
- `onChange`
- `onClick`
- Spread syntax
- `map()`
- `filter()`
- `key`
- Conditional rendering

---

## 18. Unique IDs with `uuid`

The example uses `uuid` to generate unique IDs.

```jsx
import {
  v4 as uuidv4,
} from "uuid";
```

A unique ID can be created when a new item is added.

```jsx
const newId =
  uuidv4();
```

```jsx
const newTodoItem = {
  id: newId,
  text: newTodo,
  completed: false,
};
```

The ID can then be used as the list item's `key`.

```jsx
<li key={todo.id}>
```

---

## 19. Todo App Practice

The Todo application practice combines the concepts covered in this branch.

Requirements:

- Add an item using entered text
- Delete an item with a button
- Use `map()`
- Use `filter()`
- Use `onChange`
- Use `onClick`
- Render object-array data
- Practice both list rendering approaches

Main flow:

```text
Input
  ↓
onChange
  ↓
State
  ↓
Add Button
  ↓
New Array
  ↓
map()
  ↓
List Rendering
  ↓
Delete Button
  ↓
filter()
  ↓
Updated List
```

---

## 20. Product Management App Practice

The branch also includes a simple product management application.

The example UI contains:

- Product name input
- Price input
- Product addition
- Product list rendering
- Delete button
- Total product count

An extended example also includes an image URL for each product.

The exercise combines previously learned React features to manage dynamic product data.

---

## 21. Blog Post Rendering Practice

The final exercise creates a blog post list.

Each blog post is stored as an object inside an array and rendered repeatedly.

Example structure:

```jsx
const postList = [
  {
    id: 1,
    title: "...",
    author: "...",
    content: "...",
  },
  {
    id: 2,
    title: "...",
    author: "...",
    content: "...",
  },
];
```

Rendering structure:

```jsx
{
  postList.map(
    post => (
      <BlogPost
        key={post.id}
        {...post}
      />
    )
  )
}
```

The main idea is to convert repeated UI content into array data and render it dynamically.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# Dynamic List Rendering

## 1. 리스트 렌더링이란?

리스트 렌더링은 여러 개의 유사한 데이터를 웹 화면에 목록 형태로 출력할 때 사용하는 방법입니다.

대표적인 예시는 다음과 같습니다.

- 게시글 목록
- 댓글 목록
- 상품 목록
- 일정 목록
- Todo 목록

React에서는 JavaScript의 `map()`과 JSX를 결합하여 배열 데이터를 React 요소로 변환합니다.

```jsx
const items = [
  "제주",
  "한려수도",
  "대관령",
  "금강산",
];

return (
  <ul>
    {items.map(item => (
      <li>{item}</li>
    ))}
  </ul>
);
```

다음처럼 JSX를 직접 반복해서 작성하는 대신:

```jsx
<ul>
  <li>제주</li>
  <li>한려수도</li>
  <li>대관령</li>
  <li>금강산</li>
</ul>
```

배열 데이터를 이용하여 UI를 동적으로 생성할 수 있습니다.

---

## 2. `map()`

`map()`은 배열의 각 요소를 처리하여 새로운 배열을 반환하는 JavaScript 배열 메소드입니다.

기본 문법:

```javascript
const newArray =
  array.map(
    (element, index) => {
      return transformedValue;
    }
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

const doubledNumbers =
  numbers.map(
    number =>
      number * 2
  );

console.log(
  doubledNumbers
);
```

결과:

```javascript
[2, 4, 6, 8]
```

원본 배열은 변경되지 않습니다.

---

## 3. `map()`을 이용한 JSX 렌더링

배열 데이터를 JSX 요소들의 배열로 변환할 수 있습니다.

기본 구조:

```jsx
{
  array.map(
    (item, index) => (
      <Element key={index}>
        {item}
      </Element>
    )
  )
}
```

`map()`의 결과를 변수에 저장한 뒤 렌더링할 수 있습니다.

```jsx
function MapRendering1() {
  const fruits = [
    "사과",
    "바나나",
    "포도",
  ];

  const fruitList =
    fruits.map(
      (fruit, index) => (
        <li key={index}>
          {fruit}
        </li>
      )
    );

  return (
    <ul>
      {fruitList}
    </ul>
  );
}
```

JSX 내부에서 직접 작성할 수도 있습니다.

```jsx
function MapRendering2() {
  const fruits = [
    "사과",
    "바나나",
    "포도",
  ];

  return (
    <ul>
      {fruits.map(
        (fruit, index) => (
          <li key={index}>
            {fruit}
          </li>
        )
      )}
    </ul>
  );
}
```

---

## 4. `key` Props의 필요성

리스트를 렌더링할 때 각 항목에는 고유한 `key`를 지정합니다.

```jsx
<li key={index}>
  {fruit}
</li>
```

React는 `key`를 이용하여 각각의 리스트 항목을 식별합니다.

주요 역할:

- 추가된 항목 식별
- 삭제된 항목 식별
- 항목 순서 변경 처리
- 불필요한 DOM 업데이트 감소
- 각 컴포넌트의 상태 유지

`key`를 지정하지 않으면 개발자 도구에 다음과 같은 경고가 출력됩니다.

```text
Each child in a list should have a unique "key" prop.
```

---

## 5. 고유 ID를 `key`로 사용

객체 배열에 고유 ID가 있다면 해당 ID를 `key`로 사용할 수 있습니다.

```jsx
function ProductList() {
  const products = [
    {
      id: 1,
      name: "사과",
      price: 2000,
    },
    {
      id: 2,
      name: "바나나",
      price: 1500,
    },
    {
      id: 3,
      name: "딸기",
      price: 3000,
    },
  ];

  return (
    <div>
      <h2>상품 목록</h2>

      <ul>
        {products.map(
          product => (
            <li
              key={product.id}
            >
              상품명:
              {product.name},
              가격:
              {product.price}원
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

---

## 6. Todo 목록 렌더링

객체 배열 역시 `map()`을 이용하여 렌더링할 수 있습니다.

```jsx
function MyTodos() {
  const myTodos = [
    {
      id: 1,
      text:
        "리액트 강의 준비",
    },
    {
      id: 2,
      text: "운동하기",
    },
  ];

  return (
    <div>
      <h2>할 일 목록</h2>

      <ul>
        {myTodos.map(
          todo => (
            <li
              key={todo.id}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

전체적인 흐름:

```text
배열 데이터
    ↓
map()
    ↓
JSX 요소
    ↓
리스트 렌더링
```

---

## 7. 빈 리스트의 조건부 렌더링

배열에 요소가 없을 경우를 대비하여 조건부 렌더링을 사용할 수 있습니다.

```jsx
{
  items.length > 0 ? (
    items.map(
      (item, index) => (
        <p key={index}>
          {item}
        </p>
      )
    )
  ) : (
    <p>
      항목 추가하세요
    </p>
  )
}
```

현재 배열 상태에 따라 서로 다른 UI를 보여줄 수 있습니다.

---

## 8. 객체 배열 렌더링 방식

객체 배열을 렌더링하는 방법으로 두 가지 방식이 소개됩니다.

```text
객체 배열
   │
   ├── 같은 위치에서 직접 렌더링
   │
   └── 각 항목을 별도
       컴포넌트로 분리
```

---

## 9. 항목을 컴포넌트로 분리

객체 배열의 각 항목을 별도의 컴포넌트로 구성할 수 있습니다.

```jsx
const users = [
  {
    id: 1,
    name: "지수",
    email:
      "jisu@example.com",
  },
  {
    id: 2,
    name: "민준",
    email:
      "minjun@example.com",
  },
];
```

개별 컴포넌트:

```jsx
function User({
  name,
  email,
}) {
  return (
    <li>
      {name}: {email}
    </li>
  );
}
```

리스트 컴포넌트:

```jsx
function UserList() {
  return (
    <div>
      <h3>사용자 목록</h3>

      {users.map(
        user => (
          <User
            key={user.id}
            name={user.name}
            email={
              user.email
            }
          />
        )
      )}
    </div>
  );
}
```

객체 배열의 각 항목을 재사용 가능한 `User` 컴포넌트로 변환합니다.

---

## 10. 같은 위치에서 직접 렌더링

별도의 컴포넌트로 분리하지 않고 현재 컴포넌트에서 바로 렌더링할 수도 있습니다.

```jsx
function UserList() {
  return (
    <div>
      <h3>사용자 목록</h3>

      <ul>
        {users.map(
          user => (
            <li
              key={user.id}
            >
              {user.name}:
              {user.email}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

---

## 11. `filter()`

`filter()`는 조건을 만족하는 요소만 모아 새로운 배열을 반환합니다.

기본 문법:

```javascript
const newArray =
  array.filter(
    item => condition
  );
```

원본 배열을 직접 변경하지 않습니다.

예시:

```javascript
const numbers = [
  1,
  2,
  3,
  4,
  5,
  6,
  7,
  8,
  9,
  10,
];

const evenNumbers =
  numbers.filter(
    number =>
      number % 2 === 0
  );

console.log(
  evenNumbers
);
```

결과:

```javascript
[
  2,
  4,
  6,
  8,
  10
]
```

---

## 12. 객체 배열 필터링

객체 배열에서도 조건을 이용하여 필요한 항목만 추출할 수 있습니다.

```javascript
const users = [
  {
    name: "Alice",
    age: 25,
  },
  {
    name: "Bob",
    age: 17,
  },
  {
    name: "Charlie",
    age: 32,
  },
  {
    name: "David",
    age: 19,
  },
];

const adults =
  users.filter(
    user =>
      user.age >= 20
  );
```

결과:

```javascript
[
  {
    name: "Alice",
    age: 25,
  },
  {
    name: "Charlie",
    age: 32,
  }
]
```

---

## 13. React에서 `filter()` 사용

`filter()`는 State 및 `map()`과 함께 사용할 수 있습니다.

초기 데이터:

```jsx
const initialTodos = [
  {
    id: 1,
    text:
      "리액트 문법 정리",
    completed: true,
  },
  {
    id: 2,
    text:
      "props와 state 복습",
    completed: true,
  },
  {
    id: 3,
    text:
      "반복 렌더링 기능 정리",
    completed: false,
  },
];
```

미완료 항목만 출력:

```jsx
function TodoList() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const incompleteTodos =
    todos.filter(
      todo =>
        !todo.completed
    );

  return (
    <div>
      <h2>
        미완료 할 일 목록
      </h2>

      <ul>
        {incompleteTodos.map(
          todo => (
            <li
              key={todo.id}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

처리 과정:

```text
todos
  ↓
filter()
  ↓
필터링된 배열
  ↓
map()
  ↓
JSX 렌더링
```

---

## 14. `filter()`를 이용한 검색

사용자가 입력한 검색어를 기준으로 배열을 필터링하여 검색 기능을 만들 수 있습니다.

```jsx
function TodoSearch() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const [
    searchValue,
    setSearchValue,
  ] = useState("");

  const filteredTodos =
    todos.filter(
      todo =>
        todo.text
          .toLowerCase()
          .includes(
            searchValue
              .toLowerCase()
          )
    );

  return (
    <div>
      <h2>할 일 검색</h2>

      <input
        type="text"
        placeholder="검색어를 입력하세요..."
        value={searchValue}
        onChange={e =>
          setSearchValue(
            e.target.value
          )
        }
      />

      <ul>
        {filteredTodos.map(
          todo => (
            <li
              key={todo.id}
              style={{
                textDecoration:
                  todo.completed
                    ? "line-through"
                    : "none",
              }}
            >
              {todo.text}
            </li>
          )
        )}
      </ul>
    </div>
  );
}
```

전체 흐름:

```text
사용자 입력
    ↓
State
    ↓
filter()
    ↓
map()
    ↓
검색 결과 렌더링
```

---

## 15. 리스트 항목 추가

`useState`, Spread Syntax, 고유 ID를 이용하여 새로운 항목을 추가할 수 있습니다.

```jsx
const [
  todos,
  setTodos,
] = useState(
  initialTodos
);

const [
  newTodo,
  setNewTodo,
] = useState("");
```

추가 함수:

```jsx
const handleAddTodo = () => {
  if (
    newTodo.trim() === ""
  ) {
    return;
  }

  const newId =
    uuidv4();

  const newTodoItem = {
    id: newId,
    text: newTodo,
    completed: false,
  };

  setTodos(
    prevTodos => [
      ...prevTodos,
      newTodoItem,
    ]
  );

  setNewTodo("");
};
```

기존 배열을 직접 수정하지 않고 새로운 배열을 생성하여 State를 변경합니다.

---

## 16. 리스트 항목 삭제

삭제할 항목의 ID와 다른 항목만 남도록 `filter()`를 적용할 수 있습니다.

```jsx
const handleRemoveTodo =
  id => {
    setTodos(
      prevTodos =>
        prevTodos.filter(
          todo =>
            todo.id !== id
        )
    );
  };
```

개념적으로는 다음과 같습니다.

```text
기존 배열
    ↓
filter()
    ↓
삭제할 ID 제외
    ↓
새로운 배열
    ↓
State 변경
```

---

## 17. Todo 추가 및 삭제 예제

```jsx
function TodoList() {
  const [
    todos,
    setTodos,
  ] = useState(
    initialTodos
  );

  const [
    newTodo,
    setNewTodo,
  ] = useState("");

  const handleAddTodo =
    () => {
      if (
        newTodo.trim() === ""
      ) {
        return;
      }

      const newTodoItem = {
        id: uuidv4(),
        text: newTodo,
        completed: false,
      };

      setTodos(
        prevTodos => [
          ...prevTodos,
          newTodoItem,
        ]
      );

      setNewTodo("");
    };

  const handleRemoveTodo =
    id => {
      setTodos(
        prevTodos =>
          prevTodos.filter(
            todo =>
              todo.id !== id
          )
      );
    };

  return (
    <div>
      <h2>할 일 목록</h2>

      <input
        type="text"
        value={newTodo}
        onChange={e =>
          setNewTodo(
            e.target.value
          )
        }
        placeholder="새로운 할 일을 입력하세요"
      />

      <button
        onClick={
          handleAddTodo
        }
      >
        추가
      </button>

      <ul>
        {todos.length > 0 ? (
          todos.map(
            todo => (
              <li
                key={todo.id}
              >
                {todo.text}

                <button
                  onClick={() =>
                    handleRemoveTodo(
                      todo.id
                    )
                  }
                >
                  삭제
                </button>
              </li>
            )
          )
        ) : (
          <p>
            항목 추가하세요
          </p>
        )}
      </ul>
    </div>
  );
}
```

이 예제에서는 지금까지 배운 여러 개념이 함께 사용됩니다.

- `useState`
- `onChange`
- `onClick`
- Spread Syntax
- `map()`
- `filter()`
- `key`
- 조건부 렌더링

---

## 18. `uuid`를 이용한 고유 ID

실습에서는 고유한 ID를 생성하기 위해 `uuid` 라이브러리를 사용합니다.

```jsx
import {
  v4 as uuidv4,
} from "uuid";
```

새로운 항목을 추가할 때 ID를 생성합니다.

```jsx
const newId =
  uuidv4();
```

```jsx
const newTodoItem = {
  id: newId,
  text: newTodo,
  completed: false,
};
```

생성한 ID는 리스트의 `key`로 사용할 수 있습니다.

```jsx
<li key={todo.id}>
```

---

## 19. Todo App 실습

Todo App 실습에서는 이번 Branch의 주요 개념을 함께 사용합니다.

조건:

- 입력한 텍스트를 목록에 추가
- `add` 버튼으로 항목 추가
- `del` 버튼으로 해당 항목 삭제
- `map()` 사용
- `filter()` 사용
- `onChange` 사용
- `onClick` 사용
- 객체 배열 렌더링
- 두 가지 객체 배열 렌더링 방식 모두 적용

전체 흐름:

```text
입력
  ↓
onChange
  ↓
State
  ↓
추가 버튼
  ↓
새로운 배열 생성
  ↓
map()
  ↓
목록 렌더링
  ↓
삭제 버튼
  ↓
filter()
  ↓
목록 갱신
```

---

## 20. 상품 관리 App 실습

지금까지 학습한 React 기능을 활용하여 간단한 상품 관리 App을 작성합니다.

예시 UI에는 다음 기능이 포함됩니다.

- 상품명 입력
- 가격 입력
- 상품 추가
- 상품 목록 출력
- 상품 삭제
- 전체 상품 개수 표시

확장된 예제에서는 각 상품의 이미지 URL도 입력하여 상품 이미지와 함께 렌더링합니다.

동적인 상품 데이터를 관리하면서 지금까지 배운 React 기능을 종합적으로 활용합니다.

---

## 21. 블로그 포스트 렌더링 실습

마지막 실습에서는 블로그 게시글을 객체 배열로 만들고 반복 렌더링합니다.

예시 구조:

```jsx
const postList = [
  {
    id: 1,
    title: "...",
    author: "...",
    content: "...",
  },
  {
    id: 2,
    title: "...",
    author: "...",
    content: "...",
  },
];
```

렌더링 구조:

```jsx
{
  postList.map(
    post => (
      <BlogPost
        key={post.id}
        {...post}
      />
    )
  )
}
```

반복되는 UI 정보를 배열 객체로 만들고 `map()`을 이용하여 동적으로 렌더링하는 것이 핵심입니다.

</details>
