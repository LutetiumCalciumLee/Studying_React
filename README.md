<details>
<summary>ENG (English Version)</summary>

# useRef / Forms

## 1. Input Focus in React

HTML provides the `autoFocus` attribute to focus an input when it first appears.

```jsx
<form onSubmit={handleSubmit}>
  <label>
    ID:
    <input
      type="text"
      value={id}
      name="id"
      onChange={event =>
        setId(
          event.target.value
        )
      }
      autoFocus
    />
  </label>

  <label>
    Password:
    <input
      type="password"
      value={pwd}
      name="pwd"
      onChange={event =>
        setPwd(
          event.target.value
        )
      }
    />
  </label>

  <button type="submit">
    Login
  </button>
</form>
```

`autoFocus` works when the element initially appears, but it cannot freely control focus at a specific point after rendering.

For more direct DOM control, React provides `useRef`.

---

## 2. What is `useRef`?

`useRef` provides a **reference** that can directly access a DOM element.

It can be considered a container that stores a reference to a specific object.

```text
useState
→ Tracks changing values
→ State changes cause re-rendering

useRef
→ Stores references or mutable values
→ Changes do not cause re-rendering
```

`useRef` can be used for:

- Accessing DOM elements
- Controlling input focus
- Storing mutable values
- Keeping values without causing re-rendering

---

## 3. Basic `useRef` Syntax

```jsx
const refContainer =
  useRef(initialValue);
```

The current referenced value is stored in:

```javascript
refContainer.current
```

For DOM references, the initial value is commonly `null`.

```jsx
const inputRef =
  useRef(null);
```

---

## 4. Accessing the DOM with `useRef`

Basic focus example:

```jsx
import React, {
  useRef,
} from "react";

function FocusInput() {
  const inputRef =
    useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input
        type="text"
        ref={inputRef}
      />

      <button
        onClick={
          handleFocus
        }
      >
        Focus Input
      </button>
    </div>
  );
}
```

The process is:

```text
useRef(null)
    ↓
Create Ref Object
    ↓
ref={inputRef}
    ↓
Connect Ref to DOM
    ↓
inputRef.current
    ↓
Access DOM Element
    ↓
focus()
```

---

## 5. `useRef` with `useEffect`

`useRef` and `useEffect` can be combined to focus an element after the component is mounted.

```jsx
const idRef =
  useRef(null);

useEffect(() => {
  idRef.current.focus();
}, []);
```

Connect the Ref to an input:

```jsx
<input
  type="text"
  value={id}
  name="id"
  onChange={event =>
    setId(
      event.target.value
    )
  }
  ref={idRef}
/>
```

This can replace the initial focusing behavior of `autoFocus` while allowing the same Ref to be reused later.

---

## 6. Login Form Focus Control

A login form can use `useRef` to return focus to the ID field when necessary.

```jsx
function Login() {
  const [id, setId] =
    useState("");

  const [pwd, setPwd] =
    useState("");

  const idRef =
    useRef(null);

  useEffect(() => {
    idRef.current.focus();
  }, []);

  const handleSubmit =
    event => {
      event.preventDefault();

      if (
        id === "hgd" &&
        pwd === "1234"
      ) {
        alert(
          `${id}, welcome.`
        );
      } else {
        alert(
          "Please check ID and password."
        );

        idRef.current.focus();
      }

      setId("");
      setPwd("");
    };

  return (
    <form
      onSubmit={
        handleSubmit
      }
    >
      <input
        type="text"
        value={id}
        onChange={event =>
          setId(
            event.target.value
          )
        }
        ref={idRef}
      />

      <input
        type="password"
        value={pwd}
        onChange={event =>
          setPwd(
            event.target.value
          )
        }
      />

      <button type="submit">
        Login
      </button>
    </form>
  );
}
```

---

## 7. Form Elements

This branch covers several form elements.

```text
<input>
<textarea>
<select>
<select multiple>
```

Traditional HTML form elements manage their own internal values.

