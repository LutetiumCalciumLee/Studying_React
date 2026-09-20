<details>
<summary>ENG (English Version)</summary>

# React Overview

## 1. SPA vs MPA

### MPA (Multi Page Application)

MPA is the traditional web application development approach.

- Commonly built using server-side technologies such as JSP and PHP
- Each page request retrieves a new HTML page from the server
- Initial loading can be fast because the server provides the elements required for rendering
- Frequent page requests can increase server workload
- Moving between pages usually causes a full page reload

### SPA (Single Page Application)

SPA loads the static resources required by the application initially and dynamically updates the page afterward.

- Static resources are downloaded when the application first loads
- Only necessary data is retrieved when the page changes
- Data is commonly exchanged in JSON format
- The page can be updated without a full reload
- Provides a smoother user experience
- UI elements can be divided into reusable components

---

## 2. Web Page Rendering

A browser renders a web page through the following process:

1. The **HTML Parser** reads HTML and creates the **DOM Tree**
2. The **CSS Parser** reads CSS and creates the **CSSOM**
3. The DOM and CSSOM are combined to create the **Render Tree**
4. The browser performs **Painting** based on the Render Tree
5. The final result is displayed on the screen

When the HTML parser encounters a `<script>` tag, HTML parsing can temporarily stop while the JavaScript file is loaded.

---

## 3. SSR vs CSR

### SSR (Server Side Rendering)

SSR generates the completed HTML on the server and sends it to the browser.

```text
Server
  ↓
Completed HTML
  ↓
Browser
  ↓
Display
```

### CSR (Client Side Rendering)

CSR uses JavaScript in the browser to render the web page.

```text
Server
  ↓
JavaScript
  ↓
Browser executes JavaScript
  ↓
Page rendering
```

React applications can use client-side rendering to dynamically update the screen.

---

## 4. Why React?

Traditional web development using **HTML + JavaScript + jQuery** has several limitations.

- Full page reloads when navigating between pages
- More complicated DOM manipulation
- Increased amount of code
- Higher possibility of errors
- Difficult UI state management

React provides several approaches to address these problems.

- SPA-based user experience
- Virtual DOM
- Component-based architecture
- Reusable UI components
- State-based declarative UI

### Imperative vs Declarative UI

Traditional JavaScript often directly manipulates DOM elements.

React focuses on describing what the UI should look like.

```jsx
function App() {
  return <h1>Hello!</h1>;
}
```

---

## 5. Virtual DOM

The **Virtual DOM** is a lightweight representation of the actual DOM.

The real DOM contains the information required by the browser to render the page, while the Virtual DOM allows React to calculate changes in memory before updating the real DOM.

### How It Works

```text
State Change
    ↓
Virtual DOM Update
    ↓
Compare with Previous Virtual DOM
    ↓
Diffing
    ↓
Reconciliation
    ↓
Update Required Parts of the Real DOM
```

Main concepts:

- **Virtual DOM**: Lightweight copy of the real DOM
- **Diffing**: Compares the previous and updated Virtual DOM
- **Reconciliation**: Applies the required changes to the real DOM

This reduces unnecessary DOM manipulation.

---

## 6. Component-Based Development

React divides the UI into small reusable pieces called **components**.

Advantages:

- Code reuse
- Easier maintenance
- Improved development efficiency
- Easier collaboration

Example:

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

The same component can be reused with different data.

```jsx
<Welcome name="Gildong" />
<Welcome name="Younghee" />
```

---

## 7. React Development Environment

The basic development environment introduced in this branch consists of:

- **VS Code**: Code editor
- **Node.js**: JavaScript runtime
- **npm**: Package manager included with Node.js
- **Yarn**: Alternative JavaScript package manager
- **Prettier**: Code formatter

### Node.js

Node.js is a JavaScript runtime built on the Chrome V8 JavaScript engine.

It allows JavaScript to run outside the web browser.

Check the installed version:

```bash
node -v
npm -v
```

---

## 8. Role of Node.js in React Development

### Package Management

React projects depend on various libraries and development tools.

Packages can be installed and managed using npm or Yarn.

```bash
npm install package-name
```

