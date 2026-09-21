<details>
<summary>ENG (English Version)</summary>

# Component 2 / State / Event Handling

This branch contains notes and practice code covering advanced props usage, `children`, React state with `useState`, parent-child state interaction, event handling, SyntheticEvent, and controlled form components.

## 1. Default Values for Props

Default values can be assigned when a parent component does not provide a prop.

Using destructuring with a default value:

```jsx
function Welcome({ name = "Visitor" }) {
  return (
    <p>
      Welcome, {name}!
    </p>
  );
}

export default Welcome;
```

Usage:

```jsx
<Welcome name="Chulsoo" />

<Welcome />
```

When `name` is not provided, `"Visitor"` is used.

---

## 2. Destructuring Props

A component can receive the entire `props` object.

```jsx
function GreetUser(props) {
  return (
    <div>
      <p>Name: {props.name}</p>
      <p>Age: {props.age}</p>
    </div>
  );
}
```

Props can also be extracted through destructuring.

```jsx
function GreetUser({ name, age }) {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
}
```

This makes frequently used props easier to access.

---

## 3. Multiple Default Props

Several props can have their own default values.

```jsx
function ProfileCard({
  name = "Anonymous User",
  occupation = "No Occupation",
  age = "Unknown",
}) {
  return (
    <div
      style={{
        border: "1px solid #007bff",
        padding: "15px",
        margin: "10px",
      }}
    >
      <h3>{name}</h3>
      <p>Occupation: {occupation}</p>
      <p>Age: {age}</p>
    </div>
  );
}
```

Usage:

```jsx
<ProfileCard
  name="Kim"
  occupation="Developer"
  age={28}
/>

<ProfileCard
  name="Lee"
  occupation="Designer"
/>

<ProfileCard />
```

Missing values are replaced with their default values.

---

## 4. `children` Props

`children` represents the content placed between the opening and closing tags of a component.

```jsx
<Component>
  Content
</Component>
```

The content is automatically passed as:

```javascript
props.children
```

Basic structure:

```jsx
function Component({ children }) {
  return (
    <div>
      {children}
    </div>
  );
}
```

`children` can contain:

- Text
- JSX elements
- Other components
- JavaScript expressions

---

## 5. Reusable Wrapper Components

`children` is useful when creating wrapper components such as buttons, cards, and layouts.

### Button

```jsx
function Button({ children }) {
  return (
    <button>
      {children}
    </button>
  );
}
```

Usage:

```jsx
<Button>Confirm</Button>

<Button>
  <span>▶</span>
  Start
</Button>
```

### Highlight

```jsx
function Highlight({ children }) {
  return (
    <div
      style={{
        backgroundColor: "yellow",
      }}
    >
      {children}
    </div>
  );
}
```

Usage:

```jsx
<Highlight>
  <strong>Notice: </strong>
  React Programming Room 204
</Highlight>
```

---

## 6. Layout Components with `children`

A layout component can define common page elements while receiving different content through `children`.

```jsx
function Layout({ children }) {
  return (
    <div>
      <header>
        Frontend Programming
      </header>

      <main>
        {children}
      </main>

      <footer>
        © 2025 ~
      </footer>
    </div>
  );
}
```

Usage:

```jsx
function App() {
  return (
    <Layout>
      <h1>Home</h1>

      <p>
        This content is passed
        through children.
      </p>
    </Layout>
  );
}
```

The structure can be represented as:

```text
Layout
├── Header
├── children
└── Footer
```

---

## 7. State

**State** represents dynamic data managed inside a React component.

State can change because of:

- User input
- Button clicks
- Time
- Other application interactions

When state changes, the component is rendered again.

State should mainly contain values that affect rendering or application data flow.

---

## 8. Props vs State

| | Props | State |
|---|---|---|
| Purpose | Data received from a parent | Data managed inside a component |
| Modification | Read-only | Changeable |
| Example | `<Welcome name="Jisoo" />` | `useState(0)` |

Props are received from outside the component, while state belongs to the component that manages it.

---

## 9. `useState`

The `useState` Hook is used to manage state in functional components.

```jsx
import React, {
  useState,
} from "react";
```

Basic syntax:

```jsx
const [
  state,
  setState,
] = useState(initialValue);
```

Example:

