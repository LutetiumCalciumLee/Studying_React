<details>
<summary>ENG (English Version)</summary>

# JSX & Components


## 1. What is JSX?

**JSX (JavaScript + XML)** is a syntax used inside JavaScript that looks similar to HTML.

React converts JSX into JavaScript to construct the user interface.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}
```

---

## 2. JSX Syntax

### Single Parent Element

JSX requires a single top-level element.

Incorrect:

```jsx
return (
  <h1>Hello World!</h1>
  <h2>Welcome!</h2>
);
```

Correct:

```jsx
return (
  <div>
    <h1>Hello World!</h1>
    <h2>Welcome!</h2>
  </div>
);
```

---

## 3. JavaScript Expressions in JSX

JavaScript expressions can be inserted into JSX using `{}`.

```jsx
function App() {
  const name = "Gildong";

  return (
    <div>
      <h1>{name}'s Web Page</h1>
      <h2>Hello World!</h2>
    </div>
  );
}
```

Functions can also be called inside `{}`.

```jsx
function App() {
  function userName(name) {
    return name;
  }

  return (
    <div>
      <h1>
        {userName("Gildong")}'s Web Page
      </h1>
    </div>
  );
}
```

JSX uses JavaScript **expressions**, while statements such as `if` and loops cannot be written directly inside JSX expressions.

---

## 4. JSX Attributes

JSX attributes are similar to HTML attributes, but some attribute names are different.

```text
HTML          JSX
class         className
for           htmlFor
```

Example:

```jsx
function App() {
  const name = "Gildong";

  return (
    <div className="App">
      <h1>{name}, welcome!</h1>

      <label htmlFor="input">
        Name:
      </label>

      <input id="input" />
    </div>
  );
}
```

---

## 5. JSX Comments

Comments inside JSX are written using JavaScript comment syntax wrapped in `{}`.

```jsx
function CommentExample() {
  return (
    <div>
      {/* JSX comment */}

      <h1>Title</h1>

      {/*
        Multi-line
        JSX comment
      */}

      <p>Paragraph</p>
    </div>
  );
}
```

---

## 6. Variables, Functions, and Objects in JSX

JavaScript variables and object properties can be used directly inside JSX.

```jsx
function App() {
  const name = "React";

  const naver = {
    name: "naver",
    url: "http://www.naver.com",
  };

  return (
    <div className="App">
      <h1>Hello, {name}!!</h1>
      <h1>Welcome, {name}!!</h1>

      <h2>
        <a href={naver.url}>
          {naver.name}
        </a>
      </h2>
    </div>
  );
}
```

Functions can also be used to generate values.

```jsx
function App() {
  function formatName(user) {
    return user.firstName + user.lastName;
  }

  const user = {
    firstName: "Paring ",
    lastName: "Yang",
  };

  return (
    <h1>
      Hello, {formatName(user)}
    </h1>
  );
}
```

---

## 7. Styling in JSX

The JSX `style` attribute uses a JavaScript object.

CSS property names are written in **camelCase**.

```jsx
const style = {
  backgroundColor: "yellow",
  color: "red",
  fontSize: "24px",
};

function App() {
  return (
    <h1 style={style}>
      Hello, React!
    </h1>
  );
}
```

Inline styles can also be written directly.

```jsx
<h1
  style={{
    backgroundColor: "yellow",
    color: "red",
  }}
>
  Hello, React!
</h1>
```

### External CSS

Styles can also be defined in a separate CSS file.

```css
.Title1 {
  background-color: black;
  color: yellow;
}
```

```jsx
<h1 className="Title1">
  Hello, React!