In React, form values can instead be managed using component state.

---

## 8. Controlled Components

A **Controlled Component** is a form element whose value is controlled by React.

React commonly uses `useState()` to manage the input data.

Basic flow:

```text
User Input
    ↓
onChange
    ↓
Event Handler
    ↓
useState
    ↓
State Update
    ↓
Input Value
```

Example:

```jsx
const [
  value,
  setValue,
] = useState("");

<input
  value={value}
  onChange={event =>
    setValue(
      event.target.value
    )
  }
/>
```

---

## 9. `textarea`

A `textarea` can be controlled through state in the same way as an input.

```jsx
const RequestForm = () => {
  const [
    value,
    setValue,
  ] = useState(
    "Enter your request"
  );

  const handleChange =
    event => {
      setValue(
        event.target.value
      );
    };

  const handleSubmit =
    event => {
      event.preventDefault();

      alert(
        `Request: ${value}`
      );
    };

  return (
    <form
      onSubmit={
        handleSubmit
      }
    >
      <label>
        Request:

        <textarea
          cols="40"
          rows="5"
          value={value}
          onChange={
            handleChange
          }
        />
      </label>

      <button type="submit">
        Submit
      </button>
    </form>
  );
};
```

---

## 10. `select`

A `select` element can also be controlled through React state.

```jsx
const [
  value,
  setValue,
] = useState(
  "grape"
);

const handleChange =
  event => {
    setValue(
      event.target.value
    );
  };

const handleSubmit =
  event => {
    event.preventDefault();

    alert(
      `Selected fruit: ${value}`
    );
  };
```

JSX:

```jsx
<form
  onSubmit={
    handleSubmit
  }
>
  <label>
    Select a fruit:

    <select
      value={value}
      onChange={
        handleChange
      }
    >
      <option value="apple">
        Apple
      </option>

      <option value="banana">
        Banana
      </option>

      <option value="cherry">
        Cherry
      </option>

      <option value="grape">
        Grape
      </option>

      <option value="orange">
        Orange
      </option>
    </select>
  </label>

  <button type="submit">
    Submit
  </button>
</form>
```

---

## 11. Multiple Selection

A `select` element can allow multiple selections using the `multiple` attribute.

```jsx
<select multiple={true}>
```

For multiple selection, the state is managed as an array.

```jsx
const [
  selectedFruits,
  setSelectedFruits,
] = useState([
  "grape",
]);
```

The selected values can be obtained through:

```javascript
event.target.options
```

or:

```javascript
event.target.selectedOptions
```

---

## 12. Multiple Selection with `options`

`event.target.options` contains all option elements.

The selected values can be collected by checking the `selected` property.

```jsx
const handleChange =
  event => {
    const options =
      event.target.options;

    const newSelectedValues =
      [];

    for (
      let i = 0;
      i < options.length;
      i++
    ) {
      if (
        options[i].selected
      ) {
        newSelectedValues.push(
          options[i].value
        );
      }
    }

    setSelectedFruits(
      newSelectedValues
    );
  };
```

---

## 13. Multiple Selection with `selectedOptions`

`selectedOptions` contains only the currently selected option elements.

```jsx
const handleChange =
  event => {
    const selectedOptions = [
      ...event.target
        .selectedOptions,
    ].map(
      option =>
        option.value
    );

    setSelectedFruits(
      selectedOptions
    );
  };
```

This provides a more concise way to retrieve the selected values.

---

## 14. Managing Multiple Inputs Separately

Each form value can be managed using an independent state variable.

```jsx
function Login() {
  const [
    id,
    setId,
  ] = useState("");

  const [
    pwd,
    setPwd,
  ] = useState("");

  return (
    <form>
      <input
        type="text"
        name="id"
        value={id}
        onChange={
          event =>
            setId(
              event.target.value
            )
        }
      />

      <input
        type="password"
        name="pwd"
        value={pwd}
        onChange={
          event =>
            setPwd(
              event.target.value
            )
        }
      />
    </form>
  );
}
```