### Build Tools

React code may contain JSX and modern ECMAScript syntax.

Build tools convert or bundle the source code so that it can be executed in the browser.

Examples introduced in this branch:

- **Babel**: Converts modern JavaScript and JSX
- **Webpack**: Bundles JavaScript, CSS, images, and other resources

### Development Server

A Node.js-based development server detects source code changes and updates the application during development.

---

## 9. Creating a React Project

A React project can be created using Create React App.

```bash
npx create-react-app my-react-app
```

Move to the project directory:

```bash
cd my-react-app
```

Start the development server:

```bash
npm start
```

Project names should be written in lowercase.

---

## 10. Basic Project Structure

```text
my-react-app/
├── public/
│   └── index.html
│
├── src/
│   ├── App.js
│   └── index.js
│
└── package.json
```

### `public/`

Contains static resources.

### `src/`

Contains the main application source code.

### `App.js`

Contains the main UI component.

Example:

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}

export default App;
```

### `index.js`

Acts as the starting point for root rendering.

---

## 11. Prettier

**Prettier** is a code formatter that automatically organizes code formatting and indentation.

It can be installed from the VS Code Extensions menu.

```text
Ctrl + Shift + X
```

Search for:

```text
Prettier
```

---

## 12. npm and Yarn

Both npm and Yarn are JavaScript package management tools.

### npm

Main features:

- Install packages
- Remove packages
- Manage dependencies
- Run scripts
- Publish packages

```bash
npm install package-name
npm uninstall package-name
npm start
```

### Yarn

Yarn provides similar package management functionality.

Features introduced in this branch:

- Package caching
- Fast package installation
- Dependency consistency using `yarn.lock`

Install Yarn:

```bash
npm install -g yarn
```

### Command Comparison

| Function | npm | Yarn |
|---|---|---|
| Install dependencies | `npm install` | `yarn install` |
| Add package | `npm install package-name` | `yarn add package-name` |
| Start application | `npm start` | `yarn start` |

---

## 13. React Page Rendering Process

A React SPA uses `index.html` as the base HTML document.

The HTML file contains a root element.

```html
<div id="root"></div>
```

The actual UI is defined in components such as `App.js`.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}
```

`index.js` imports the `App` component and renders it inside the `root` element.

```jsx
const root = ReactDOM.createRoot(
  document.getElementById("root")
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

The overall rendering process is:

```text
index.html
    ↓
<div id="root">
    ↓
index.js
    ↓
App.js
    ↓
React renders the component
    ↓
Browser Screen
```

A Single Page Application keeps one main HTML page and changes the displayed content by rendering different components.

---

## 14. Practice

Display a welcome message containing a name.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, Gildong</h1>
      <h1>Welcome, React!!!</h1>
    </div>
  );
}

export default App;
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# React 개요

## 1. SPA vs MPA

### MPA (Multi Page Application)

MPA는 전통적인 웹 애플리케이션 개발 방식입니다.

- JSP, PHP 등의 서버 측 기술을 사용하여 웹사이트 구성
- 페이지를 요청할 때마다 서버에서 새로운 HTML 페이지를 전달
- 렌더링에 필요한 요소를 서버에서 받아 초기 화면을 구성
- 페이지 요청이 많아질수록 서버 부담 증가
- 페이지 이동 시 전체 페이지가 새로고침됨

### SPA (Single Page Application)

SPA는 웹 애플리케이션에 필요한 정적 리소스를 처음에 불러온 후 필요한 부분만 갱신하는 방식입니다.

- 최초 실행 시 필요한 정적 리소스를 다운로드
- 페이지 변경 시 필요한 데이터만 추가로 요청
- 데이터는 주로 JSON 형식으로 전달
- 전체 페이지를 다시 로드하지 않고 화면 갱신 가능
- 사용자 경험 향상
- UI 요소를 컴포넌트 단위로 분리하고 재사용 가능

---

## 2. 웹페이지 렌더링

브라우저는 다음 과정을 통해 웹페이지를 화면에 표시합니다.

1. **HTML Parser**가 HTML을 읽고 **DOM Tree** 생성
2. **CSS Parser**가 CSS를 읽고 **CSSOM** 생성
3. DOM과 CSSOM을 결합하여 **Render Tree** 생성
4. Render Tree를 기반으로 **Painting**
5. 실제 화면에 결과 출력

HTML을 읽는 과정에서 `<script>` 태그를 만나면 HTML 파싱을 잠시 중단하고 JavaScript 파일을 로드할 수 있습니다.

---

## 3. SSR vs CSR

### SSR (Server Side Rendering)

SSR은 서버에서 완성된 HTML을 생성하여 브라우저로 전달하는 방식입니다.

```text
Server
  ↓