```jsx
function Counter() {
  const [
    count,
    setCount,
  ] = useState(0);

  return (
    <div>
      <h2>
        Current Number: {count}
      </h2>

      <button
        onClick={() =>
          setCount(
            prevCount =>
              prevCount + 1
          )
        }
      >
        +1
      </button>
    </div>
  );
}
```

---

## 10. Updating Parent State from a Child Component

A child component cannot directly modify state owned by its parent.

However, the parent can pass a function through props.

```text
Parent State
     ↓
Parent Function
     ↓
Function passed through Props
     ↓
Child Component
     ↓
Function called
     ↓
Parent State Updated
```

Example:

```jsx
function ChildCounter({
  increment,
}) {
  return (
    <button
      onClick={increment}
    >
      Increase
    </button>
  );
}
```

Parent component:

```jsx
function ParentCounter() {
  const [
    count,
    setCount,
  ] = useState(0);

  const increment = () => {
    setCount(
      prevCount =>
        prevCount + 1
    );
  };

  return (
    <div>
      <h1>
        Count: {count}
      </h1>

      <ChildCounter
        increment={increment}
      />
    </div>
  );
}
```

---

## 11. State Practice

A state value can be used to control both displayed data and UI properties.

For example, a like button can increase the value until it reaches a limit.

```jsx
function LikeButton() {
  const [
    likes,
    setLikes,
  ] = useState(0);

  return (
    <div>
      <p>
        Likes: {likes}
      </p>

      <button
        disabled={likes > 10}
        onClick={() =>
          setLikes(
            prev => prev + 1
          )
        }
      >
        Like
      </button>
    </div>
  );
}
```

---

## 12. React Event Handling

React uses its own event system to provide consistent event handling across browsers.

React events use **camelCase** names.

HTML:

```html
<button onclick="handleClick()">
  Click
</button>
```

React JSX:

```jsx
<button onClick={handleClick}>
  Click
</button>
```

Important:

```jsx
onClick={handleClick}
```

passes the function itself.

```jsx
onClick={handleClick()}
```

calls the function immediately and is not the normal event handler pattern.

---

## 13. Event Handler Without Parameters

```jsx
function BasicClickExample() {
  const [
    message,
    setMessage,
  ] = useState(
    "Before clicking the button."
  );

  const handleBtnClick = () => {
    setMessage(
      "The button was clicked!"
    );
  };

  return (
    <div>
      <p>{message}</p>

      <button
        onClick={handleBtnClick}
      >
        Click
      </button>
    </div>
  );
}
```

---

## 14. Passing Parameters to an Event Handler

When an event handler requires additional parameters, an arrow function can be used.

```jsx
function ButtonLogger() {
  const [
    log,
    setLog,
  ] = useState("");

  const handleButtonClick =
    buttonName => {
      setLog(
        `${buttonName} clicked!`
      );
    };

  return (
    <div>
      <h2>Log: {log}</h2>

      <button
        onClick={() =>
          handleButtonClick(
            "Button 1"
          )
        }
      >
        Button 1
      </button>

      <button
        onClick={() =>
          handleButtonClick(
            "Button 2"
          )
        }
      >
        Button 2
      </button>
    </div>
  );
}
```

---

## 15. Event Object

React automatically passes an event object to an event handler.

```jsx
function InputChangeExample() {
  const [
    inputValue,
    setInputValue,
  ] = useState("");

  const handleInputChange = e => {
    setInputValue(
      e.target.value
    );
  };

  return (
    <div>
      <p>
        Input: {inputValue}
      </p>

      <input
        type="text"
        onChange={
          handleInputChange
        }
      />
    </div>
  );
}
```

When only the function name is passed to `onChange`, React automatically provides the event object.

---

## 16. Passing a Parameter and Event Object Together

An arrow function can explicitly pass both a custom parameter and the event object.

```jsx
function ProductList() {
  const handlePurchase =
    (productId, event) => {
      console.log(
        `Product ID: ${productId}`
      );

      console.log(
        `X=${event.clientX}, Y=${event.clientY}`
      );
    };

  return (
    <div>
      <h3>
        Available Products
      </h3>

      <button
        onClick={e =>
          handlePurchase(
            "product-1",
            e
          )
        }
      >
        Buy Product 1
      </button>

      <button
        onClick={e =>
          handlePurchase(
            "product-2",
            e
          )
        }
      >
        Buy Product 2
      </button>
    </div>
  );
}
```

---

## 17. SyntheticEvent