This is simple, but the number of state variables and handlers can increase as the form grows.

---

## 15. Managing Multiple Inputs with One Object

Multiple input fields can be stored inside one state object.

```jsx
const [
  formValues,
  setFormValues,
] = useState({
  id: "",
  password: "",
});
```

Each input uses a `name` that matches a property in the state object.

```jsx
<input
  type="text"
  name="id"
  value={formValues.id}
  onChange={
    handleChange
  }
/>

<input
  type="password"
  name="password"
  value={
    formValues.password
  }
  onChange={
    handleChange
  }
/>
```

---

## 16. Generic `handleChange`

`event.target.name` can identify which input has changed.

```jsx
const handleChange =
  event => {
    const {
      name,
      value,
    } = event.target;

    setFormValues(
      prevValues => ({
        ...prevValues,
        [name]: value,
      })
    );
  };
```

The process is:

```text
Input Changed
    ↓
event.target.name
    ↓
Identify State Property
    ↓
event.target.value
    ↓
Update Selected Property
```

The spread syntax preserves the other properties in the state object.

---

## 17. User Profile Form

The practice form manages several kinds of inputs inside one object.

```jsx
const [
  formValues,
  setFormValues,
] = useState({
  name: "",
  gender: "",
  age: "",
  fruit: "",
});
```

The form includes:

- Name: `text`
- Gender: `radio`
- Age: `number`
- Favorite fruit: `select`

A separate state stores the final submitted data.

```jsx
const [
  submittedData,
  setSubmittedData,
] = useState(null);
```

---

## 18. Handling Different Input Types

The form handler can inspect several properties of the event target.

```jsx
const handleChange =
  event => {
    const {
      name,
      value,
      type,
      checked,
    } = event.target;

    const newValue =
      type === "radio"
        ? (
            checked
              ? value
              : formValues[
                  name
                ]
          )
        : value;

    setFormValues(
      prevValues => ({
        ...prevValues,
        [name]:
          newValue,
      })
    );
  };
```

This allows multiple form elements to share one event handler.

---

## 19. Displaying Submitted Data

After form submission, the saved information can be displayed through conditional rendering.

```jsx
<h2>
  User Information
</h2>

{submittedData ? (
  <div>
    <p>
      Name:
      <strong>
        {submittedData.name}
      </strong>
    </p>

    <p>
      Gender:
      <strong>
        {
          submittedData.gender
        }
      </strong>
    </p>

    <p>
      Age:
      <strong>
        {submittedData.age}
      </strong>
    </p>

    <p>
      Favorite Fruit:
      <strong>
        {
          submittedData.fruit
        }
      </strong>
    </p>
  </div>
) : (
  <p>
    Submit the form
    to view the information.
  </p>
)}
```

---

## 20. Kiosk Practice

The branch includes a mini kiosk project for code analysis and feature extension.

Main React concepts used in the project:

- `useState`
- Props
- Parent-to-child data passing
- `map()` list rendering
- Event handling
- `reduce()` total calculation
- Category filtering
- Component separation

Project structure:

```text
kiosk-mini/
├── src/
│   ├── App.js
│   ├── index.js
│   ├── components/
│   │   ├── DrinkCard.jsx
│   │   └── QuantityControl.jsx
│   └── styles/
│       └── kiosk.css
│
└── public/
    └── images/
```

The project manages the current screen and shopping cart through state.

---

## 21. Kiosk Feature Extension

The practice extends the kiosk with additional features.

Examples include:

- ICE / HOT options
- Drink size options
- Price adjustments
- Recommended menu display
- Maximum quantity control
- Warning messages
- Cart persistence using localStorage
- Product images

This exercise combines the React concepts learned in previous branches.

---

## 22. Movie Page Practice

Another exercise analyzes a movie introduction page.

Main tasks include:

- Install and run the provided React project
- Analyze the project structure
- Identify the implemented features
- Compare different image `src` handling methods
- Suggest improvements

