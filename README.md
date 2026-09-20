<details>
<summary>ENG (English Version)</summary>

# Modern JavaScript

## 1. ES6 and Modern JavaScript

Modern JavaScript generally refers to JavaScript based on **ECMAScript 6 (ES6) and later specifications**.

ES6 was introduced as ECMAScript 2015, and new features have continued to be added to the JavaScript specification.

Modern web development commonly uses:

- React, Vue, Angular
- npm and Yarn
- Webpack
- Babel
- SPA architecture
- Functional components and Hooks
- Immutable data updates

For React development, understanding ES6 syntax and array methods such as `map()`, `filter()`, and `reduce()` is important.

---

## 2. `let` and `const`

Modern JavaScript generally uses `let` and `const` instead of `var`.

### `var`

- Function-scoped
- Allows redeclaration
- Allows reassignment
- Can cause unexpected behavior because of hoisting and scope

```javascript
var value = "first";

value = "second";

var value = "third";
```

### `let`

`let` is block-scoped and allows reassignment.

```javascript
let value = "first";

value = "second";
```

Redeclaration in the same scope is not allowed.

```javascript
let value = "first";

// SyntaxError
let value = "second";
```

### `const`

`const` is block-scoped and cannot be reassigned.

```javascript
const value = "React";
```

```javascript
// TypeError
value = "JavaScript";
```

---

## 3. `const` with Objects and Arrays

When an object or array is declared with `const`, the variable itself cannot be reassigned.

However, its internal properties or elements can still be changed.

### Object

```javascript
const user = {
  name: "Popcorn",
  age: 24,
};

user.name = "React";
user.address = "Seoul";
```

### Array

```javascript
const animals = ["dog", "cat"];

animals[0] = "bird";
animals.push("monkey");
```

The reference stored in the variable remains the same while the internal data changes.

---

## 4. Hoisting

**Hoisting** describes how variable and function declarations behave as if they are processed before code execution.

With `var`, the variable is initialized with `undefined`.

```javascript
console.log(myVar); // undefined

var myVar = "React";
```

`let` and `const` are also hoisted, but they are not initialized in the same way.

Accessing them before declaration causes an error.

```javascript
// ReferenceError
console.log(myLet);

let myLet = "React";
```

---

## 5. Template Literals

Template literals use backticks `` ` ` `` and `${}` to insert variables or expressions into strings.

Traditional string concatenation:

```javascript
const name = "Popcorn";
const age = 24;

const message =
  "My name is " + name + ". I am " + age + " years old.";
```

Using a template literal:

```javascript
const message =
  `My name is ${name}. I am ${age} years old.`;
```

Expressions and function calls can also be used.

```javascript
const sayHello = () => "Hello";

const month = 1;

const message =
  `${sayHello()}! ${month * 8}`;
```

---

## 6. Arrow Functions

Arrow functions provide a shorter syntax for defining functions.

Traditional function:

```javascript
const add = function (a, b) {
  return a + b;
};
```

Arrow function:

```javascript
const add = (a, b) => {
  return a + b;
};
```

When the function contains only one expression, `{}` and `return` can be omitted.

```javascript
const multiply = (a, b) => a * b;
```

When there is only one parameter, parentheses can also be omitted.

```javascript
const square = num => num * num;
```

### Returning an Object

An object can be returned directly by wrapping it in parentheses.

```javascript
const createUser = (name, age) => ({
  name: name,
  age: age,
});
```

Arrow functions also inherit `this` from the scope where they are defined.

---

## 7. Destructuring Assignment

Destructuring allows values from objects or arrays to be extracted into individual variables.

### Object Destructuring

```javascript
const profile = {
  name: "Popcorn",
  age: 24,
};

const { name, age } = profile;
```

Properties can be extracted in any order.

```javascript
const { age, name } = profile;
```

A different variable name can also be assigned.

```javascript
const {
  name: newName,
  age: newAge,
} = profile;
```

### Array Destructuring

Array values are assigned according to their position.

```javascript
const profile = ["Popcorn", 24];

const [name, age] = profile;
```

Only required elements can also be extracted.

```javascript
const [name] = profile;
```

---

## 8. Default Values

Default values can be used when an argument or property value is not provided.

### Function Parameter

```javascript
const sayHello = (name = "Guest") => {
  console.log(`${name}, hello`);
};

sayHello();
sayHello("Popcorn");
```

### Object Destructuring