React's event object is called a **SyntheticEvent**.

It wraps the browser's native event and provides a standardized interface.

Common properties and methods:

| Property / Method | Description |
|---|---|
| `event.type` | Event type such as `click` or `change` |
| `event.target` | DOM element where the event occurred |
| `event.currentTarget` | DOM element with the event handler |
| `event.preventDefault()` | Prevents the default browser behavior |
| `event.stopPropagation()` | Stops event propagation |

---

## 18. Event-Specific Properties

### Mouse Events

```javascript
event.clientX
event.clientY
event.screenX
event.screenY
event.button
```

### Keyboard Events

```javascript
event.key
event.code
event.keyCode
```

### Form Events

```javascript
event.target.value
event.target.checked
```

### Touch Events

```javascript
event.touches
```

### Clipboard Events

```javascript
event.clipboardData
```

---

## 19. Controlled Components

A **Controlled Component** is a form element whose value is managed by React state.

The basic flow is:

```text
User Input
    ↓
onChange
    ↓
Event Handler
    ↓
State Update
    ↓
Re-render
```

Example:

```jsx
function App() {
  const [
    text,
    setText,
  ] = useState("");

  const handleChange = e => {
    setText(
      e.target.value
    );
  };

  return (
    <div>
      <input
        value={text}
        onChange={handleChange}
      />

      <p>
        Input: {text}
      </p>
    </div>
  );
}
```

---

## 20. Form with Independent State Values

Each input field can use its own state.

```jsx
function SignUpForm() {
  const [
    name,
    setName,
  ] = useState("");

  const [
    email,
    setEmail,
  ] = useState("");

  const handleNameChange =
    e => {
      setName(
        e.target.value
      );
    };

  const handleEmailChange =
    e => {
      setEmail(
        e.target.value
      );
    };

  const handleSubmit = e => {
    e.preventDefault();

    alert(
      `Name: ${name}\nEmail: ${email}`
    );
  };

  return (
    <form
      onSubmit={handleSubmit}
    >
      <h2>Sign Up</h2>

      <label>
        Name:

        <input
          type="text"
          value={name}
          onChange={
            handleNameChange
          }
        />
      </label>

      <label>
        Email:

        <input
          type="email"
          value={email}
          onChange={
            handleEmailChange
          }
        />
      </label>

      <h3>Entered Information</h3>

      <p>Name: {name}</p>
      <p>Email: {email}</p>

      <button type="submit">
        Sign Up
      </button>
    </form>
  );
}
```

---

## 21. Managing Multiple Inputs with One Object State

Several form fields can also be managed using one state object.

```jsx
function SignUpForm() {
  const [
    userInfo,
    setUserInfo,
  ] = useState({
    name: "",
    email: "",
  });

  const handleChange = e => {
    const {
      name,
      value,
    } = e.target;

    setUserInfo(
      prevInfo => ({
        ...prevInfo,
        [name]: value,
      })
    );
  };

  const handleSubmit = e => {
    e.preventDefault();

    alert(
      `Name: ${userInfo.name}\nEmail: ${userInfo.email}`
    );
  };

  return (
    <form
      onSubmit={handleSubmit}
    >
      <label>
        Name:

        <input
          type="text"
          name="name"
          value={userInfo.name}
          onChange={handleChange}
        />
      </label>

      <label>
        Email:

        <input
          type="email"
          name="email"
          value={userInfo.email}
          onChange={handleChange}
        />
      </label>

      <p>
        Name: {userInfo.name}
      </p>

      <p>
        Email: {userInfo.email}
      </p>

      <button type="submit">
        Sign Up
      </button>
    </form>
  );
}
```

The important update pattern is:

```jsx
setUserInfo(
  prevInfo => ({
    ...prevInfo,
    [name]: value,
  })
);
```

The previous state is copied with spread syntax, and only the changed property is updated.

---

## 22. `preventDefault()`

Submitting an HTML form normally causes the browser to perform its default form submission behavior.

React form handlers can prevent this using:

```javascript
event.preventDefault();
```

Example:

```jsx
const handleSubmit = e => {
  e.preventDefault();

  console.log(
    "Form submitted"
  );
};
```

---

## 23. Form Practice

The branch includes a feedback form practice containing:

- Gender
- Age
- Email
- Product review
- Submit button

The form uses state to manage user input and displays a message when the form is submitted.

