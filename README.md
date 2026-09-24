<details>
<summary>ENG (English Version)</summary>

# Context / Custom Hooks

## 1. Rules of Hooks

React Hooks must be called at the top level of a functional component.

Hooks should be called in the same order whenever the component renders.

Therefore, Hooks should not be called inside:

- Conditional statements
- Loops

Incorrect example:

```jsx
function MyComponent(props) {
  if (props.name !== "") {
    useEffect(() => {
      // ...
    });
  }

  // ...
}
```

Another incorrect example:

```jsx
function MyComponent({
  shouldShow,
}) {
  if (shouldShow) {
    const [
      name,
      setName,
    ] = useState("React");
  }

  // ...
}
```

The order of Hook calls must remain consistent across renders.

---

## 2. What is a Custom Hook?

A **Custom Hook** is a Hook created to reuse logic across multiple components.

It can separate and package logic such as:

- State management
- Side effects
- Data loading
- Event subscriptions

Custom Hook names begin with `use`.

Examples:

```text
useFetchData()
→ Data loading and loading state

useWindowSize()
→ Window size subscription

useToggle()
→ Toggle state management
```

---

## 3. Why Use Custom Hooks?

Suppose two components contain the same toggle logic.

```jsx
function ModalButton() {
  const [
    isOpen,
    setIsOpen,
  ] = useState(false);

  const toggle = () =>
    setIsOpen(
      prev => !prev
    );

  return (
    <div>
      <button
        onClick={toggle}
      >
        Modal
        {isOpen
          ? " Close"
          : " Open"}
      </button>

      {isOpen && (
        <ModalContent />
      )}
    </div>
  );
}
```

```jsx
function DropdownMenu() {
  const [
    isExpanded,
    setIsExpanded,
  ] = useState(false);

  const toggle = () =>
    setIsExpanded(
      prev => !prev
    );

  return (
    <div>
      <button
        onClick={toggle}
      >
        Menu
        {isExpanded
          ? " Hide"
          : " Show"}
      </button>

      {isExpanded && (
        <DropdownContent />
      )}
    </div>
  );
}
```

Both components contain the same pattern:

```text
useState
   ↓
Boolean State
   ↓
Toggle Function
```

If the logic changes later, both components must be modified.

A Custom Hook can separate this duplicated logic.

---

## 4. `useToggle` Custom Hook

The repeated toggle logic can be encapsulated as `useToggle`.

```jsx
import {
  useState,
} from "react";

const useToggle = (
  initialValue = false
) => {
  const [
    value,
    setValue,
  ] = useState(
    initialValue
  );

  const toggle = () => {
    setValue(
      prev => !prev
    );
  };

  return [
    value,
    toggle,
  ];
};

export default useToggle;
```

The Hook returns:

```text
Current State
+
Toggle Function
```

---

## 5. Using `useToggle`

### Modal Component

```jsx
import useToggle
  from "./useToggle";

function ModalButton() {
  const [
    isOpen,
    toggleModal,
  ] = useToggle(false);

  return (
    <div>
      <button
        onClick={
          toggleModal
        }
      >
        Modal
        {isOpen
          ? " Close"
          : " Open"}
      </button>

      {isOpen && (
        <div
          style={{
            border:
              "1px solid black",
            padding: "10px",
          }}
        >
          Modal Content
        </div>
      )}
    </div>
  );
}
```

### Dropdown Component

```jsx
import useToggle
  from "./useToggle";

function DropdownMenu() {
  const [
    isExpanded,
    toggleDropdown,
  ] = useToggle(false);

  return (
    <div>
      <button
        onClick={
          toggleDropdown
        }
      >
        Menu
        {isExpanded
          ? " Hide"
          : " Show"}
      </button>

      {isExpanded && (
        <div
          style={{
            background: "#eee",
            padding: "10px",
          }}
        >
          Dropdown Content
        </div>
      )}
    </div>
  );
}
```

The components reuse the same state-management logic without duplicating it.

---

## 6. Window Size Subscription

A component can subscribe to changes in browser window size.

The process includes:

```text
Window Resize
     ↓
resize Event
     ↓
Event Listener
     ↓
handleResize()
     ↓
State Update
     ↓
Component Re-render
```