</h1>
```

---

## 8. Conditional Rendering

JSX can render different UI elements depending on a condition.

Because `if` is a statement rather than an expression, JSX commonly uses:

- Ternary operator
- Logical AND `&&`
- Logical OR `||`

### Ternary Operator

```jsx
function App() {
  const id = "admin";

  return (
    <div>
      <h1>User Status</h1>

      {id === "admin" ? (
        <h2>
          Logged in as {id}
        </h2>
      ) : (
        <h2>
          General account login
        </h2>
      )}
    </div>
  );
}
```

---

## 9. Conditional Rendering with `&&`

The logical AND operator can render an element only when a condition is true.

```jsx
function App() {
  const id = "admin";

  return (
    <div>
      {id === "admin" && (
        <h1>
          {id} logged in
        </h1>
      )}
    </div>
  );
}
```

React does not render values such as `false`, `null`, or `undefined` as UI content.

---

## 10. Default Values with `||`

The logical OR operator can provide a fallback value.

```jsx
function App() {
  const name = undefined;

  return (
    <div>
      <h2>
        Hello, {name || "Guest"}!
      </h2>

      <p>Profile Page</p>
    </div>
  );
}
```

---

## 11. Conditional Rendering Example

```jsx
function WarningBanner(props) {
  const showWarning =
    props.showWarning;

  return (
    <div>
      {showWarning && (
        <p
          style={{
            color: "red",
            border:
              "1px solid red",
            padding: "10px",
          }}
        >
          Warning! Data will be deleted.
        </p>
      )}

      <button
        onClick={() =>
          alert("Button clicked!")
        }
      >
        Confirm
      </button>
    </div>
  );
}
```

Usage:

```jsx
function App() {
  return (
    <>
      <WarningBanner
        showWarning={true}
      />

      <WarningBanner
        showWarning={false}
      />
    </>
  );
}
```

---

## 12. Conditional Rendering with Arrays

Conditional rendering can also be combined with `map()`.

```jsx
function ItemList(props) {
  const items = props.items;

  return (
    <div>
      <h2>Shopping List</h2>

      {items.length > 0 && (
        <ul>
          {items.map(
            (item, index) => (
              <li key={index}>
                {item}
              </li>
            )
          )}
        </ul>
      )}

      {items.length === 0 && (
        <p>
          The shopping cart is empty.
        </p>
      )}
    </div>
  );
}
```

---

## 13. What is a React Component?

A **component** is a basic unit used to construct a React user interface.

Examples include:

- Buttons
- Input fields
- Menus
- Cards
- Sections of a page

Instead of writing all UI code inside `App.js`, an application can be divided into independent and reusable components.

Main characteristics:

- Reusability
- Independence
- Encapsulation
- Composability
- Easier maintenance

Small components can be combined to create larger UI structures.

---

## 14. Creating a Component

Component names must begin with an uppercase letter.

```text
HTML Element
<div>

React Component
<Menu />
```

Example:

```jsx
const Menu = () => {
  return (
    <div>
      <h1>Americano</h1>
      <p>3500</p>
    </div>
  );
};

export default Menu;
```

The component can then be imported into `App.js`.

```jsx
import Menu from "./component/Menu";

function App() {
  return (
    <div>
      <Menu />
    </div>
  );
}
```

Basic component workflow:

```text
Create Component
      ↓
export
      ↓
import in App.js
      ↓
Render Component
```

---

## 15. Types of React Components

React components can be written in two forms:

```text
React Components
├── Functional Components
└── Class Components
```

---

## 16. Class Components

Class components were commonly used in earlier React development.

They use ES6 class syntax and extend `React.Component`.

```jsx
import React from "react";

class Counter
  extends React.Component {

  constructor(props) {
    super(props);

    this.state = {
      count: 0,
    };
  }

  render() {
    return (
      <div>
        <p>
          Count:
          {this.state.count}
        </p>

        <button
          onClick={() =>
            this.setState({
              count:
                this.state.count + 1,
            })
          }
        >
          Increase
        </button>
      </div>
    );
  }
}

export default Counter;
```

Class components can manage:

- State
- Lifecycle methods
- Rendering

---

## 17. Component Lifecycle

A component lifecycle describes the process through which a component is:

```text
Mounting
   ↓
Updating
   ↓
Unmounting
```

Lifecycle methods introduced in this branch include:

```text
Mounting
componentDidMount()

Updating
componentDidUpdate()

Unmounting
componentWillUnmount()
```

A component is created, rendered, updated when its data changes, and eventually removed.

---

## 18. State

**State** represents changeable data inside a React component.

- State is a JavaScript object
- It stores data that can change
- It is defined by the component developer
- A state change causes the component to render again

Only values related to rendering or application data flow should be stored in state.

---

## 19. Functional Components

Functional components are written as JavaScript functions.

They receive `props` as an argument and return React elements.

```jsx
function HelloMessage(props) {
  return (
    <h1>
      Hello, {props.name}!
    </h1>
  );
}
```

Usage:

```jsx
<HelloMessage name="Gildong" />
```

React Hooks allow functional components to use features such as state and lifecycle handling.

Examples introduced in this branch include:

```text
useState
useEffect
```

---

## 20. Functional Component Syntax

Function syntax:

```jsx
import React from "react";