```jsx
const handleSubmit = () => {
  alert(
    "Thank you. The review coupon will be sent by email."
  );
};
```

The exercise also extends the form so that multiple input fields can be managed through a single object state.

---

## 24. Real-Time Character Counter

The final practice combines state and event handling.

Requirements:

- Use a `<textarea>`
- Update state whenever the user enters text
- Display the current character count
- Limit input to 20 characters
- Change the counter style when the limit is reached

Main concepts:

```text
onChange
   ↓
Event Object
   ↓
State Update
   ↓
Character Count
   ↓
Conditional Styling
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# Component 2 / State / Event Handling

이 Branch에서는 Props의 심화 사용법과 `children`, `useState`를 이용한 상태 관리, 부모·자식 컴포넌트 간 상태 제어, React 이벤트 핸들링, SyntheticEvent, Controlled Component를 학습했습니다.

## 1. Props 기본값

부모 컴포넌트에서 Props 값을 전달하지 않았을 경우 사용할 기본값을 지정할 수 있습니다.

구조 분해 할당과 함께 기본값을 지정합니다.

```jsx
function Welcome({
  name = "방문자",
}) {
  return (
    <p>
      환영합니다, {name}님!
    </p>
  );
}

export default Welcome;
```

사용:

```jsx
<Welcome name="철수" />

<Welcome />
```

`name`이 전달되지 않으면 `"방문자"`가 사용됩니다.

---

## 2. Props 구조 분해 할당

Props 객체 전체를 인자로 받을 수 있습니다.

```jsx
function GreetUser(props) {
  return (
    <div>
      <p>
        이름: {props.name}
      </p>

      <p>
        나이: {props.age}
      </p>
    </div>
  );
}
```

구조 분해 할당을 이용하여 필요한 Props만 추출할 수도 있습니다.

```jsx
function GreetUser({
  name,
  age,
}) {
  return (
    <div>
      <p>이름: {name}</p>
      <p>나이: {age}</p>
    </div>
  );
}
```

자주 사용하는 Props를 보다 간결하게 사용할 수 있습니다.

---

## 3. 여러 Props에 기본값 지정

각 Props마다 기본값을 지정할 수 있습니다.

```jsx
function ProfileCard({
  name = "익명 사용자",
  occupation = "직업 없음",
  age = "알 수 없음",
}) {
  return (
    <div
      style={{
        border:
          "1px solid #007bff",
        padding: "15px",
        margin: "10px",
      }}
    >
      <h3>{name}</h3>

      <p>
        직업: {occupation}
      </p>

      <p>
        나이: {age}
      </p>
    </div>
  );
}
```

사용:

```jsx
<ProfileCard
  name="김철수"
  occupation="개발자"
  age={28}
/>

<ProfileCard
  name="이영희"
  occupation="디자이너"
/>

<ProfileCard />
```

전달하지 않은 Props에는 기본값이 적용됩니다.

---

## 4. `children` Props

`children`은 컴포넌트의 여는 태그와 닫는 태그 사이에 작성된 내용을 의미합니다.

```jsx
<Component>
  내용
</Component>
```

태그 사이의 내용은 자동으로 다음 값으로 전달됩니다.

```javascript
props.children
```

기본 구조:

```jsx
function Component({
  children,
}) {
  return (
    <div>
      {children}
    </div>
  );
}
```

`children`에는 다음과 같은 내용을 전달할 수 있습니다.

- 텍스트
- JSX 요소
- 다른 컴포넌트
- JavaScript 표현식

---

## 5. 재사용 가능한 Wrapper 컴포넌트

`children`은 버튼, 카드, 레이아웃처럼 내부 콘텐츠가 달라지는 컴포넌트를 만들 때 사용할 수 있습니다.

### Button

```jsx
function Button({
  children,
}) {
  return (
    <button>
      {children}
    </button>
  );
}
```

사용:

```jsx
<Button>확인</Button>

<Button>
  <span>▶</span>
  시작하기
</Button>
```

### Highlight

```jsx
function Highlight({
  children,
}) {
  return (
    <div
      style={{
        backgroundColor:
          "yellow",
      }}
    >
      {children}
    </div>
  );
}
```

사용:

```jsx
<Highlight>
  <strong>안내: </strong>
  리액트 프로그래밍 204호실