React features used:

- `useState`
- `useEffect`
- `window.addEventListener()`
- Cleanup function
- `window.removeEventListener()`

When the component is mounted, the resize listener is registered.

When the component is unmounted, the listener is removed.

---

## 7. `useWindowSize` Custom Hook

The window-size subscription logic can be separated into a custom Hook.

```jsx
import {
  useEffect,
  useState,
} from "react";

const useWindowSize = () => {
  const [
    windowSize,
    setWindowSize,
  ] = useState({
    width:
      window.innerWidth,
    height:
      window.innerHeight,
  });

  useEffect(() => {
    const handleResize =
      () => {
        setWindowSize({
          width:
            window.innerWidth,
          height:
            window.innerHeight,
        });
      };

    window.addEventListener(
      "resize",
      handleResize
    );

    return () => {
      window.removeEventListener(
        "resize",
        handleResize
      );
    };
  }, []);

  return windowSize;
};

export default useWindowSize;
```

The Hook combines:

```text
State
+
Resize Event Subscription
+
Cleanup
```

and returns the current window size.

---

## 8. Reusing `useWindowSize`

### Display Window Size

```jsx
import useWindowSize
  from "./useWindowSize";

function SizeDisplay() {
  const {
    width,
    height,
  } = useWindowSize();

  return (
    <div>
      <h2>
        Current Window Size
      </h2>

      <p>
        Width: {width}px
      </p>

      <p>
        Height: {height}px
      </p>
    </div>
  );
}
```

### Conditional Rendering

```jsx
import useWindowSize
  from "./useWindowSize";

function MobileWarning() {
  const {
    width,
  } = useWindowSize();

  const isMobile =
    width < 768;

  return (
    <div
      style={{
        padding: "20px",
        border:
          "1px solid gray",
      }}
    >
      {isMobile ? (
        <p
          style={{
            color: "red",
            fontWeight:
              "bold",
          }}
        >
          Mobile Environment
        </p>
      ) : (
        <p
          style={{
            color: "green",
          }}
        >
          Desktop Environment
        </p>
      )}
    </div>
  );
}
```

The same Custom Hook can therefore provide window-size information to several components.

---

## 9. `useCounter` Custom Hook

Another example encapsulates counter state management.

```jsx
import React, {
  useState,
} from "react";

function useCounter(
  initialValue
) {
  const [
    count,
    setCount,
  ] = useState(
    initialValue
  );

  const increaseCount =
    () =>
      setCount(
        count =>
          count + 1
      );

  const decreaseCount =
    () =>
      setCount(
        count =>
          Math.max(
            count - 1,
            0
          )
      );

  return [
    count,
    increaseCount,
    decreaseCount,
  ];
}

export default useCounter;
```

Usage:

```jsx
const [
  count,
  increaseCount,
  decreaseCount,
] = useCounter(
  INITIAL_COUNT
);
```

The practice also manages whether the number of users has reached a maximum capacity.

```jsx
const MAX_CAPACITY = 10;

const [
  isFull,
  setIsFull,
] = useState(false);
```

---

## 10. What is Props Drilling?

React normally passes data from a parent component to child components through Props.

When the data has to travel through several intermediate components, **Props Drilling** can occur.

Example:

```text
App
 ↓ theme
Toolbar
 ↓ theme
ThemedButton
 ↓ theme
Button
```

The intermediate components may not need the data themselves but still have to receive and pass the Props.

Example:

```jsx
function Toolbar(props) {
  return (
    <div>
      <ThemedButton
        theme={
          props.theme
        }
      />
    </div>
  );
}
```

```jsx
function ThemedButton(
  props
) {
  return (
    <Button
      theme={
        props.theme
      }
    />
  );
}
```

This can make the component structure more complicated and harder to maintain.

---

## 11. What is Context?

**Context** allows data to be shared throughout a React component tree without explicitly passing Props through every intermediate component.

Conceptually:

```text
Props

Parent
  ↓
Component A
  ↓
Component B
  ↓
Target Component
```

With Context:

```text
Context
   ↓
Provider
   ↓
Component Tree
   ↓
Consumer Component
```