```javascript
const profile = {
  age: 24,
};

const { name = "Guest" } = profile;

console.log(`${name}, hello`);
```

Default values help prevent unexpected `undefined` values.

---

## 9. Spread Syntax

Spread syntax uses `...` to expand arrays or objects into individual elements or properties.

### Combining Arrays

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

const combined = [...arr1, ...arr2];

console.log(combined);
// [1, 2, 3, 4]
```

Elements can also be inserted while spreading an array.

```javascript
const newArray = [0, ...arr1, 3];
```

### Copying Arrays

```javascript
const original = [10, 20];

const copied = [...original];

copied[0] = 100;
```

The new array is separate from the original array.

### Combining Objects

```javascript
const user = {
  name: "React",
  age: 10,
};

const location = {
  city: "Seoul",
};

const profile = {
  ...user,
  ...location,
};
```

### Updating Object Properties

```javascript
const updatedUser = {
  ...user,
  age: 11,
};
```

This pattern is frequently used for immutable updates in React.

---

## 10. Spread Syntax in Function Calls

An array can be expanded into individual function arguments.

```javascript
const numbers = [1, 2];

const sum = (num1, num2) => num1 + num2;

console.log(sum(...numbers));
```

---

## 11. Rest Parameter

Rest parameters also use `...`, but they collect multiple arguments into an array.

```javascript
const sum = (...numbers) => {
  return numbers.reduce(
    (acc, number) => acc + number,
    0
  );
};

console.log(sum(1, 2));
console.log(sum(1, 2, 3));
```

A rest parameter must be the final parameter of a function.

```javascript
const introduce = (name, ...hobbies) => {
  console.log(`My name is ${name}.`);
  console.log(hobbies);
};

introduce(
  "React",
  "Coding",
  "Exercise"
);
```

### Spread vs Rest

```text
Spread
Array/Object → Individual Elements

Rest
Individual Values → Array
```

---

## 12. Object Property Shorthand

When a variable name and an object property name are the same, the property value can be omitted.

Traditional syntax:

```javascript
const name = "Popcorn";
const age = 24;

const user = {
  name: name,
  age: age,
};
```

Shorthand syntax:

```javascript
const user = {
  name,
  age,
};
```

---

## 13. Functional Array Methods

Functional methods return new data without directly modifying the original data.

Common methods:

- `map()`
- `filter()`
- `reduce()`

These methods are important when handling data and state in React.

---

## 14. `map()`

`map()` iterates through every element of an array and creates a new array from the returned values.

```javascript
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(
  number => number * 2
);
```

Object properties can also be extracted.

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
];

const names = users.map(
  user => user.name
);

console.log(names);
```

The callback can receive an index.

```javascript
const names = [
  "Popcorn",
  "Vue",
  "Ant",
];

names.map((name, index) => {
  console.log(
    `${index + 1}: ${name}`
  );
});
```

---

## 15. `filter()`

`filter()` creates a new array containing only the elements that satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4, 5];

const oddNumbers = numbers.filter(
  number => number % 2 === 1
);

console.log(oddNumbers);
// [1, 3, 5]
```

---

## 16. `reduce()`

`reduce()` processes all elements of an array and reduces them into a single value.

```javascript
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce(
  (accumulator, currentNumber) => {
    return accumulator + currentNumber;
  },
  0
);

console.log(sum);
// 10
```

`reduce()` can return numbers, strings, arrays, or objects.

Example:

```javascript
const students = [
  { id: 1, name: "Kim", score: 64 },
  { id: 2, name: "Lee", score: 91 },
  { id: 3, name: "Park", score: 82 },
];

const scores = students.reduce(
  (acc, student) => {
    acc[student.name] = student.score;
    return acc;
  },
  {}
);

console.log(scores);
```

Result:

```javascript
{
  Kim: 64,
  Lee: 91,
  Park: 82
}
```

---

## 17. Method Chaining

Array methods can be connected into a processing pipeline.

```javascript
const students = [
  { id: 1, name: "Kim", score: 64 },
  { id: 2, name: "Lee", score: 91 },
  { id: 3, name: "Park", score: 82 },
];

const totalScore = students
  .map(student => ({
    ...student,
    score: student.score + 10,
  }))
  .filter(student => student.score >= 80)
  .reduce(
    (acc, student) => acc + student.score,
    0
  );

console.log(totalScore);
```

The processing order is:

```text
Original Array
     ↓
map()
     ↓
Transform Data
     ↓