function Menu() {
  return (
    <div>
      <h1>Americano</h1>
      <p>3500</p>
    </div>
  );
}

export default Menu;
```

Arrow function syntax:

```jsx
import React from "react";

const Menu = () => {
  return (
    <div>
      <h1>Americano</h1>
      <p>3500</p>
    </div>
  );
};

export default Menu;
```

---

## 21. Component Design

Repeated UI structures can be separated into reusable components.

Instead of repeatedly writing:

```jsx
<div>
  <h1>Americano</h1>
  <p>3500</p>
</div>

<div>
  <h1>Americano</h1>
  <p>3500</p>
</div>
```

A reusable component can be created.

```jsx
const Menu = () => {
  const style = {
    border:
      "1px solid gray",
  };

  return (
    <div style={style}>
      <h1>Americano</h1>
      <p>3500</p>
    </div>
  );
};
```

Then it can be rendered as:

```jsx
<Menu />
```

This reduces duplicated UI code and improves maintainability.

---

## 22. Props

**Props** is short for **properties**.

Props are used to pass data from a parent component to a child component.

```text
Parent Component
      ↓
    props
      ↓
Child Component
```

Props are **read-only** and cannot be modified by the child component.

Example:

```jsx
function Welcome(props) {
  return (
    <h2>
      Hello, {props.name}!
    </h2>
  );
}
```

Parent component:

```jsx
<Welcome name="Chulsoo" />
<Welcome name="Younghee" />
```

When React renders:

```jsx
<Welcome name="Chulsoo" />
```

the component receives an object similar to:

```javascript
{
  name: "Chulsoo"
}
```

---

## 23. Reusing Components with Props

A single component can display different data through props.

```jsx
function UserCard(props) {
  return (
    <div
      style={{
        border:
          "1px solid gray",
        padding: "15px",
        margin: "10px",
      }}
    >
      <h3>
        Name: {props.name}
      </h3>

      <p>
        Age: {props.age}
      </p>

      <p>
        Job: {props.job}
      </p>
    </div>
  );
}
```

Usage:

```jsx
function App() {
  return (
    <>
      <UserCard
        name="Ann"
        age={28}
        job="Developer"
      />

      <UserCard
        name="Minsu"
        age={31}
        job="Designer"
      />

      <UserCard
        name="John"
        age={28}
        job="Marketer"
      />
    </>
  );
}
```

The same component structure can therefore be reused with different data.

---

## 24. Coffee Item Example

```jsx
const CoffeeItem = props => {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>{props.price} won</p>
    </div>
  );
};

export default CoffeeItem;
```

Usage:

```jsx
<CoffeeItem
  name="Americano"
  price={3000}
/>

<CoffeeItem
  name="Cafe Latte"
  price={3500}
/>

<CoffeeItem
  name="Cappuccino"
  price={3500}
/>
```

JavaScript values such as numbers are passed through `{}`.

```jsx
price={3000}
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# JSX & Components


## 1. JSX란?

**JSX(JavaScript + XML)**는 HTML과 비슷한 형태를 가지지만 JavaScript 내부에서 사용하는 문법입니다.