Components at different depths can directly access the shared data.

---

## 12. Common Uses of Context

The branch introduces Context for globally shared data such as:

- Theme settings
  - Light Mode
  - Dark Mode
- User authentication
  - User ID
  - Login state
- Language settings
- Simple global state management

---

## 13. Context Workflow

Context is used in three main steps.

```text
1. Create Context
      ↓
2. Provide Data
      ↓
3. Consume Data
```

React features:

```text
createContext()
      ↓
Provider
      ↓
useContext()
```

---

## 14. Creating Context

A Context object is created using `createContext()`.

```jsx
import {
  createContext,
} from "react";

export const ThemeContext =
  createContext("light");
```

The initial value can be used as a default when a Provider is not present.

---

## 15. Context Provider

The `Provider` supplies actual data to components below it in the component tree.

```jsx
import React, {
  useState,
} from "react";

import {
  ThemeContext,
} from "./ThemeContext";

import Button
  from "./Button";

function ContextApp() {
  const [
    theme,
    setTheme,
  ] = useState(
    "light"
  );

  return (
    <ThemeContext.Provider
      value={{
        theme,
        setTheme,
      }}
    >
      <div
        style={{
          padding: "20px",
        }}
      >
        <h1>
          Theme Provider
        </h1>

        <Button />
      </div>
    </ThemeContext.Provider>
  );
}
```

The shared data is supplied through:

```jsx
value={{
  theme,
  setTheme
}}
```

The child components do not need a `theme` prop.

---

## 16. Consuming Context with `useContext`

A component can access Context data using `useContext`.

```jsx
import React, {
  useContext,
} from "react";

import {
  ThemeContext,
} from "./ThemeContext";

function Button() {
  const {
    theme,
    setTheme,
  } = useContext(
    ThemeContext
  );

  const toggleTheme =
    () => {
      setTheme(
        theme === "light"
          ? "dark"
          : "light"
      );
    };

  const style = {
    backgroundColor:
      theme === "dark"
        ? "black"
        : "white",

    color:
      theme === "dark"
        ? "white"
        : "black",

    padding:
      "10px 20px",
  };

  return (
    <button
      onClick={
        toggleTheme
      }
      style={style}
    >
      Theme:
      {theme.toUpperCase()}
    </button>
  );
}
```

The data flow becomes:

```text
ThemeContext.Provider
        ↓
{ theme, setTheme }
        ↓
useContext(ThemeContext)
        ↓
Button
```

No intermediate Props are required.

---

## 17. Context with a Custom Hook

The Context access logic can also be wrapped inside a Custom Hook.

```jsx
export const ThemeContext =
  createContext("light");

export const useTheme =
  () => {
    const context =
      useContext(
        ThemeContext
      );

    if (
      context ===
      undefined
    ) {
      throw new Error(
        "useTheme must be used within a ThemeProvider"
      );
    }

    return context;
  };
```

Instead of repeatedly writing:

```jsx
useContext(
  ThemeContext
);
```

a component can call:

```jsx
const {
  theme,
  setTheme,
} = useTheme();
```

This encapsulates the way components access the Context.

---

## 18. Using `useTheme`

```jsx
import {
  useTheme,
} from "./ThemeContext";

function Button() {
  const {
    theme,
    setTheme,
  } = useTheme();

  const toggleTheme =
    () => {
      setTheme(
        theme === "light"
          ? "dark"
          : "light"
      );
    };

  const style = {
    backgroundColor:
      theme === "dark"
        ? "black"
        : "white",

    color:
      theme === "dark"
        ? "white"
        : "black",

    padding:
      "10px 20px",
  };

  return (
    <button
      onClick={
        toggleTheme
      }
      style={style}
    >
      Theme:
      {theme.toUpperCase()}
    </button>
  );
}
```

The Custom Hook hides the direct `useContext()` call from the component.

---

## 19. Context + Custom Hook Practice

The final practice uses React Context and a Custom Hook to manage global application settings.

The application manages:

```text
Global Settings
├── Theme
│   ├── light
│   └── dark
│
└── Font Size
    ├── medium
    └── large
```

Files:

```text
src/
├── SettingsContext.js
├── Header.js
├── Content.js
└── App.js
```