filter()
     ↓
Select Data
     ↓
reduce()
     ↓
Single Result
```

---

## 18. Ternary Operator

The ternary operator is frequently used for conditional expressions.

Syntax:

```javascript
condition
  ? valueIfTrue
  : valueIfFalse
```

Example:

```javascript
const isLoggedIn = true;

const message = isLoggedIn
  ? "Welcome!"
  : "Please log in.";
```

It can also be used inside functions.

```javascript
const result = num =>
  num % 2 === 0
    ? "even"
    : "odd";
```

---

## 19. Logical Operators

### OR Operator `||`

If the left value is evaluated as falsy, the right value is returned.

```javascript
const nickname = null;

const displayName =
  nickname || "Anonymous";

console.log(displayName);
// Anonymous
```

Values such as `null`, `undefined`, and `0` are treated as falsy in JavaScript.

### AND Operator `&&`

If the left value is truthy, the right expression is evaluated and returned.

```javascript
const score = 85;

score > 80 &&
  console.log("Excellent score");
```

It can also be used for conditional expressions.

```javascript
const age = 20;

const message =
  age >= 18 && "Adult";
```

---

## 20. DOM Manipulation with Modern JavaScript

Modern JavaScript syntax can be combined with DOM manipulation.

HTML:

```html
<div id="app">
  <h1>Dynamic Web Page with ES6</h1>

  <div class="user-card">
    <h2 id="userName"></h2>
    <p id="userStatus"></p>
  </div>

  <button id="toggleBtn">
    Change Login Status
  </button>
</div>
```

JavaScript:

```javascript
const userNameEl =
  document.getElementById("userName");

const userStatusEl =
  document.getElementById("userStatus");

const toggleBtn =
  document.getElementById("toggleBtn");

let isLoggedIn = false;

const userProfile = {
  name: "Gildong",
  isMember: true,
  age: 30,
};

const { name, isMember } = userProfile;

const updateUI = () => {
  const statusMessage = isLoggedIn
    ? "Welcome!"
    : "Please log in.";

  const nameToDisplay = isLoggedIn
    ? name
    : "Visitor";

  userNameEl.textContent =
    `Hello, ${nameToDisplay}!`;

  userStatusEl.textContent =
    statusMessage;

  isLoggedIn &&
    (userStatusEl.style.color = "green");

  !isLoggedIn &&
    (userStatusEl.style.color = "red");
};

toggleBtn.addEventListener(
  "click",
  () => {
    isLoggedIn = !isLoggedIn;
    updateUI();
  }
);

updateUI();
```

This example combines several ES6 features:

- `const` and `let`
- Object destructuring
- Arrow functions
- Template literals
- Ternary operator
- Logical `&&`
- DOM manipulation
- Event listeners

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# Modern JavaScript

## 1. ES6와 모던 JavaScript

모던 JavaScript는 일반적으로 **ECMAScript 6(ES6) 이후의 JavaScript 문법**을 의미합니다.

ES6는 ECMAScript 2015로 발표되었으며 이후 JavaScript 표준에는 지속적으로 새로운 기능이 추가되었습니다.

모던 웹 개발에서는 다음과 같은 기술을 함께 사용합니다.

- React, Vue, Angular
- npm, Yarn
- Webpack
- Babel
- SPA 구조
- 함수형 컴포넌트와 Hooks
- 불변성을 고려한 데이터 업데이트

React를 사용하기 위해서는 ES6 문법과 `map()`, `filter()`, `reduce()` 등의 배열 고차함수를 이해하는 것이 중요합니다.

---

## 2. `let`과 `const`

모던 JavaScript에서는 일반적으로 `var` 대신 `let`과 `const`를 사용합니다.

### `var`

- 함수 스코프
- 재선언 가능
- 재할당 가능
- 호이스팅과 스코프로 인해 예상하지 못한 문제가 발생할 수 있음

```javascript
var value = "first";

value = "second";

var value = "third";
```

### `let`

`let`은 블록 스코프를 가지며 재할당이 가능합니다.

```javascript
let value = "first";

value = "second";
```

같은 스코프에서 재선언할 수 없습니다.

```javascript
let value = "first";