The example UI displays movie cards and a movie title search field.

---

## 23. Bulletin Board Practice

The final exercise applies component separation and `useRef` to a bulletin board.

Project structure:

```text
bbs/
└── src/
    ├── App.js
    ├── index.js
    └── components/
        ├── Article.jsx
        ├── Create.jsx
        ├── CHeader.jsx
        ├── Nav.jsx
        └── Update.jsx
```

Main tasks:

- Analyze the bulletin board code
- Separate the UI into components
- Implement post creation
- Implement post editing
- Focus the first input field using `useRef`
- Apply CSS

`useRef` is used when creating or editing a post so that the first input field receives focus.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# useRef / Forms

## 1. React 입력 양식 포커스

HTML의 `autoFocus` 속성을 사용하면 화면이 처음 나타날 때 특정 입력란에 포커스를 지정할 수 있습니다.

```jsx
<form onSubmit={handleSubmit}>
  <label>
    아이디:
    <input
      type="text"
      value={id}
      name="id"
      onChange={event =>
        setId(
          event.target.value
        )
      }
      autoFocus
    />
  </label>

  <label>
    비밀번호:
    <input
      type="password"
      value={pwd}
      name="pwd"
      onChange={event =>
        setPwd(
          event.target.value
        )
      }
    />
  </label>

  <button type="submit">
    로그인
  </button>
</form>
```

`autoFocus`는 처음 한 번 포커스를 지정할 수 있지만, 이후 원하는 시점마다 DOM 요소의 포커스를 직접 제어하기에는 한계가 있습니다.

이러한 경우 `useRef`를 사용할 수 있습니다.

---

## 2. `useRef`란?

`useRef`는 특정 DOM 요소에 직접 접근할 수 있는 **참조(Reference)**를 제공하는 Hook입니다.

특정 객체를 보관하는 상자와 같은 역할을 합니다.

```text
useState
→ 값의 변화를 관리
→ State 변경 시 재렌더링 발생

useRef
→ DOM이나 변경 가능한 값의 참조 관리
→ 값 변경만으로 재렌더링되지 않음
```

대표적인 사용 목적:

- DOM 요소 직접 접근
- 입력 요소 포커스 제어
- 변경 가능한 값 저장
- 렌더링에 영향을 주지 않는 값 유지

---

## 3. `useRef` 기본 문법

```jsx
const refContainer =
  useRef(initialValue);
```

현재 참조하고 있는 값은 `current` 속성에 저장됩니다.

```javascript
refContainer.current
```

DOM 요소를 참조할 때는 일반적으로 `null`로 초기화합니다.

```jsx
const inputRef =
  useRef(null);
```

---

## 4. `useRef`를 이용한 DOM 접근

입력 요소에 직접 포커스를 지정하는 예제입니다.

```jsx
import React, {
  useRef,
} from "react";

function FocusInput() {
  const inputRef =
    useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input
        type="text"
        ref={inputRef}
      />

      <button
        onClick={
          handleFocus
        }
      >
        입력 필드에 포커스
      </button>
    </div>
  );
}
```

동작 과정:

```text
useRef(null)
    ↓
Ref 객체 생성
    ↓
ref={inputRef}
    ↓
DOM 요소와 연결
    ↓
inputRef.current
    ↓
DOM 요소 접근
    ↓
focus()
```

---

## 5. `useRef`와 `useEffect`

`useEffect`와 함께 사용하면 컴포넌트가 마운트된 후 DOM 요소에 포커스를 지정할 수 있습니다.

```jsx
const idRef =
  useRef(null);

useEffect(() => {
  idRef.current.focus();
}, []);
```

입력 요소에 연결합니다.

```jsx
<input
  type="text"
  value={id}
  name="id"
  onChange={event =>
    setId(
      event.target.value
    )
  }
  ref={idRef}
/>
```

초기 포커싱뿐 아니라 이후 필요한 시점에도 같은 Ref를 이용하여 DOM 요소에 접근할 수 있습니다.

---