</Highlight>
```

---

## 6. Layout과 `children`

공통 레이아웃을 유지하면서 가운데 콘텐츠만 변경할 수 있습니다.

```jsx
function Layout({
  children,
}) {
  return (
    <div>
      <header>
        프론트앤드 프로그래밍
      </header>

      <main>
        {children}
      </main>

      <footer>
        © 2025 ~
      </footer>
    </div>
  );
}
```

사용:

```jsx
function App() {
  return (
    <Layout>
      <h1>홈 화면</h1>

      <p>
        children으로
        전달된 내용입니다.
      </p>
    </Layout>
  );
}
```

구조:

```text
Layout
├── Header
├── children
└── Footer
```

---

## 7. State

**State**는 React 컴포넌트 내부에서 관리하는 동적인 데이터입니다.

다음과 같은 상황에서 값이 변할 수 있습니다.

- 사용자 입력
- 버튼 클릭
- 시간의 흐름
- 애플리케이션의 상호작용

State가 변경되면 컴포넌트는 다시 렌더링됩니다.

렌더링이나 데이터 흐름에 필요한 값을 State로 관리합니다.

---

## 8. Props와 State 비교

| 구분 | Props | State |
|---|---|---|
| 역할 | 부모로부터 전달받는 데이터 | 컴포넌트 내부에서 관리되는 데이터 |
| 변경 여부 | 읽기 전용 | 변경 가능 |
| 예시 | `<Welcome name="지수" />` | `useState(0)` |

Props는 외부에서 전달받고, State는 해당 컴포넌트 내부에서 관리합니다.

---

## 9. `useState`

함수형 컴포넌트에서는 `useState` Hook을 사용하여 상태를 관리합니다.

```jsx
import React, {
  useState,
} from "react";
```

기본 문법:

```jsx
const [
  변수명,
  set변수명,
] = useState(초기값);
```

예시:

```jsx
function Counter() {
  const [
    count,
    setCount,
  ] = useState(0);

  return (
    <div>
      <h2>
        현재 숫자: {count}
      </h2>

      <button
        onClick={() =>
          setCount(
            prevCount =>
              prevCount + 1
          )
        }
      >
        +1 증가
      </button>
    </div>
  );
}
```

---

## 10. 자식 컴포넌트에서 부모 State 변경

자식 컴포넌트가 부모 컴포넌트의 State를 직접 변경할 수는 없습니다.

대신 부모 컴포넌트가 State를 변경하는 함수를 Props로 전달할 수 있습니다.

```text
부모 State
    ↓
부모의 상태 변경 함수
    ↓
Props로 함수 전달
    ↓
자식 컴포넌트
    ↓
함수 호출
    ↓
부모 State 변경
```

자식 컴포넌트:

```jsx
function ChildCounter({
  increment,
}) {
  return (
    <button
      onClick={increment}
    >
      증가
    </button>
  );
}
```

부모 컴포넌트:

```jsx
function ParentCounter() {
  const [
    count,
    setCount,
  ] = useState(0);

  const increment = () => {
    setCount(
      prevCount =>
        prevCount + 1
    );
  };

  return (
    <div>
      <h1>
        카운트: {count}
      </h1>

      <ChildCounter
        increment={increment}
      />
    </div>
  );
}
```

---

## 11. State 응용

State 값에 따라 출력값뿐 아니라 UI 속성도 제어할 수 있습니다.

예를 들어 좋아요 수를 증가시키고 일정 값을 넘으면 버튼을 비활성화할 수 있습니다.

```jsx
function LikeButton() {
  const [
    likes,
    setLikes,
  ] = useState(0);

  return (
    <div>
      <p>
        좋아요: {likes}
      </p>

      <button
        disabled={likes > 10}
        onClick={() =>
          setLikes(
            prev => prev + 1
          )
        }
      >
        좋아요
      </button>
    </div>
  );
}
```

---

## 12. React 이벤트 핸들링

React는 브라우저마다 다른 이벤트 처리 방식을 직접 사용하지 않고 React의 이벤트 시스템을 사용합니다.

이벤트 이름은 **camelCase**로 작성합니다.

HTML:

```html
<button onclick="handleClick()">
  클릭
</button>
```

React JSX:

```jsx
<button onClick={handleClick}>
  클릭