### `SettingsContext.js`

Responsibilities:

- Create `SettingsContext`
- Define the `useSettings` Custom Hook

### `App.js`

Acts as the Provider.

Initial values:

```javascript
theme = "light"
fontSize = "medium"
```

The Provider supplies:

- Current theme
- Theme setter
- Current font size
- Font-size setter

### `Header.js`

Provides controls for changing global settings.

Required features:

- Toggle theme between `light` and `dark`
- Increase font size from `medium` to `large`

### `Content.js`

Reads the settings through:

```jsx
useSettings()
```

and applies:

- Background color
- Text color
- Font size

The final structure is:

```text
App
└── SettingsContext.Provider
    ├── Header
    │   └── Change Settings
    │
    └── Content
        └── Apply Settings
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# Context / Custom Hooks

## 1. Hook 사용 규칙

React Hook은 함수형 컴포넌트의 **최상위에서 호출**해야 합니다.

컴포넌트가 렌더링될 때마다 Hook이 같은 순서로 호출되어야 합니다.

따라서 다음 위치에서는 Hook을 호출하지 않습니다.

- 조건문 내부
- 반복문 내부

잘못된 예:

```jsx
function MyComponent(props) {
  if (props.name !== "") {
    useEffect(() => {
      // ...
    });
  }

  // ...
}
```

또 다른 잘못된 예:

```jsx
function MyComponent({
  shouldShow,
}) {
  if (shouldShow) {
    const [
      name,
      setName,
    ] = useState("React");
  }

  // ...
}
```

렌더링마다 Hook의 호출 순서가 동일하게 유지되어야 합니다.

---

## 2. Custom Hook이란?

**Custom Hook**은 여러 컴포넌트에서 반복되는 로직을 재사용하기 위해 직접 만들어 사용하는 Hook입니다.

컴포넌트 내부의 다음과 같은 로직을 분리할 수 있습니다.

- State 관리
- Side Effect 처리
- 데이터 로딩
- 이벤트 구독

Custom Hook의 이름은 `use`로 시작합니다.

예:

```text
useFetchData()
→ 데이터 로딩과 로딩 상태 관리

useWindowSize()
→ 화면 크기 구독

useToggle()
→ Toggle 상태 관리
```

---

## 3. Custom Hook이 필요한 이유

두 컴포넌트에서 동일한 Toggle 로직을 사용한다고 가정합니다.

```jsx
function ModalButton() {
  const [
    isOpen,
    setIsOpen,
  ] = useState(false);

  const toggle = () =>
    setIsOpen(
      prev => !prev
    );

  return (
    <div>
      <button
        onClick={toggle}
      >
        모달
        {isOpen
          ? " 닫기"
          : " 열기"}
      </button>

      {isOpen && (
        <ModalContent />
      )}
    </div>
  );
}
```

```jsx
function DropdownMenu() {
  const [
    isExpanded,
    setIsExpanded,
  ] = useState(false);

  const toggle = () =>
    setIsExpanded(
      prev => !prev
    );

  return (
    <div>
      <button
        onClick={toggle}
      >
        메뉴
        {isExpanded
          ? " 숨기기"
          : " 표시"}
      </button>

      {isExpanded && (
        <DropdownContent />
      )}
    </div>
  );
}
```

두 컴포넌트에는 다음 로직이 중복됩니다.

```text
useState
   ↓
Boolean State
   ↓
Toggle 함수
```

Toggle 로직을 변경할 경우 여러 컴포넌트를 각각 수정해야 합니다.

이를 Custom Hook으로 분리할 수 있습니다.

---

## 4. `useToggle` Custom Hook

반복되는 Toggle 로직을 `useToggle`로 캡슐화합니다.

```jsx
import {
  useState,
} from "react";

const useToggle = (
  initialValue = false
) => {
  const [
    value,
    setValue,
  ] = useState(
    initialValue
  );

  const toggle = () => {
    setValue(
      prev => !prev
    );
  };

  return [
    value,
    toggle,
  ];
};

export default useToggle;
```

Custom Hook에서는 다음 두 값을 반환합니다.

```text
현재 State
+
Toggle 함수
```

---

## 5. `useToggle` 재사용

### Modal 컴포넌트

```jsx
import useToggle
  from "./useToggle";