## 6. 로그인 화면 포커스 제어

로그인 결과에 따라 아이디 입력란에 다시 포커스를 지정할 수 있습니다.

```jsx
function Login() {
  const [
    id,
    setId,
  ] = useState("");

  const [
    pwd,
    setPwd,
  ] = useState("");

  const idRef =
    useRef(null);

  useEffect(() => {
    idRef.current.focus();
  }, []);

  const handleSubmit =
    event => {
      event.preventDefault();

      if (
        id === "hgd" &&
        pwd === "1234"
      ) {
        alert(
          `${id}님 반갑습니다.`
        );
      } else {
        alert(
          "id, pwd 확인바랍니다."
        );

        idRef.current.focus();
      }

      setId("");
      setPwd("");
    };

  return (
    <form
      onSubmit={
        handleSubmit
      }
    >
      <input
        type="text"
        value={id}
        onChange={
          event =>
            setId(
              event.target.value
            )
        }
        ref={idRef}
      />

      <input
        type="password"
        value={pwd}
        onChange={
          event =>
            setPwd(
              event.target.value
            )
        }
      />

      <button type="submit">
        로그인
      </button>
    </form>
  );
}
```

---

## 7. Form 입력 요소

이 Branch에서는 다음과 같은 Form 요소를 다룹니다.

```text
<input>
<textarea>
<select>
<select multiple>
```

기존 HTML Form에서는 각 요소가 자체적으로 값을 관리합니다.

React에서는 입력값을 State와 연결하여 관리할 수 있습니다.

---

## 8. Controlled Component

**Controlled Component**는 React에서 입력값을 직접 관리하는 Form 요소입니다.

일반적으로 `useState()`를 이용하여 입력 데이터를 관리합니다.

기본 흐름:

```text
사용자 입력
    ↓
onChange
    ↓
이벤트 핸들러
    ↓
useState
    ↓
State 변경
    ↓
입력값 반영
```

예시:

```jsx
const [
  value,
  setValue,
] = useState("");

<input
  value={value}
  onChange={event =>
    setValue(
      event.target.value
    )
  }
/>
```

---

## 9. `textarea`

`textarea` 역시 State와 연결하여 제어할 수 있습니다.

```jsx
const RequestForm = () => {
  const [
    value,
    setValue,
  ] = useState(
    "요청사항을 입력하세요"
  );

  const handleChange =
    event => {
      setValue(
        event.target.value
      );
    };

  const handleSubmit =
    event => {
      event.preventDefault();

      alert(
        `입력한 요청사항: ${value}`
      );
    };

  return (
    <form
      onSubmit={
        handleSubmit
      }
    >
      <label>
        요청사항:

        <textarea
          cols="40"
          rows="5"
          value={value}
          onChange={
            handleChange
          }
        />
      </label>

      <button type="submit">
        제출
      </button>
    </form>
  );
};
```

---

## 10. `select`

`select` 요소도 State와 연결하여 선택값을 관리할 수 있습니다.

```jsx
const [
  value,
  setValue,
] = useState(
  "grape"
);

const handleChange =
  event => {
    setValue(
      event.target.value
    );
  };

const handleSubmit =
  event => {
    event.preventDefault();

    alert(
      `선택한 과일: ${value}`
    );
  };
```

JSX:

```jsx
<form
  onSubmit={
    handleSubmit
  }
>
  <label>
    과일을 선택하세요:

    <select
      value={value}
      onChange={
        handleChange
      }
    >
      <option value="apple">
        사과
      </option>

      <option value="banana">
        바나나
      </option>

      <option value="cherry">
        체리
      </option>

      <option value="grape">
        포도
      </option>

      <option value="orange">
        오렌지
      </option>
    </select>
  </label>

  <button type="submit">
    제출
  </button>
</form>
```

---

## 11. `select` 다중 선택

`multiple` 속성을 사용하면 여러 항목을 선택할 수 있습니다.

```jsx
<select multiple={true}>
```