// SyntaxError
let value = "second";
```

### `const`

`const`는 블록 스코프를 가지며 변수 자체를 재할당할 수 없습니다.

```javascript
const value = "React";
```

```javascript
// TypeError
value = "JavaScript";
```

---

## 3. 객체와 배열에서의 `const`

객체나 배열을 `const`로 선언하면 변수 자체는 다른 값으로 재할당할 수 없습니다.

하지만 객체의 속성이나 배열 내부 요소는 변경할 수 있습니다.

### 객체

```javascript
const user = {
  name: "Popcorn",
  age: 24,
};

user.name = "React";
user.address = "Seoul";
```

### 배열

```javascript
const animals = ["dog", "cat"];

animals[0] = "bird";
animals.push("monkey");
```

즉, 변수에 저장된 참조는 유지되지만 내부 데이터는 변경할 수 있습니다.

---

## 4. 호이스팅

**호이스팅(Hoisting)**은 코드 실행 전에 변수나 함수 선언이 먼저 처리되는 것처럼 동작하는 특성을 의미합니다.

`var`는 선언 단계에서 `undefined`로 초기화됩니다.

```javascript
console.log(myVar);
// undefined

var myVar = "React";
```

`let`과 `const` 역시 호이스팅되지만 같은 방식으로 초기화되지 않습니다.

선언 전에 접근하면 오류가 발생합니다.

```javascript
// ReferenceError
console.log(myLet);

let myLet = "React";
```

---

## 5. 템플릿 리터럴

템플릿 리터럴은 백틱 `` ` ` ``과 `${}`를 이용해 문자열 안에 변수나 표현식을 삽입하는 문법입니다.

기존 문자열 결합 방식:

```javascript
const name = "Popcorn";
const age = 24;

const message =
  "내 이름은 " + name +
  "입니다. 나이는 " +
  age + "세입니다.";
```

템플릿 리터럴:

```javascript
const message =
  `내 이름은 ${name}입니다. 나이는 ${age}세입니다.`;
```

함수 호출이나 계산식도 사용할 수 있습니다.

```javascript
const sayHello = () =>
  "안녕하세요";

const month = 1;

const message =
  `${sayHello()}! ${month * 8}월입니다.`;
```

---

## 6. 화살표 함수

화살표 함수는 함수를 간결하게 작성하기 위한 ES6 문법입니다.

기존 함수:

```javascript
const add = function (a, b) {
  return a + b;
};
```

화살표 함수:

```javascript
const add = (a, b) => {
  return a + b;
};
```

실행문이 하나의 표현식일 경우 `{}`와 `return`을 생략할 수 있습니다.

```javascript
const multiply =
  (a, b) => a * b;
```

매개변수가 하나라면 `()`도 생략할 수 있습니다.

```javascript
const square =
  num => num * num;
```

### 객체 반환

객체를 바로 반환하려면 `()`로 감쌉니다.

```javascript
const createUser =
  (name, age) => ({
    name: name,
    age: age,
  });
```

화살표 함수의 `this`는 함수가 선언된 스코프의 `this`를 상속합니다.

---

## 7. 구조 분해 할당

구조 분해 할당은 객체나 배열의 값을 각각의 변수로 분리하여 저장하는 문법입니다.

### 객체 구조 분해 할당

```javascript
const profile = {
  name: "Popcorn",
  age: 24,
};

const { name, age } = profile;
```

객체는 속성명을 기준으로 추출하기 때문에 순서를 변경할 수 있습니다.

```javascript
const { age, name } = profile;
```

새로운 변수명을 지정할 수도 있습니다.

```javascript
const {
  name: newName,
  age: newAge,
} = profile;
```

### 배열 구조 분해 할당

배열은 저장된 순서를 기준으로 값을 변수에 할당합니다.

```javascript
const profile = [
  "Popcorn",
  24,
];

const [name, age] = profile;
```

필요한 요소만 추출할 수도 있습니다.

```javascript
const [name] = profile;
```

---

## 8. 디폴트값

함수의 인수나 객체의 속성이 존재하지 않을 경우 기본값을 지정할 수 있습니다.

### 함수 매개변수

```javascript
const sayHello =
  (name = "게스트") => {
    console.log(
      `${name}님, 안녕하세요`
    );
  };

sayHello();
sayHello("Popcorn");
```

### 객체 구조 분해 할당

```javascript
const profile = {
  age: 24,
};

const {
  name = "게스트"
} = profile;

console.log(
  `${name}님 안녕하세요`
);
```

값이 존재하지 않을 때 `undefined`가 출력되는 상황을 줄일 수 있습니다.

---

## 9. 전개 구문

전개 구문은 `...`을 이용하여 배열의 요소나 객체의 속성을 개별적으로 펼치는 문법입니다.

### 배열 합치기

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

const combined = [
  ...arr1,
  ...arr2,
];

console.log(combined);
// [1, 2, 3, 4]
```