React는 JSX 코드를 JavaScript로 변환하여 UI를 구성합니다.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}
```

---

## 2. JSX 기본 문법

### 하나의 최상위 요소

JSX에서는 여러 요소를 반환할 경우 하나의 부모 요소로 감싸야 합니다.

잘못된 예:

```jsx
return (
  <h1>Hello World!</h1>
  <h2>Welcome!</h2>
);
```

올바른 예:

```jsx
return (
  <div>
    <h1>Hello World!</h1>
    <h2>Welcome!</h2>
  </div>
);
```

---

## 3. JSX에서 JavaScript 표현식 사용

JSX 안에서는 `{}`를 사용하여 JavaScript 표현식을 작성합니다.

```jsx
function App() {
  const name = "홍길동";

  return (
    <div>
      <h1>
        {name}님의 웹페이지입니다.
      </h1>

      <h2>Hello World!</h2>
    </div>
  );
}
```

함수도 호출할 수 있습니다.

```jsx
function App() {
  function userName(name) {
    return name;
  }

  return (
    <div>
      <h1>
        {userName("홍길동")}님의
        웹페이지입니다.
      </h1>
    </div>
  );
}
```

JSX 내부에서는 JavaScript **표현식**을 사용하며 `if`문이나 반복문과 같은 statement를 직접 작성하지 않습니다.

---

## 4. JSX 속성

JSX에서도 HTML과 비슷하게 속성을 지정할 수 있지만 일부 속성 이름이 다릅니다.

```text
HTML          JSX
class         className
for           htmlFor
```

예시:

```jsx
function App() {
  const name = "홍길동";

  return (
    <div className="App">
      <h1>
        {name}님 반갑습니다.
      </h1>

      <label htmlFor="input">
        이름:
      </label>

      <input id="input" />
    </div>
  );
}
```

---

## 5. JSX 주석

JSX 내부의 주석은 `{}` 안에 JavaScript 주석 문법을 사용합니다.

```jsx
function CommentExample() {
  return (
    <div>
      {/* JSX 내부의 주석 */}

      <h1>제목</h1>

      {/*
        여러 줄
        JSX 주석
      */}

      <p>단락입니다.</p>
    </div>
  );
}
```

---

## 6. JSX에서 변수, 함수, 객체 사용

JavaScript 변수나 객체의 속성을 JSX 안에서 사용할 수 있습니다.

```jsx
function App() {
  const name = "React";

  const naver = {
    name: "naver",
    url: "http://www.naver.com",
  };

  return (
    <div className="App">
      <h1>Hello, {name}!!</h1>
      <h1>Welcome, {name}!!</h1>

      <h2>
        <a href={naver.url}>
          {naver.name}
        </a>
      </h2>
    </div>
  );
}
```

함수의 반환값 역시 JSX에서 사용할 수 있습니다.

```jsx
function App() {
  function formatName(user) {
    return (
      user.firstName +
      user.lastName
    );
  }

  const user = {
    firstName: "Paring ",
    lastName: "Yang",
  };

  return (
    <h1>
      Hello, {formatName(user)}
    </h1>
  );
}
```

---

## 7. JSX 스타일 적용

JSX의 `style` 속성은 JavaScript 객체 형태로 작성합니다.

CSS 속성명은 **camelCase**를 사용합니다.

```jsx
const style = {
  backgroundColor: "yellow",
  color: "red",
  fontSize: "24px",
};

function App() {
  return (
    <h1 style={style}>
      Hello, React!
    </h1>
  );
}
```

직접 작성할 수도 있습니다.

```jsx
<h1
  style={{
    backgroundColor: "yellow",
    color: "red",
  }}
>
  Hello, React!
</h1>
```

### 외부 CSS

`App.css` 등의 외부 CSS 파일을 사용할 수도 있습니다.

```css
.Title1 {
  background-color: black;
  color: yellow;
}
```

```jsx
<h1 className="Title1">
  Hello, React!