완성된 HTML
  ↓
Browser
  ↓
화면 출력
```

### CSR (Client Side Rendering)

CSR은 웹 브라우저에서 JavaScript를 실행하여 웹페이지를 렌더링하는 방식입니다.

```text
Server
  ↓
JavaScript
  ↓
브라우저에서 JavaScript 실행
  ↓
페이지 렌더링
```

---

## 4. 왜 React인가?

기존의 **HTML + JavaScript + jQuery** 기반 웹 개발에는 다음과 같은 문제가 있습니다.

- 페이지 이동 시 전체 새로고침 발생
- 복잡한 DOM 조작
- 코드 양 증가
- 오류 발생 가능성 증가
- UI 상태 관리가 어려움

React에서는 다음과 같은 방식을 사용합니다.

- SPA 기반 사용자 경험
- Virtual DOM
- 컴포넌트 기반 구조
- UI 코드 재사용
- State 기반 선언형 UI

### 명령형 UI와 선언형 UI

기존 JavaScript 방식에서는 DOM을 직접 조작하는 경우가 많습니다.

React에서는 화면이 어떤 모습이어야 하는지를 선언하는 방식으로 UI를 작성합니다.

```jsx
function App() {
  return <h1>Hello!</h1>;
}
```

---

## 5. Virtual DOM

**Virtual DOM**은 실제 DOM의 가벼운 사본입니다.

실제 DOM에는 브라우저가 화면을 그리는 데 필요한 정보가 들어 있으며, React에서는 메모리상의 Virtual DOM에서 변경 사항을 계산한 후 필요한 부분을 실제 DOM에 반영합니다.

### 동작 과정

```text
State 변경
    ↓
Virtual DOM 변경
    ↓
이전 Virtual DOM과 비교
    ↓
Diffing
    ↓
Reconciliation
    ↓
필요한 부분만 실제 DOM에 반영
```

주요 개념:

- **Virtual DOM**: 실제 DOM의 가벼운 사본
- **Diffing**: 이전 Virtual DOM과 변경된 Virtual DOM 비교
- **Reconciliation**: 필요한 변경 사항을 실제 DOM에 반영

이를 통해 불필요한 DOM 조작을 줄일 수 있습니다.

---

## 6. 컴포넌트 기반 개발

React는 UI를 작은 단위인 **컴포넌트(Component)**로 나누어 개발합니다.

장점:

- 코드 재사용 가능
- 유지보수 용이
- 개발 효율 향상
- 협업 효율 향상

예시:

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

하나의 컴포넌트에 서로 다른 데이터를 전달하여 재사용할 수 있습니다.

```jsx
<Welcome name="Gildong" />
<Welcome name="Younghee" />
```

---

## 7. React 개발 환경 구성

React 개발을 위해 다음 도구를 사용합니다.

- **VS Code**: 코드 편집기
- **Node.js**: JavaScript 런타임
- **npm**: Node.js에 포함된 패키지 관리 도구
- **Yarn**: JavaScript 패키지 관리 도구
- **Prettier**: 코드 포맷터

### Node.js

Node.js는 Chrome V8 JavaScript 엔진을 기반으로 만들어진 JavaScript 런타임입니다.

웹 브라우저 외부에서도 JavaScript를 실행할 수 있도록 합니다.

설치 확인:

```bash
node -v
npm -v
```

---

## 8. React 개발에서 Node.js의 역할

### 패키지 관리

React 프로젝트에서는 다양한 라이브러리와 개발 도구를 사용합니다.

npm 또는 Yarn을 이용하여 필요한 패키지를 설치하고 관리할 수 있습니다.

```bash
npm install package-name
```

### 빌드 도구 실행

React에서는 JSX와 최신 ECMAScript 문법을 사용할 수 있습니다.

이러한 코드를 브라우저에서 실행할 수 있도록 변환하거나 여러 파일을 묶는 빌드 도구를 사용합니다.

이 Branch에서 다룬 대표적인 도구:

- **Babel**: 최신 JavaScript 문법과 JSX 변환
- **Webpack**: JavaScript, CSS, 이미지 등의 파일을 묶는 모듈 번들러

### 개발 서버 실행

Node.js 기반 개발 서버는 코드 변경 사항을 감지하여 개발 중인 애플리케이션에 반영합니다.

---

## 9. React 프로젝트 생성

Create React App을 사용하여 React 프로젝트를 생성합니다.

```bash
npx create-react-app my-react-app
```

생성된 프로젝트 폴더로 이동합니다.

```bash
cd my-react-app
```

개발 서버를 실행합니다.

```bash
npm start
```

프로젝트 이름은 소문자로 작성합니다.

---

## 10. 기본 프로젝트 구조

```text
my-react-app/
├── public/
│   └── index.html
│
├── src/
│   ├── App.js
│   └── index.js
│
└── package.json
```

### `public/`

정적 리소스를 저장합니다.

### `src/`

React 애플리케이션의 개발 코드를 저장합니다.

### `App.js`

주요 화면 컴포넌트를 작성합니다.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}

export default App;
```