function ModalButton() {
  const [
    isOpen,
    toggleModal,
  ] = useToggle(false);

  return (
    <div>
      <button
        onClick={
          toggleModal
        }
      >
        모달
        {isOpen
          ? " 닫기"
          : " 열기"}
      </button>

      {isOpen && (
        <div
          style={{
            border:
              "1px solid black",
            padding: "10px",
          }}
        >
          모달 내용
        </div>
      )}
    </div>
  );
}
```

### Dropdown 컴포넌트

```jsx
import useToggle
  from "./useToggle";

function DropdownMenu() {
  const [
    isExpanded,
    toggleDropdown,
  ] = useToggle(false);

  return (
    <div>
      <button
        onClick={
          toggleDropdown
        }
      >
        메뉴
        {isExpanded
          ? " 숨기기"
          : " 표시"}
      </button>

      {isExpanded && (
        <div
          style={{
            background:
              "#eee",
            padding: "10px",
          }}
        >
          드롭다운 내용
        </div>
      )}
    </div>
  );
}
```

동일한 상태 관리 로직을 여러 컴포넌트에서 재사용할 수 있습니다.

---

## 6. 화면 크기 구독

브라우저 창의 크기가 변할 때마다 최신 크기 정보를 State에 반영할 수 있습니다.

전체 흐름:

```text
브라우저 크기 변경
      ↓
resize Event
      ↓
Event Listener
      ↓
handleResize()
      ↓
State 변경
      ↓
컴포넌트 재렌더링
```

사용되는 기능:

- `useState`
- `useEffect`
- `window.addEventListener()`
- Cleanup 함수
- `window.removeEventListener()`

컴포넌트가 Mount될 때 이벤트를 등록하고 Unmount될 때 제거합니다.

---

## 7. `useWindowSize` Custom Hook

화면 크기 구독 로직을 Custom Hook으로 분리합니다.

```jsx
import {
  useEffect,
  useState,
} from "react";

const useWindowSize = () => {
  const [
    windowSize,
    setWindowSize,
  ] = useState({
    width:
      window.innerWidth,
    height:
      window.innerHeight,
  });

  useEffect(() => {
    const handleResize =
      () => {
        setWindowSize({
          width:
            window.innerWidth,
          height:
            window.innerHeight,
        });
      };

    window.addEventListener(
      "resize",
      handleResize
    );

    return () => {
      window.removeEventListener(
        "resize",
        handleResize
      );
    };
  }, []);

  return windowSize;
};

export default useWindowSize;
```

다음 기능이 하나의 Hook 안에 포함됩니다.

```text
State
+
Resize Event 구독
+
Cleanup
```

---

## 8. `useWindowSize` 재사용

### 화면 크기 출력

```jsx
import useWindowSize
  from "./useWindowSize";

function SizeDisplay() {
  const {
    width,
    height,
  } = useWindowSize();

  return (
    <div>
      <h2>
        현재 화면 크기
      </h2>

      <p>
        가로:
        {width}px
      </p>

      <p>
        세로:
        {height}px
      </p>
    </div>
  );
}
```

### 조건부 렌더링

```jsx
import useWindowSize
  from "./useWindowSize";

function MobileWarning() {
  const {
    width,
  } = useWindowSize();

  const isMobile =
    width < 768;

  return (
    <div
      style={{
        padding: "20px",
        border:
          "1px solid gray",
      }}
    >
      {isMobile ? (
        <p
          style={{
            color: "red",
            fontWeight:
              "bold",
          }}
        >
          모바일 환경입니다!
        </p>
      ) : (
        <p
          style={{
            color: "green",
          }}
        >
          데스크톱 환경입니다.
        </p>
      )}
    </div>
  );
}
```

하나의 Custom Hook을 여러 컴포넌트에서 호출하여 화면 크기 정보를 사용할 수 있습니다.

---

## 9. `useCounter` Custom Hook

카운터 상태 관리 역시 Custom Hook으로 분리할 수 있습니다.

```jsx
import React, {
  useState,
} from "react";