배열 중간에 새로운 요소를 추가할 수도 있습니다.

```javascript
const newArray = [
  0,
  ...arr1,
  3,
];
```

### 배열 복사

```javascript
const original = [10, 20];

const copied = [
  ...original
];

copied[0] = 100;
```

새로운 배열을 만들기 때문에 원본 배열과 분리됩니다.

### 객체 병합

```javascript
const user = {
  name: "React",
  age: 10,
};

const location = {
  city: "Seoul",
};

const profile = {
  ...user,
  ...location,
};
```

### 객체 속성 변경

```javascript
const updatedUser = {
  ...user,
  age: 11,
};
```

이 방식은 React에서 기존 객체를 직접 변경하지 않고 새로운 객체를 생성할 때 자주 사용됩니다.

---

## 10. 함수 호출과 전개 구문

배열의 요소를 개별 함수 인수로 펼쳐 전달할 수 있습니다.

```javascript
const numbers = [1, 2];

const sum =
  (num1, num2) =>
    num1 + num2;

console.log(
  sum(...numbers)
);
```

---

## 11. 나머지 매개변수

나머지 매개변수 역시 `...`을 사용하지만 여러 인수를 하나의 배열로 모으는 역할을 합니다.

```javascript
const sum = (...numbers) => {
  return numbers.reduce(
    (acc, number) =>
      acc + number,
    0
  );
};

console.log(sum(1, 2));
console.log(sum(1, 2, 3));
```

나머지 매개변수는 함수의 마지막 매개변수로 사용해야 합니다.

```javascript
const introduce =
  (name, ...hobbies) => {
    console.log(
      `제 이름은 ${name}입니다.`
    );

    console.log(hobbies);
  };

introduce(
  "React",
  "코딩",
  "운동"
);
```

### Spread와 Rest의 차이

```text
Spread
배열/객체 → 개별 요소로 펼침

Rest
여러 값 → 하나의 배열로 모음
```

---

## 12. 객체 생략 표기법

객체의 속성명과 변수명이 같으면 `속성명: 변수명` 형태를 생략할 수 있습니다.

기존 방식:

```javascript
const name = "Popcorn";
const age = 24;

const user = {
  name: name,
  age: age,
};
```

생략 표기법:

```javascript
const user = {
  name,
  age,
};
```

---

## 13. 함수형 배열 메소드

함수형 메소드는 원본 데이터를 직접 수정하지 않고 새로운 데이터를 반환하는 방식으로 사용할 수 있습니다.

대표적인 메소드:

- `map()`
- `filter()`
- `reduce()`

React에서 데이터를 가공하거나 상태를 관리할 때 중요한 개념입니다.

---

## 14. `map()`

`map()`은 배열의 모든 요소를 순회하면서 각 요소에 함수를 적용하고 새로운 배열을 반환합니다.

```javascript
const numbers = [
  1,
  2,
  3,
  4,
];

const doubled =
  numbers.map(
    number => number * 2
  );
```

객체 배열에서 특정 속성만 추출할 수도 있습니다.

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
];

const names =
  users.map(
    user => user.name
  );

console.log(names);
```

콜백 함수에서 인덱스를 사용할 수도 있습니다.

```javascript
const names = [
  "Popcorn",
  "Vue",
  "Ant",
];

names.map(
  (name, index) => {
    console.log(
      `${index + 1}번째: ${name}`
    );
  }
);
```

---

## 15. `filter()`

`filter()`는 조건을 만족하는 요소만 골라 새로운 배열을 반환합니다.

```javascript
const numbers = [
  1,
  2,
  3,
  4,
  5,
];

const oddNumbers =
  numbers.filter(
    number =>
      number % 2 === 1
  );

console.log(oddNumbers);
// [1, 3, 5]
```

---

## 16. `reduce()`

`reduce()`는 배열의 각 요소를 처리하여 하나의 값으로 줄이는 함수입니다.

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

console.log(sum);
// 10
```

숫자뿐 아니라 문자열, 배열, 객체 등으로 변환할 수도 있습니다.