다중 선택에서는 State를 배열로 관리합니다.

```jsx
const [
  selectedFruits,
  setSelectedFruits,
] = useState([
  "grape",
]);
```

선택값을 가져올 때 다음 값을 사용할 수 있습니다.

```javascript
event.target.options
```

또는:

```javascript
event.target.selectedOptions
```

---

## 12. `options`를 이용한 다중 선택

`event.target.options`에는 모든 `<option>` 요소가 포함됩니다.

각 요소의 `selected` 값을 확인하여 선택된 값만 배열에 추가합니다.

```jsx
const handleChange =
  event => {
    const options =
      event.target.options;

    const newSelectedValues =
      [];

    for (
      let i = 0;
      i < options.length;
      i++
    ) {
      if (
        options[i].selected
      ) {
        newSelectedValues.push(
          options[i].value
        );
      }
    }

    setSelectedFruits(
      newSelectedValues
    );
  };
```

---

## 13. `selectedOptions`를 이용한 다중 선택

`selectedOptions`에는 현재 선택된 `<option>` 요소만 포함됩니다.

```jsx
const handleChange =
  event => {
    const selectedOptions = [
      ...event.target
        .selectedOptions,
    ].map(
      option =>
        option.value
    );

    setSelectedFruits(
      selectedOptions
    );
  };
```

선택된 값만 바로 가져올 수 있어 보다 간결하게 처리할 수 있습니다.

---

## 14. 여러 입력값을 각각 관리

입력값마다 별도의 State를 사용할 수 있습니다.

```jsx
function Login() {
  const [
    id,
    setId,
  ] = useState("");

  const [
    pwd,
    setPwd,
  ] = useState("");

  return (
    <form>
      <input
        type="text"
        name="id"
        value={id}
        onChange={
          event =>
            setId(
              event.target.value
            )
        }
      />

      <input
        type="password"
        name="pwd"
        value={pwd}
        onChange={
          event =>
            setPwd(
              event.target.value
            )
        }
      />
    </form>
  );
}
```

입력 요소가 많아질수록 State와 이벤트 핸들러의 수도 증가합니다.

---

## 15. 하나의 객체로 여러 입력값 관리

여러 입력 필드의 값을 하나의 객체 State로 관리할 수 있습니다.

```jsx
const [
  formValues,
  setFormValues,
] = useState({
  id: "",
  password: "",
});
```

각 입력 요소의 `name`을 State 객체의 Key와 동일하게 지정합니다.

```jsx
<input
  type="text"
  name="id"
  value={formValues.id}
  onChange={
    handleChange
  }
/>

<input
  type="password"
  name="password"
  value={
    formValues.password
  }
  onChange={
    handleChange
  }
/>
```

---

## 16. 공통 `handleChange`

`event.target.name`을 이용하면 어떤 입력 필드에서 이벤트가 발생했는지 확인할 수 있습니다.

```jsx
const handleChange =
  event => {
    const {
      name,
      value,
    } = event.target;

    setFormValues(
      prevValues => ({
        ...prevValues,
        [name]: value,
      })
    );
  };
```

동작 과정:

```text
입력값 변경
    ↓
event.target.name
    ↓
변경할 State 속성 식별
    ↓
event.target.value
    ↓
해당 속성 변경
```

Spread Syntax를 사용하여 기존 객체의 다른 값은 유지합니다.

---

## 17. 사용자 정보 입력 Form

실습에서는 여러 종류의 입력값을 하나의 객체로 관리합니다.

```jsx
const [
  formValues,
  setFormValues,
] = useState({
  name: "",
  gender: "",
  age: "",
  fruit: "",
});
```

입력 항목:

```text
name
→ text

gender
→ radio

age
→ number

fruit
→ select
```

제출된 최종 정보는 별도의 State로 관리합니다.

```jsx
const [
  submittedData,
  setSubmittedData,
] = useState(null);
```

---

## 18. 서로 다른 입력 유형 처리