function useCounter(
  initialValue
) {
  const [
    count,
    setCount,
  ] = useState(
    initialValue
  );

  const increaseCount =
    () =>
      setCount(
        count =>
          count + 1
      );

  const decreaseCount =
    () =>
      setCount(
        count =>
          Math.max(
            count - 1,
            0
          )
      );

  return [
    count,
    increaseCount,
    decreaseCount,
  ];
}

export default useCounter;
```

사용:

```jsx
const [
  count,
  increaseCount,
  decreaseCount,
] = useCounter(
  INITIAL_COUNT
);
```

실습에서는 최대 수용 인원을 `10`으로 지정하고 최대 인원 도달 상태도 함께 관리합니다.

```jsx
const MAX_CAPACITY = 10;

const [
  isFull,
  setIsFull,
] = useState(false);
```

---

## 10. Props Drilling

React에서는 일반적으로 부모 컴포넌트에서 자식 컴포넌트로 Props를 이용하여 데이터를 전달합니다.

데이터가 여러 단계의 중간 컴포넌트를 거쳐 전달되면 **Props Drilling**이 발생할 수 있습니다.

```text
App
 ↓ theme
Toolbar
 ↓ theme
ThemedButton
 ↓ theme
Button
```

예:

```jsx
function Toolbar(props) {
  return (
    <div>
      <ThemedButton
        theme={
          props.theme
        }
      />
    </div>
  );
}
```

```jsx
function ThemedButton(
  props
) {
  return (
    <Button
      theme={
        props.theme
      }
    />
  );
}
```

중간 컴포넌트가 직접 사용하지 않는 데이터도 계속 전달해야 하므로 코드가 복잡해질 수 있습니다.

---

## 11. Context란?

**Context**는 React 컴포넌트 트리 안에서 데이터를 모든 중간 컴포넌트에 Props로 전달하지 않고 공유할 수 있도록 하는 기능입니다.

Props 방식:

```text
부모
 ↓
컴포넌트 A
 ↓
컴포넌트 B
 ↓
목표 컴포넌트
```

Context 방식:

```text
Context
   ↓
Provider
   ↓
컴포넌트 트리
   ↓
필요한 컴포넌트
```

컴포넌트 트리의 여러 위치에서 필요한 데이터에 접근할 수 있습니다.

---

## 12. Context의 주요 용도

PDF에서는 다음과 같은 전역 데이터를 Context의 대표적인 활용 대상으로 설명합니다.

- 테마 설정
  - Light Mode
  - Dark Mode
- 사용자 인증
  - 사용자 ID
  - 로그인 상태
- 언어 설정
- 복잡하지 않은 수준의 전역 상태 관리

---

## 13. Context 사용 과정

Context는 크게 세 단계로 사용합니다.

```text
1. Context 생성
      ↓
2. 데이터 공급
      ↓
3. 데이터 사용
```

관련 기능:

```text
createContext()
      ↓
Provider
      ↓
useContext()
```

---

## 14. Context 생성

`createContext()`를 이용하여 Context 객체를 생성합니다.

```jsx
import {
  createContext,
} from "react";

export const ThemeContext =
  createContext("light");
```

Provider가 없는 경우 초기값이 기본값으로 사용됩니다.

---

## 15. Context Provider

`Provider`는 Context를 통해 실제 데이터를 하위 컴포넌트에 제공합니다.

```jsx
import React, {
  useState,
} from "react";

import {
  ThemeContext,
} from "./ThemeContext";

import Button
  from "./Button";

function ContextApp() {
  const [
    theme,
    setTheme,
  ] = useState(
    "light"
  );

  return (
    <ThemeContext.Provider
      value={{
        theme,
        setTheme,
      }}
    >
      <div
        style={{
          padding: "20px",
        }}
      >
        <h1>
          테마 공급자
        </h1>

        <Button />
      </div>
    </ThemeContext.Provider>
  );
}
```

공유할 데이터는 `value`에 전달합니다.

```jsx
value={{
  theme,
  setTheme
}}
```

하위 컴포넌트에 별도의 `theme` Props를 전달하지 않아도 됩니다.

---

## 16. `useContext`로 Context 사용

`useContext` Hook을 이용하여 Provider가 공급한 값을 직접 가져옵니다.

```jsx
import React, {
  useContext,
} from "react";