</button>
```

이벤트에는 함수 자체를 전달합니다.

```jsx
onClick={handleClick}
```

다음과 같이 작성하면 함수를 즉시 호출하게 됩니다.

```jsx
onClick={handleClick()}
```

---

## 13. 매개변수가 없는 이벤트 핸들러

```jsx
function BasicClickExample() {
  const [
    message,
    setMessage,
  ] = useState(
    "버튼을 클릭하기 전입니다."
  );

  const handleBtnClick = () => {
    setMessage(
      "버튼이 클릭되었습니다!"
    );
  };

  return (
    <div>
      <p>{message}</p>

      <button
        onClick={handleBtnClick}
      >
        클릭하세요
      </button>
    </div>
  );
}
```

---

## 14. 이벤트 핸들러에 매개변수 전달

추가적인 값을 전달해야 할 경우 화살표 함수를 사용합니다.

```jsx
function ButtonLogger() {
  const [
    log,
    setLog,
  ] = useState("");

  const handleButtonClick =
    buttonName => {
      setLog(
        `${buttonName} 클릭함!`
      );
    };

  return (
    <div>
      <h2>
        로그: {log}
      </h2>

      <button
        onClick={() =>
          handleButtonClick(
            "버튼 1"
          )
        }
      >
        버튼 1
      </button>

      <button
        onClick={() =>
          handleButtonClick(
            "버튼 2"
          )
        }
      >
        버튼 2
      </button>
    </div>
  );
}
```

---

## 15. 이벤트 객체

React는 이벤트 핸들러에 이벤트 객체를 자동으로 전달합니다.

```jsx
function InputChangeExample() {
  const [
    inputValue,
    setInputValue,
  ] = useState("");

  const handleInputChange =
    e => {
      setInputValue(
        e.target.value
      );
    };

  return (
    <div>
      <p>
        입력된 텍스트:
        {inputValue}
      </p>

      <input
        type="text"
        onChange={
          handleInputChange
        }
      />
    </div>
  );
}
```

`onChange`에 함수 이름만 전달하면 React가 이벤트 객체를 자동으로 전달합니다.

---

## 16. 매개변수와 이벤트 객체 함께 전달

사용자 정의 매개변수와 이벤트 객체를 동시에 전달할 수 있습니다.

```jsx
function ProductList() {
  const handlePurchase =
    (productId, event) => {
      console.log(
        `상품 ID: ${productId}`
      );

      console.log(
        `X=${event.clientX}, Y=${event.clientY}`
      );
    };

  return (
    <div>
      <h3>
        구매 가능한 상품
      </h3>

      <button
        onClick={e =>
          handlePurchase(
            "product-1",
            e
          )
        }
      >
        상품 1 구매
      </button>

      <button
        onClick={e =>
          handlePurchase(
            "product-2",
            e
          )
        }
      >
        상품 2 구매
      </button>
    </div>
  );
}
```

---

## 17. SyntheticEvent

React의 이벤트 객체를 **SyntheticEvent(합성 이벤트)**라고 합니다.

브라우저의 Native Event를 감싸서 브라우저 간에 동일한 인터페이스를 사용할 수 있도록 합니다.

| 속성 / 메소드 | 설명 |
|---|---|
| `event.type` | `click`, `change` 등의 이벤트 유형 |
| `event.target` | 이벤트가 실제 발생한 DOM 요소 |
| `event.currentTarget` | 이벤트 핸들러가 연결된 DOM 요소 |
| `event.preventDefault()` | 이벤트의 기본 동작 방지 |
| `event.stopPropagation()` | 이벤트 전파 중단 |

---

## 18. 이벤트 유형별 주요 속성

### Mouse Event

```javascript
event.clientX
event.clientY
event.screenX
event.screenY
event.button
```

### Keyboard Event

```javascript
event.key
event.code
event.keyCode
```

### Form Event

```javascript
event.target.value
event.target.checked
```

### Touch Event

```javascript
event.touches
```

### Clipboard Event

```javascript
event.clipboardData
```

---

## 19. Controlled Component

사용자의 `<input>`, `<textarea>`, `<select>` 등의 입력값을 React State에서 직접 관리하는 방식을 **Controlled Component**라고 합니다.

기본 흐름:

```text
사용자 입력
    ↓
onChange
    ↓
이벤트 핸들러
    ↓
State 변경
    ↓