이벤트 객체의 여러 속성을 이용하여 입력 유형별로 값을 처리할 수 있습니다.

```jsx
const handleChange =
  event => {
    const {
      name,
      value,
      type,
      checked,
    } = event.target;

    const newValue =
      type === "radio"
        ? (
            checked
              ? value
              : formValues[
                  name
                ]
          )
        : value;

    setFormValues(
      prevValues => ({
        ...prevValues,
        [name]:
          newValue,
      })
    );
  };
```

하나의 이벤트 핸들러로 여러 종류의 Form 요소를 관리할 수 있습니다.

---

## 19. 제출된 데이터 출력

제출된 데이터가 존재하는지에 따라 조건부 렌더링할 수 있습니다.

```jsx
<h2>
  입력한 사용자 정보
</h2>

{submittedData ? (
  <div>
    <p>
      이름:
      <strong>
        {submittedData.name}
      </strong>
    </p>

    <p>
      성별:
      <strong>
        {
          submittedData.gender
        }
      </strong>
    </p>

    <p>
      나이:
      <strong>
        {submittedData.age}
      </strong>
    </p>

    <p>
      좋아하는 과일:
      <strong>
        {
          submittedData.fruit
        }
      </strong>
    </p>
  </div>
) : (
  <p>
    제출 버튼을 눌러
    정보를 확인하세요.
  </p>
)}
```

---

## 20. Kiosk 프로젝트 실습

PDF 후반부에서는 Mini Kiosk 프로젝트의 코드를 분석하고 기능을 확장합니다.

프로젝트에서 사용하는 주요 React 개념:

- `useState`
- Props
- 부모 → 자식 데이터 전달
- `map()` 리스트 렌더링
- 이벤트 핸들링
- `reduce()` 총액 계산
- 카테고리 필터링
- 컴포넌트 분리

프로젝트 구조:

```text
kiosk-mini/
├── src/
│   ├── App.js
│   ├── index.js
│   ├── components/
│   │   ├── DrinkCard.jsx
│   │   └── QuantityControl.jsx
│   └── styles/
│       └── kiosk.css
│
└── public/
    └── images/
```

현재 화면과 장바구니 데이터 등을 State로 관리합니다.

---

## 21. Kiosk 기능 확장

실습에서는 기존 Kiosk에 여러 기능을 추가합니다.

주요 확장 내용:

```text
음료 옵션
→ ICE / HOT

사이즈 옵션
→ 가격 가중치 반영

카테고리 추천
→ 오늘의 추천 표시

수량 제한
→ 최대 수량 변경 및 경고

데이터 저장
→ localStorage

상품 이미지
→ public/images 사용
```

이전 Branch에서 학습한 State, Props, 리스트 렌더링, 이벤트 처리 등을 종합적으로 활용합니다.

---

## 22. 영화 소개 페이지 실습

또 다른 실습에서는 영화 소개 React 프로젝트를 분석합니다.

주요 작업:

- 제공된 React 프로젝트 실행
- 프로젝트 구조 분석
- 기능 정리
- 이미지 `src` 처리 방식 비교
- 개선 아이디어 작성

예제 화면에서는 영화 제목 검색과 여러 영화 카드가 표시됩니다.

---

## 23. 게시판 실습

마지막 실습에서는 게시판 코드를 여러 컴포넌트로 분리하고 `useRef`를 적용합니다.

프로젝트 구조:

```text
bbs/
└── src/
    ├── App.js
    ├── index.js
    └── components/
        ├── Article.jsx
        ├── Create.jsx
        ├── CHeader.jsx
        ├── Nav.jsx
        └── Update.jsx
```

주요 구현 내용:

- 게시판 코드 분석
- 컴포넌트 분리
- 글 작성
- 글 수정
- 글 작성 및 수정 시 첫 입력란에 `useRef`로 포커싱
- CSS 적용

`useRef`를 실제 UI 기능에 적용하여 글 작성 또는 수정 화면이 나타날 때 첫 입력란에 포커스를 지정합니다.

</details>