</h1>
```

---

## 8. 조건부 렌더링

조건에 따라 서로 다른 UI를 렌더링할 수 있습니다.

JSX에서는 `if`문 대신 주로 다음 표현식을 사용합니다.

- 삼항 연산자
- 논리 AND 연산자 `&&`
- 논리 OR 연산자 `||`

### 삼항 연산자

```jsx
function App() {
  const id = "admin";

  return (
    <div>
      <h1>사용자 상태</h1>

      {id === "admin" ? (
        <h2>
          {id} 계정으로 로그인
        </h2>
      ) : (
        <h2>
          일반 계정 로그인
        </h2>
      )}
    </div>
  );
}
```

---

## 9. `&&`를 이용한 조건부 렌더링

조건이 true인 경우에만 특정 요소를 렌더링할 수 있습니다.

```jsx
function App() {
  const id = "admin";

  return (
    <div>
      {id === "admin" && (
        <h1>
          {id}님 로그인
        </h1>
      )}
    </div>
  );
}
```

React에서는 `false`, `null`, `undefined`와 같은 값은 UI 요소로 렌더링되지 않습니다.

---

## 10. `||`를 이용한 기본값 지정

데이터가 없는 경우 기본값을 출력할 때 `||`를 사용할 수 있습니다.

```jsx
function App() {
  const name = undefined;

  return (
    <div>
      <h2>
        안녕하세요,
        {name || "게스트"}님!
      </h2>

      <p>프로필 페이지입니다.</p>
    </div>
  );
}
```

---

## 11. 조건부 렌더링 예제

```jsx
function WarningBanner(props) {
  const showWarning =
    props.showWarning;

  return (
    <div>
      {showWarning && (
        <p
          style={{
            color: "red",
            border:
              "1px solid red",
            padding: "10px",
          }}
        >
          경고! 데이터가
          삭제됩니다.
        </p>
      )}

      <button
        onClick={() =>
          alert("버튼 클릭!")
        }
      >
        확인
      </button>
    </div>
  );
}
```

사용:

```jsx
function App() {
  return (
    <>
      <WarningBanner
        showWarning={true}
      />

      <WarningBanner
        showWarning={false}
      />
    </>
  );
}
```

---

## 12. 배열과 조건부 렌더링

`map()`과 조건식을 조합하여 목록을 렌더링할 수 있습니다.

```jsx
function ItemList(props) {
  const items = props.items;

  return (
    <div>
      <h2>내 쇼핑 리스트</h2>

      {items.length > 0 && (
        <ul>
          {items.map(
            (item, index) => (
              <li key={index}>
                {item}
              </li>
            )
          )}
        </ul>
      )}

      {items.length === 0 && (
        <p>
          장바구니가 비어있습니다.
        </p>
      )}
    </div>
  );
}
```

---

## 13. React 컴포넌트란?

**컴포넌트(Component)**는 React에서 UI를 구성하는 기본 단위입니다.

예를 들어 다음과 같은 UI 요소를 컴포넌트로 만들 수 있습니다.

- 버튼
- 입력창
- 메뉴
- 카드
- 화면의 특정 영역

모든 코드를 `App.js` 하나에 작성하는 대신 독립적이고 재사용 가능한 UI 조각으로 분리합니다.

주요 특징:

- 재사용성
- 독립성 및 캡슐화
- 조합 가능성
- 유지보수 용이
- 선언적인 UI 구성

작은 컴포넌트를 조합하여 더 큰 컴포넌트를 만들 수 있습니다.

---

## 14. 컴포넌트 생성

컴포넌트 이름은 반드시 **대문자로 시작**합니다.

```text
HTML 요소
<div>

React 컴포넌트
<Menu />
```

예시:

```jsx
const Menu = () => {
  return (
    <div>
      <h1>아메리카노</h1>
      <p>3500원</p>
    </div>
  );
};

export default Menu;
```

`App.js`에서 가져와 사용합니다.

```jsx
import Menu from "./component/Menu";

function App() {
  return (
    <div>
      <Menu />
    </div>
  );
}
```

전체 과정:

```text
컴포넌트 생성
    ↓
export
    ↓
App.js에서 import
    ↓
컴포넌트 렌더링
```

---

## 15. 컴포넌트 종류

React 컴포넌트는 크게 두 가지 형태로 작성할 수 있습니다.

```text
React Component
├── 함수형 컴포넌트
└── 클래스형 컴포넌트
```

---

## 16. 클래스형 컴포넌트

클래스형 컴포넌트는 React 초기에 주로 사용되던 방식입니다.

ES6의 `class` 문법을 사용하고 `React.Component`를 상속받습니다.

```jsx
import React from "react";

class Counter
  extends React.Component {

  constructor(props) {
    super(props);

    this.state = {
      count: 0,
    };
  }

  render() {
    return (
      <div>
        <p>
          카운트:
          {this.state.count}
        </p>

        <button
          onClick={() =>
            this.setState({
              count:
                this.state.count + 1,
            })
          }
        >
          증가
        </button>
      </div>
    );
  }
}

export default Counter;
```

클래스형 컴포넌트에서는 다음 기능을 사용할 수 있습니다.

- State
- Lifecycle Method
- `render()`

---

## 17. 컴포넌트 생명주기

컴포넌트 생명주기란 컴포넌트가 생성되고, 변경되고, 제거되는 과정을 의미합니다.

```text
Mounting
   ↓
Updating
   ↓
Unmounting
```

이 Branch에서 다룬 주요 Lifecycle Method:

```text
Mounting
componentDidMount()

Updating
componentDidUpdate()