### `index.js`

React 애플리케이션의 루트 렌더링 시작점입니다.

---

## 11. Prettier

**Prettier**는 코드의 들여쓰기와 형식을 자동으로 정리해주는 코드 포맷터입니다.

VS Code의 Extensions 메뉴에서 설치할 수 있습니다.

```text
Ctrl + Shift + X
```

검색:

```text
Prettier
```

---

## 12. npm과 Yarn

npm과 Yarn은 모두 JavaScript 패키지를 관리하는 도구입니다.

### npm

주요 기능:

- 패키지 설치
- 패키지 제거
- 의존성 관리
- Script 실행
- 패키지 배포

```bash
npm install package-name
npm uninstall package-name
npm start
```

### Yarn

Yarn 역시 JavaScript 패키지를 관리하는 도구입니다.

이 Branch에서 다룬 특징:

- 패키지 캐시
- 빠른 설치
- `yarn.lock`을 이용한 의존성 일관성 유지

Yarn 설치:

```bash
npm install -g yarn
```

### 명령어 비교

| 기능 | npm | Yarn |
|---|---|---|
| 의존성 설치 | `npm install` | `yarn install` |
| 패키지 추가 | `npm install package-name` | `yarn add package-name` |
| 프로젝트 실행 | `npm start` | `yarn start` |

---

## 13. React 페이지 구동 과정

React SPA에서는 `index.html`이 기본 HTML 문서 역할을 합니다.

HTML에는 React 애플리케이션이 렌더링될 `root` 요소가 존재합니다.

```html
<div id="root"></div>
```

실제 화면 구성은 `App.js`와 같은 컴포넌트에 작성합니다.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
    </div>
  );
}
```

`index.js`에서는 `App` 컴포넌트를 가져와 `root` 요소에 렌더링합니다.

```jsx
const root = ReactDOM.createRoot(
  document.getElementById("root")
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

전체적인 구동 과정은 다음과 같습니다.

```text
index.html
    ↓
<div id="root">
    ↓
index.js
    ↓
App.js
    ↓
React Component Rendering
    ↓
브라우저 화면
```

SPA는 하나의 HTML 페이지를 유지하면서 렌더링되는 컴포넌트를 변경하여 서로 다른 화면을 표시할 수 있습니다.

---

## 14. 실습

이름을 포함한 환영 메시지를 화면에 출력합니다.

```jsx
function App() {
  return (
    <div>
      <h1>Hello, Gildong</h1>
      <h1>Welcome, React!!!</h1>
    </div>
  );
}

export default App;
```

</details>