```javascript
const students = [
  {
    id: 1,
    name: "Kim",
    score: 64,
  },
  {
    id: 2,
    name: "Lee",
    score: 91,
  },
  {
    id: 3,
    name: "Park",
    score: 82,
  },
];

const scores =
  students.reduce(
    (acc, student) => {
      acc[student.name] =
        student.score;

      return acc;
    },
    {}
  );

console.log(scores);
```

결과:

```javascript
{
  Kim: 64,
  Lee: 91,
  Park: 82
}
```

---

## 17. 메소드 체이닝

여러 배열 메소드를 연결하여 데이터 처리 과정을 구성할 수 있습니다.

```javascript
const students = [
  {
    id: 1,
    name: "Kim",
    score: 64,
  },
  {
    id: 2,
    name: "Lee",
    score: 91,
  },
  {
    id: 3,
    name: "Park",
    score: 82,
  },
];

const totalScore =
  students
    .map(student => ({
      ...student,
      score:
        student.score + 10,
    }))
    .filter(
      student =>
        student.score >= 80
    )
    .reduce(
      (acc, student) =>
        acc + student.score,
      0
    );

console.log(totalScore);
```

처리 과정은 다음과 같습니다.

```text
원본 배열
   ↓
map()
   ↓
데이터 변환
   ↓
filter()
   ↓
조건에 맞는 데이터 선택
   ↓
reduce()
   ↓
하나의 결과값
```

---

## 18. 삼항 연산자

삼항 연산자는 조건에 따라 서로 다른 값을 반환할 때 사용할 수 있습니다.

기본 문법:

```javascript
조건
  ? true일 때 값
  : false일 때 값
```

예시:

```javascript
const isLoggedIn = true;

const message =
  isLoggedIn
    ? "환영합니다!"
    : "로그인해주세요.";
```

함수에서도 사용할 수 있습니다.

```javascript
const result =
  num =>
    num % 2 === 0
      ? "even"
      : "odd";
```

---

## 19. 논리 연산자

### OR 연산자 `||`

왼쪽 값이 falsy로 판단되면 오른쪽 값을 반환합니다.

```javascript
const nickname = null;

const displayName =
  nickname || "익명";

console.log(displayName);
// 익명
```

JavaScript에서는 `null`, `undefined`, `0` 등이 falsy로 판단됩니다.

### AND 연산자 `&&`

왼쪽 값이 truthy이면 오른쪽 표현식을 평가하고 반환합니다.

```javascript
const score = 85;

score > 80 &&
  console.log(
    "우수한 성적입니다."
  );
```

조건부 값을 만들 때도 사용할 수 있습니다.

```javascript
const age = 20;

const message =
  age >= 18 &&
  "성인입니다.";
```

---

## 20. Modern JavaScript와 DOM 조작

모던 JavaScript 문법은 DOM 조작과 함께 사용할 수 있습니다.

HTML:

```html
<div id="app">
  <h1>
    ES6와 함께하는 동적 웹페이지
  </h1>

  <div class="user-card">
    <h2 id="userName"></h2>
    <p id="userStatus"></p>
  </div>

  <button id="toggleBtn">
    로그인 상태 변경
  </button>
</div>
```

JavaScript:

```javascript
const userNameEl =
  document.getElementById(
    "userName"
  );

const userStatusEl =
  document.getElementById(
    "userStatus"
  );

const toggleBtn =
  document.getElementById(
    "toggleBtn"
  );

let isLoggedIn = false;

const userProfile = {
  name: "홍길동",
  isMember: true,
  age: 30,
};

const {
  name,
  isMember
} = userProfile;

const updateUI = () => {
  const statusMessage =
    isLoggedIn
      ? "환영합니다!"
      : "로그인해주세요.";

  const nameToDisplay =
    isLoggedIn
      ? name
      : "방문자";

  userNameEl.textContent =
    `안녕하세요, ${nameToDisplay}님!`;

  userStatusEl.textContent =
    statusMessage;

  isLoggedIn &&
    (
      userStatusEl.style.color =
        "green"
    );

  !isLoggedIn &&
    (
      userStatusEl.style.color =
        "red"
    );
};

toggleBtn.addEventListener(
  "click",
  () => {
    isLoggedIn =
      !isLoggedIn;

    updateUI();
  }
);

updateUI();
```

이 예제에서는 다음 ES6 문법을 함께 사용합니다.

- `const`, `let`
- 객체 구조 분해 할당
- 화살표 함수
- 템플릿 리터럴
- 삼항 연산자
- 논리곱 `&&`
- DOM 조작
- 이벤트 리스너

</details>