재렌더링
```

예시:

```jsx
function App() {
  const [
    text,
    setText,
  ] = useState("");

  const handleChange = e => {
    setText(
      e.target.value
    );
  };

  return (
    <div>
      <input
        value={text}
        onChange={handleChange}
      />

      <p>
        입력값: {text}
      </p>
    </div>
  );
}
```

---

## 20. 각각의 State로 폼 관리

각 입력 필드를 별도의 State로 관리할 수 있습니다.

```jsx
function SignUpForm() {
  const [
    name,
    setName,
  ] = useState("");

  const [
    email,
    setEmail,
  ] = useState("");

  const handleNameChange =
    e => {
      setName(
        e.target.value
      );
    };

  const handleEmailChange =
    e => {
      setEmail(
        e.target.value
      );
    };

  const handleSubmit = e => {
    e.preventDefault();

    alert(
      `가입 정보\n이름: ${name}\n이메일: ${email}`
    );
  };

  return (
    <form
      onSubmit={handleSubmit}
    >
      <h2>회원가입</h2>

      <label>
        이름:

        <input
          type="text"
          value={name}
          onChange={
            handleNameChange
          }
        />
      </label>

      <label>
        이메일:

        <input
          type="email"
          value={email}
          onChange={
            handleEmailChange
          }
        />
      </label>

      <h3>입력된 정보</h3>

      <p>이름: {name}</p>
      <p>이메일: {email}</p>

      <button type="submit">
        가입하기
      </button>
    </form>
  );
}
```

---

## 21. 하나의 객체 State로 폼 관리

여러 입력 필드를 하나의 객체 State로 관리할 수도 있습니다.

```jsx
function SignUpForm() {
  const [
    userInfo,
    setUserInfo,
  ] = useState({
    name: "",
    email: "",
  });

  const handleChange = e => {
    const {
      name,
      value,
    } = e.target;

    setUserInfo(
      prevInfo => ({
        ...prevInfo,
        [name]: value,
      })
    );
  };

  const handleSubmit = e => {
    e.preventDefault();

    alert(
      `가입 정보\n이름: ${userInfo.name}\n이메일: ${userInfo.email}`
    );
  };

  return (
    <form
      onSubmit={handleSubmit}
    >
      <label>
        이름:

        <input
          type="text"
          name="name"
          value={userInfo.name}
          onChange={handleChange}
        />
      </label>

      <label>
        이메일:

        <input
          type="email"
          name="email"
          value={userInfo.email}
          onChange={handleChange}
        />
      </label>

      <p>
        이름: {userInfo.name}
      </p>

      <p>
        이메일: {userInfo.email}
      </p>

      <button type="submit">
        가입하기
      </button>
    </form>
  );
}
```

핵심 State 변경 방식:

```jsx
setUserInfo(
  prevInfo => ({
    ...prevInfo,
    [name]: value,
  })
);
```

기존 객체를 Spread Syntax로 복사한 뒤 변경된 입력 필드만 갱신합니다.

---

## 22. `preventDefault()`

HTML의 `<form>`을 제출하면 기본적으로 브라우저의 폼 제출 동작이 실행됩니다.

이 기본 동작을 막기 위해 다음 메소드를 사용합니다.

```javascript
event.preventDefault();
```

예시:

```jsx
const handleSubmit = e => {
  e.preventDefault();

  console.log(
    "폼 제출 완료"
  );
};
```

---

## 23. Feedback Form 실습

사용자에게 다음 정보를 입력받는 폼을 작성합니다.

- 성별
- 나이
- 이메일
- 제품 사용 후기
- 전송 버튼

전송 버튼을 클릭하면 메시지를 출력합니다.

```jsx
const handleSubmit = () => {
  alert(
    "감사합니다. 후기 쿠폰은 이메일로 발송합니다."
  );
};
```

이후 여러 입력 필드를 하나의 객체 State로 관리하는 방식으로 확장합니다.

---

## 24. 실시간 글자 수 카운터 실습

마지막 실습에서는 State와 이벤트 핸들링을 함께 사용합니다.

조건:

- `<textarea>`에 입력할 때마다 State 업데이트
- 현재 입력된 글자 수 실시간 표시
- 최대 20자까지 입력
- 20자를 넘으면 추가 입력 제한
- 20자에 도달하면 글자 수 카운터의 스타일 변경

주요 흐름:

```text
onChange
   ↓
Event Object
   ↓
State 변경
   ↓
글자 수 계산
   ↓
조건부 스타일 적용
```

</details>