Unmounting
componentWillUnmount()
```

컴포넌트가 생성되어 렌더링된 뒤 데이터 변화에 따라 업데이트되고 최종적으로 화면에서 제거됩니다.

---

## 18. State

**State**는 React 컴포넌트 내부에서 변경 가능한 데이터를 의미합니다.

- JavaScript 객체
- 변경 가능한 데이터 관리
- 컴포넌트 개발자가 직접 정의
- State가 변경되면 컴포넌트가 다시 렌더링됨

렌더링이나 데이터 흐름에 필요한 값만 State에 포함합니다.

---

## 19. 함수형 컴포넌트

함수형 컴포넌트는 JavaScript 함수 형태로 작성합니다.

`props`를 인자로 받고 React 요소를 반환합니다.

```jsx
function HelloMessage(props) {
  return (
    <h1>
      안녕하세요,
      {props.name}님!
    </h1>
  );
}
```

사용:

```jsx
<HelloMessage name="홍길동" />
```

React의 Hook을 통해 함수형 컴포넌트에서도 State와 생명주기 관련 기능을 사용할 수 있습니다.

이 Branch에서 소개한 Hook의 예:

```text
useState
useEffect
```

---

## 20. 함수형 컴포넌트 문법

일반 함수:

```jsx
import React from "react";

function Menu() {
  return (
    <div>
      <h1>아메리카노</h1>
      <p>3500원</p>
    </div>
  );
}

export default Menu;
```

화살표 함수:

```jsx
import React from "react";

const Menu = () => {
  return (
    <div>
      <h1>아메리카노</h1>
      <p>3500원</p>
    </div>
  );
};

export default Menu;
```

---

## 21. 컴포넌트 설계

반복되는 UI를 컴포넌트로 분리하여 재사용할 수 있습니다.

기존 방식:

```jsx
<div>
  <h1>아메리카노</h1>
  <p>3500원</p>
</div>

<div>
  <h1>아메리카노</h1>
  <p>3500원</p>
</div>
```

컴포넌트화:

```jsx
const Menu = () => {
  const style = {
    border:
      "1px solid gray",
  };

  return (
    <div style={style}>
      <h1>아메리카노</h1>
      <p>3500원</p>
    </div>
  );
};
```

사용:

```jsx
<Menu />
```

반복되는 UI 코드를 컴포넌트화하면 코드 중복을 줄이고 유지보수를 쉽게 할 수 있습니다.

---

## 22. Props

**Props**는 **properties**의 줄임말입니다.

부모 컴포넌트가 자식 컴포넌트에 데이터를 전달할 때 사용하는 객체입니다.

```text
부모 컴포넌트
      ↓
    props
      ↓
자식 컴포넌트
```

Props는 **읽기 전용**이며 자식 컴포넌트 내부에서 변경할 수 없습니다.

```jsx
function Welcome(props) {
  return (
    <h2>
      안녕하세요,
      {props.name}님!
    </h2>
  );
}
```

부모 컴포넌트:

```jsx
<Welcome name="철수" />
<Welcome name="영희" />
```

다음과 같이 컴포넌트를 호출하면:

```jsx
<Welcome name="철수" />
```

React에서는 다음과 같은 props 객체가 전달됩니다.

```javascript
{
  name: "철수"
}
```

---

## 23. Props를 이용한 컴포넌트 재사용

하나의 컴포넌트에 서로 다른 데이터를 전달할 수 있습니다.

```jsx
function UserCard(props) {
  return (
    <div
      style={{
        border:
          "1px solid gray",
        padding: "15px",
        margin: "10px",
      }}
    >
      <h3>
        이름: {props.name}
      </h3>

      <p>
        나이: {props.age}
      </p>

      <p>
        직업: {props.job}
      </p>
    </div>
  );
}
```

사용:

```jsx
function App() {
  return (
    <>
      <UserCard
        name="Ann"
        age={28}
        job="개발자"
      />

      <UserCard
        name="Minsu"
        age={31}
        job="디자이너"
      />

      <UserCard
        name="John"
        age={28}
        job="마케터"
      />
    </>
  );
}
```

동일한 컴포넌트 구조를 유지하면서 서로 다른 데이터를 출력할 수 있습니다.

---

## 24. CoffeeItem 컴포넌트 실습

```jsx
const CoffeeItem = props => {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>{props.price}원</p>
    </div>
  );
};

export default CoffeeItem;
```

사용:

```jsx
<CoffeeItem
  name="아메리카노"
  price={3000}
/>

<CoffeeItem
  name="카페라떼"
  price={3500}
/>

<CoffeeItem
  name="카푸치노"
  price={3500}
/>
```

숫자와 같은 JavaScript 값은 `{}` 안에 전달합니다.

```jsx
price={3000}
```

</details>