import {
  ThemeContext,
} from "./ThemeContext";

function Button() {
  const {
    theme,
    setTheme,
  } = useContext(
    ThemeContext
  );

  const toggleTheme =
    () => {
      setTheme(
        theme === "light"
          ? "dark"
          : "light"
      );
    };

  const style = {
    backgroundColor:
      theme === "dark"
        ? "black"
        : "white",

    color:
      theme === "dark"
        ? "white"
        : "black",

    padding:
      "10px 20px",
  };

  return (
    <button
      onClick={
        toggleTheme
      }
      style={style}
    >
      테마 전환:
      {theme.toUpperCase()}
    </button>
  );
}
```

전체 데이터 흐름:

```text
ThemeContext.Provider
        ↓
{ theme, setTheme }
        ↓
useContext(ThemeContext)
        ↓
Button
```

중간 컴포넌트에서 Props를 전달할 필요가 없습니다.

---

## 17. Context 접근을 Custom Hook으로 분리

Context를 사용하는 방식 자체도 Custom Hook으로 캡슐화할 수 있습니다.

```jsx
export const ThemeContext =
  createContext("light");

export const useTheme =
  () => {
    const context =
      useContext(
        ThemeContext
      );

    if (
      context ===
      undefined
    ) {
      throw new Error(
        "useTheme must be used within a ThemeProvider"
      );
    }

    return context;
  };
```

컴포넌트에서 직접 다음 코드를 반복하는 대신:

```jsx
useContext(
  ThemeContext
);
```

다음처럼 사용할 수 있습니다.

```jsx
const {
  theme,
  setTheme,
} = useTheme();
```

Context 접근 방법을 Custom Hook 내부에 캡슐화할 수 있습니다.

---

## 18. `useTheme` 사용

```jsx
import {
  useTheme,
} from "./ThemeContext";

function Button() {
  const {
    theme,
    setTheme,
  } = useTheme();

  const toggleTheme =
    () => {
      setTheme(
        theme === "light"
          ? "dark"
          : "light"
      );
    };

  const style = {
    backgroundColor:
      theme === "dark"
        ? "black"
        : "white",

    color:
      theme === "dark"
        ? "white"
        : "black",

    padding:
      "10px 20px",
  };

  return (
    <button
      onClick={
        toggleTheme
      }
      style={style}
    >
      테마 전환:
      {theme.toUpperCase()}
    </button>
  );
}
```

컴포넌트에서는 직접 `useContext()`를 호출하지 않고 `useTheme()`를 이용하여 전역 데이터에 접근합니다.

---

## 19. Context + Custom Hook 종합 실습

마지막 실습에서는 React Context와 Custom Hook을 이용하여 전역적인 애플리케이션 설정을 관리합니다.

관리할 설정:

```text
전역 설정
├── Theme
│   ├── light
│   └── dark
│
└── Font Size
    ├── medium
    └── large
```

생성할 파일:

```text
src/
├── SettingsContext.js
├── Header.js
├── Content.js
└── App.js
```

### `SettingsContext.js`

역할:

- `SettingsContext` 생성
- `useSettings` Custom Hook 정의

### `App.js`

Provider 역할을 수행합니다.

초기값:

```javascript
theme = "light"
fontSize = "medium"
```

Provider를 통해 다음 값을 하위 컴포넌트에 제공합니다.

- 현재 Theme
- Theme 변경 함수
- 현재 Font Size
- Font Size 변경 함수

### `Header.js`

설정을 변경하는 버튼을 구현합니다.

조건:

- `light` ↔ `dark` 테마 전환
- `medium` → `large` Font Size 변경
- `large` 상태에서 추가 클릭 시 더 이상 변경하지 않음

### `Content.js`

다음 Hook을 이용하여 설정값을 가져옵니다.

```jsx
useSettings()
```

설정값에 따라 다음 스타일을 적용합니다.

- 배경색
- 글자색
- Font Size

전체 구조:

```text
App
└── SettingsContext.Provider
    ├── Header
    │   └── 설정 변경
    │
    └── Content
        └── 설정 적용
```

</details>
