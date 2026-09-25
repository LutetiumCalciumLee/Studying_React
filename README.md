<details>

<summary>ENG (English Version)</summary>

# React Router

## 1. What is React Router?

React Router is a library used to implement routing in React Single Page Applications.

In an SPA, changing the URL does not require a full page reload. React Router changes the displayed component according to the current URL.

```text
URL Change
    ↓
React Router
    ↓
Route Matching
    ↓
Component Rendering
```

Example:

```text
/
→ Home

/about
→ About

/products
→ Products
```

---

## 2. Routing

**Routing** means selecting content according to a requested URL path.

In React:

```text
URL
  ↓
Route Matching
  ↓
Component
```

Example:

```text
/
→ Home Component

/about
→ About Component

/user/101
→ User Detail Component
```

---

## 3. Main React Router Components

The main components introduced in this branch are:

- `BrowserRouter`
- `Routes`
- `Route`
- `Link`
- `Navigate`

### `BrowserRouter`

Uses the browser's History API to manage URLs and enables routing in the React application.

### `Routes`

Contains multiple `Route` components and finds the route that matches the current URL.

### `Route`

Defines which component should be rendered for a specific URL path.

### `Link`

Provides navigation between routes.

### `Navigate`

Redirects the user to another route.

---

## 4. Installing React Router

Install `react-router-dom` in the React project.

```bash
npm install react-router-dom
```

---

## 5. Basic Project Structure

A simple React Router project can be organized as follows.

```text
my-react-app/
└── src/
    ├── App.js
    ├── Nav.js
    └── pages/
        ├── Home.js
        ├── About.js
        └── NotFound.js
```

Each page is separated into its own component.

---

## 6. `BrowserRouter`

The application should be wrapped with `BrowserRouter` to enable routing.

```jsx
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";

import App from "./App";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);

root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

Basic structure:

```text
BrowserRouter
    ↓
App
    ↓
Routes
    ↓
Route
```

`BrowserRouter` also supports browser back and forward navigation.

---

## 7. `Routes`

`Routes` contains multiple Route definitions.

```jsx
import {
  Routes,
  Route,
} from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route
        path="/"
        element={<Home />}
      />

      <Route
        path="/about"
        element={<About />}
      />
    </Routes>
  );
}
```

When the URL changes, React Router checks the routes and renders the matching component.

---

## 8. `Route`

A `Route` mainly uses two props.

```text
path
→ URL path

element
→ Component to render
```

Example:

```jsx
<Route
  path="/"
  element={<Home />}
/>
```

This means:

```text
URL: /
     ↓
<Home />
```

Another example:

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

```text
URL: /about
     ↓
<About />
```

---

## 9. Root and Sub Paths

The root path `/` represents the main page.

```jsx
<Route
  path="/"
  element={<Home />}
/>
```

Example:

```text
https://www.example.com/
```

A sub path can be defined as:

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

Example:

```text
https://www.example.com/about
```

---

## 10. `Link`

`Link` is used to move between routes.

```jsx
import {
  Link,
} from "react-router-dom";

function Nav() {
  return (
    <div>
      <Link to="/">
        Home
      </Link>

      <Link to="/about">
        About
      </Link>
    </div>
  );
}
```

The destination is specified using the `to` prop.

```text
<Link to="/about">
        ↓
URL → /about
        ↓
React Router checks Routes
        ↓
<About /> rendered
```

---

## 11. `Link` and `Route` Flow

When the user clicks:

```jsx
<Link to="/about">
  About
</Link>
```

the routing process is:

```text
User Click
    ↓
Link
    ↓
URL → /about
    ↓
Routes
    ↓
Route path="/about"
    ↓
<About />
```

The page content changes without replacing the entire page.

---

## 12. Handling Invalid URLs

A wildcard route can be used for URLs that do not match any defined path.

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="*"
    element={<NotFound />}
  />
</Routes>
```

`path="*"` handles unmatched routes.

```text
/unknown
    ↓
No Matching Route
    ↓
*
    ↓
NotFound
```

---

## 13. 404 Page

A `NotFound` component can be displayed for invalid URLs.

```jsx
function NotFound() {
  return (
    <div>
      <h1>404</h1>

      <p>
        Page not found.
      </p>
    </div>
  );
}
```

Route:

```jsx
<Route
  path="*"
  element={<NotFound />}
/>
```

---

## 14. Redirecting Invalid URLs

Instead of displaying a 404 page, invalid URLs can be redirected to another route.

```jsx
import {
  Navigate,
} from "react-router-dom";

<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="*"
    element={
      <Navigate
        to="/"
        replace
      />
    }
  />
</Routes>
```

This redirects unmatched URLs to the home page.

---

## 15. `Navigate`

`Navigate` redirects the user when the component is rendered.

Basic syntax:

```jsx
<Navigate
  to="/login"
  replace
/>
```

Main props:

| Prop | Description |
| --- | --- |
| `to` | Destination URL |
| `replace` | Replaces the current browser history entry |

Using `replace` removes the current route from the browser history and replaces it with the new route.

---

## 16. Conditional Redirect

`Navigate` can redirect users depending on a condition.

```jsx
import {
  Navigate,
} from "react-router-dom";

function UserProfile({
  isLoggedIn,
}) {
  if (!isLoggedIn) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  return (
    <div>
      <h1>
        User Profile
      </h1>

      <p>
        Welcome!
      </p>
    </div>
  );
}
```

Flow:

```text
isLoggedIn?
   │
   ├── true
   │   → Profile
   │
   └── false
       → /login
```

---

## 17. Role-Based Redirect

Routing can also depend on the user's role.

```jsx
function AdminPage({
  userRole,
}) {
  if (
    userRole !== "admin"
  ) {
    return (
      <Navigate
        to="/"
        replace
      />
    );
  }

  return (
    <div>
      <h1>
        Admin Dashboard
      </h1>

      <p>
        Administrator-only
        information.
      </p>
    </div>
  );
}
```

If the user is not an administrator, the page redirects to the main route.

---

## 18. Route Parameters

A **Route Parameter** places a variable-like value inside a URL.

It is useful when multiple items use the same page structure but have different IDs.

Examples:

- Blog posts
- Products
- User profiles

Route parameters are defined using `:`.

```jsx
<Route
  path="/posts/:postId"
  element={<PostDetail />}
/>
```

Possible URLs:

```text
/posts/1
/posts/2
/posts/100
```

A single Route can therefore handle many detail pages.

---

## 19. User Detail Route

Example:

```jsx
<Routes>
  <Route
    path="/"
    element={<UserList />}
  />

  <Route
    path="/user/:userId"
    element={<UserDetail />}
  />
</Routes>
```

Possible URLs:

```text
/user/101
/user/202
```

Here:

```text
:userId
```

is the Route Parameter.

---

## 20. `useNavigate()`

`useNavigate()` provides a function that moves the user to another route through JavaScript.

```jsx
import {
  useNavigate,
} from "react-router-dom";

function UserList() {
  const navigate =
    useNavigate();

  const users = [
    {
      id: 101,
      name: "Kim React",
    },
    {
      id: 202,
      name: "Park Router",
    },
  ];

  const goToUserDetail =
    id => {
      navigate(
        `/user/${id}`
      );
    };

  return (
    <div>
      <h2>
        User List
      </h2>

      {users.map(
        user => (
          <button
            key={user.id}
            onClick={() =>
              goToUserDetail(
                user.id
              )
            }
          >
            {user.name}
            {" "}
            ({user.id})
            Details
          </button>
        )
      )}
    </div>
  );
}
```

A dynamic URL is created using a template literal.

```javascript
navigate(
  `/user/${id}`
);
```

---

## 21. `Link` vs `useNavigate()`

Both can be used for route navigation.

```text
Link
→ Navigation through JSX
→ Usually triggered by a user click

useNavigate()
→ Navigation through JavaScript
→ Used inside event handlers or logic
```

Examples:

```jsx
<Link to="/about">
  About
</Link>
```

```jsx
const navigate =
  useNavigate();

navigate("/about");
```

---

## 22. `useParams()`

`useParams()` retrieves Route Parameter values from the current URL.

```jsx
import {
  useParams,
} from "react-router-dom";

function UserDetail() {
  const {
    userId,
  } = useParams();

  return (
    <div
      style={{
        border:
          "2px solid blue",
        padding: "20px",
        marginTop: "20px",
      }}
    >
      <h3>
        User Details
      </h3>

      <p>
        User ID from URL:
        {userId}
      </p>

      <p>
        Display detailed
        information for user
        {userId}.
      </p>
    </div>
  );
}
```

For:

```text
/user/101
```

the extracted value is:

```javascript
userId === "101"
```

---

## 23. Dynamic Routing Flow

The overall process is:

```text
User List
    ↓
Button Click
    ↓
useNavigate()
    ↓
/user/101
    ↓
Route
/user/:userId
    ↓
UserDetail
    ↓
useParams()
    ↓
userId = 101
```

The extracted ID can then be used to retrieve detailed data.

---

## 24. React Router Practice Project

The final practice combines React Router and JSON Server.

Install:

```bash
npm install json-server
```

```bash
npm install react-router-dom
```

Required routes:

```text
/
→ Home

/about
→ About

/products
→ Product List

/products/:id
→ Product Detail

*
→ Not Found
   or redirect to Home
```

---

## 25. Practice Project Structure

```text
my-react-app2/
└── src/
    ├── App.js
    ├── index.js
    │
    ├── pages/
    │   ├── Home.js
    │   ├── About.js
    │   ├── Products.js
    │   └── ProductDetail.js
    │
    └── components/
        └── Nav.js
```

### `App.js`

Defines the application routes.

### `index.js`

Acts as the application entry point.

### `Home.js`

Home page component.

### `About.js`

Company information page.

### `Products.js`

Displays the complete product list.

### `ProductDetail.js`

Displays detailed information about a selected product.

### `Nav.js`

Provides navigation between pages.

---

## 26. Final Routing Structure

A possible route configuration for the practice project is:

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="/products"
    element={<Products />}
  />

  <Route
    path="/products/:id"
    element={
      <ProductDetail />
    }
  />

  <Route
    path="*"
    element={<NotFound />}
  />
</Routes>
```

Basic routing flow:

```text
BrowserRouter
      ↓
Routes
      ↓
Route
      ↓
URL Matching
      ↓
Page Component
```

Dynamic detail page flow:

```text
/products/:id
      ↓
useParams()
      ↓
Product ID
      ↓
Product Detail
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# React Router

## 1. React Router란?

React Router는 React SPA에서 **URL에 따라 서로 다른 컴포넌트를 렌더링**할 수 있도록 하는 라이브러리입니다.

SPA에서는 URL이 변경되어도 전체 페이지를 새로고침하지 않고 필요한 컴포넌트만 변경할 수 있습니다.

```text
URL 변경
    ↓
React Router
    ↓
Route 확인
    ↓
컴포넌트 렌더링
```

예:

```text
/
→ Home

/about
→ About

/products
→ Products
```

---

## 2. Routing

**Routing**은 요청된 URL 경로에 맞는 콘텐츠를 선택하는 과정입니다.

React에서는 다음과 같이 이해할 수 있습니다.

```text
URL
  ↓
Route Matching
  ↓
Component
```

예:

```text
/
→ Home Component

/about
→ About Component

/user/101
→ User Detail Component
```

---

## 3. React Router 주요 컴포넌트

이 Branch에서 다루는 주요 구성 요소:

- `BrowserRouter`
- `Routes`
- `Route`
- `Link`
- `Navigate`

### `BrowserRouter`

브라우저의 History API를 이용하여 URL과 Routing을 관리합니다.

### `Routes`

여러 `Route`를 포함하고 현재 URL에 맞는 Route를 찾습니다.

### `Route`

특정 URL에서 어떤 컴포넌트를 렌더링할지 정의합니다.

### `Link`

Route 사이의 페이지 이동을 담당합니다.

### `Navigate`

특정 URL로 사용자를 Redirect합니다.

---

## 4. React Router 설치

React 프로젝트에서 `react-router-dom`을 설치합니다.

```bash
npm install react-router-dom
```

---

## 5. 기본 프로젝트 구조

간단한 Router 프로젝트는 다음과 같이 구성할 수 있습니다.

```text
my-react-app/
└── src/
    ├── App.js
    ├── Nav.js
    └── pages/
        ├── Home.js
        ├── About.js
        └── NotFound.js
```

페이지별 UI를 각각의 컴포넌트로 분리합니다.

---

## 6. `BrowserRouter`

Routing 기능을 사용하기 위해 애플리케이션을 `BrowserRouter`로 감쌉니다.

```jsx
import ReactDOM from "react-dom/client";

import {
  BrowserRouter,
} from "react-router-dom";

import App from "./App";

const root =
  ReactDOM.createRoot(
    document.getElementById(
      "root"
    )
  );

root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

전체 구조:

```text
BrowserRouter
    ↓
App
    ↓
Routes
    ↓
Route
```

브라우저의 뒤로 가기와 앞으로 가기 기능도 사용할 수 있습니다.

---

## 7. `Routes`

`Routes` 내부에 여러 Route를 정의합니다.

```jsx
import {
  Routes,
  Route,
} from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route
        path="/"
        element={<Home />}
      />

      <Route
        path="/about"
        element={<About />}
      />
    </Routes>
  );
}
```

URL이 변경되면 Route들을 확인하여 현재 URL에 맞는 컴포넌트를 렌더링합니다.

---

## 8. `Route`

`Route`에서는 주로 다음 두 Props를 사용합니다.

```text
path
→ URL 경로

element
→ 렌더링할 컴포넌트
```

예:

```jsx
<Route
  path="/"
  element={<Home />}
/>
```

의미:

```text
URL: /
     ↓
<Home />
```

또 다른 예:

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

```text
URL: /about
     ↓
<About />
```

---

## 9. Root Path와 Sub Path

최상위 경로는 `/`로 표현합니다.

```jsx
<Route
  path="/"
  element={<Home />}
/>
```

예:

```text
https://www.example.com/
```

하위 경로:

```jsx
<Route
  path="/about"
  element={<About />}
/>
```

예:

```text
https://www.example.com/about
```

---

## 10. `Link`

`Link`는 Route 사이를 이동할 때 사용합니다.

```jsx
import {
  Link,
} from "react-router-dom";

function Nav() {
  return (
    <div>
      <Link to="/">
        Home
      </Link>

      <Link to="/about">
        About
      </Link>
    </div>
  );
}
```

이동할 경로는 `to`에 지정합니다.

```text
<Link to="/about">
        ↓
URL → /about
        ↓
Routes 확인
        ↓
<About /> 렌더링
```

---

## 11. `Link`와 `Route` 동작 과정

사용자가 다음 Link를 클릭한다고 가정합니다.

```jsx
<Link to="/about">
  About
</Link>
```

동작 과정:

```text
사용자 클릭
    ↓
Link
    ↓
URL → /about
    ↓
Routes
    ↓
Route path="/about"
    ↓
<About />
```

SPA 내부에서 URL에 따라 화면 콘텐츠가 변경됩니다.

---

## 12. 유효하지 않은 URL 처리

정의되지 않은 URL은 `*` 경로를 이용하여 처리할 수 있습니다.

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="*"
    element={<NotFound />}
  />
</Routes>
```

예:

```text
/unknown
    ↓
일치하는 Route 없음
    ↓
*
    ↓
NotFound
```

---

## 13. 404 페이지

잘못된 URL에 대해 `NotFound` 컴포넌트를 표시할 수 있습니다.

```jsx
function NotFound() {
  return (
    <div>
      <h1>404</h1>

      <p>
        페이지를 찾을 수 없습니다.
      </p>
    </div>
  );
}
```

Route:

```jsx
<Route
  path="*"
  element={<NotFound />}
/>
```

---

## 14. 잘못된 URL Redirect

404 페이지 대신 잘못된 URL을 특정 페이지로 이동시킬 수도 있습니다.

```jsx
import {
  Navigate,
} from "react-router-dom";

<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="*"
    element={
      <Navigate
        to="/"
        replace
      />
    }
  />
</Routes>
```

잘못된 URL을 Home으로 이동시킵니다.

---

## 15. `Navigate`

`Navigate`는 렌더링되는 순간 지정한 Route로 사용자를 이동시킵니다.

기본 구조:

```jsx
<Navigate
  to="/login"
  replace
/>
```

주요 Props:

| Prop | 역할  |
| --- | --- |
| `to` | 이동할 URL 지정 |
| `replace` | 현재 History 항목을 새로운 Route로 대체 |

`replace`를 사용하면 현재 URL을 Browser History에서 새로운 URL로 대체합니다.

---

## 16. 조건부 Redirect

사용자의 상태에 따라 다른 페이지로 이동시킬 수 있습니다.

```jsx
import {
  Navigate,
} from "react-router-dom";

function UserProfile({
  isLoggedIn,
}) {
  if (!isLoggedIn) {
    return (
      <Navigate
        to="/login"
        replace
      />
    );
  }

  return (
    <div>
      <h1>
        사용자 프로필
      </h1>

      <p>
        환영합니다!
      </p>
    </div>
  );
}
```

동작:

```text
isLoggedIn?
   │
   ├── true
   │   → Profile
   │
   └── false
       → /login
```

---

## 17. 권한에 따른 Redirect

사용자 Role에 따라 접근을 제한할 수도 있습니다.

```jsx
function AdminPage({
  userRole,
}) {
  if (
    userRole !== "admin"
  ) {
    return (
      <Navigate
        to="/"
        replace
      />
    );
  }

  return (
    <div>
      <h1>
        관리자 대시보드
      </h1>

      <p>
        관리자만 볼 수 있는
        정보입니다.
      </p>
    </div>
  );
}
```

관리자가 아닌 경우 메인 페이지로 이동합니다.

---

## 18. Route Parameter

**Route Parameter**는 URL의 특정 위치를 변수처럼 사용하는 기능입니다.

같은 페이지 구조에서 서로 다른 ID를 가진 데이터를 보여줄 때 사용할 수 있습니다.

대표적인 활용:

- 게시글
- 상품
- 사용자 프로필

Route Parameter는 `:`을 사용합니다.

```jsx
<Route
  path="/posts/:postId"
  element={<PostDetail />}
/>
```

가능한 URL:

```text
/posts/1
/posts/2
/posts/100
```

하나의 Route 정의로 여러 Detail Page를 처리할 수 있습니다.

---

## 19. 사용자 상세 Route

예:

```jsx
<Routes>
  <Route
    path="/"
    element={<UserList />}
  />

  <Route
    path="/user/:userId"
    element={<UserDetail />}
  />
</Routes>
```

가능한 URL:

```text
/user/101
/user/202
```

여기서:

```text
:userId
```

가 동적인 Route Parameter입니다.

---

## 20. `useNavigate()`

`useNavigate()`는 JavaScript 코드에서 페이지를 이동할 수 있는 함수를 제공합니다.

```jsx
import {
  useNavigate,
} from "react-router-dom";

function UserList() {
  const navigate =
    useNavigate();

  const users = [
    {
      id: 101,
      name: "김리액트",
    },
    {
      id: 202,
      name: "박라우터",
    },
  ];

  const goToUserDetail =
    id => {
      navigate(
        `/user/${id}`
      );
    };

  return (
    <div>
      <h2>
        사용자 목록
      </h2>

      {users.map(
        user => (
          <button
            key={user.id}
            onClick={() =>
              goToUserDetail(
                user.id
              )
            }
          >
            {user.name}
            {" "}
            ({user.id})
            상세 보기
          </button>
        )
      )}
    </div>
  );
}
```

Template Literal을 이용하여 동적인 URL을 생성합니다.

```javascript
navigate(
  `/user/${id}`
);
```

---

## 21. `Link`와 `useNavigate()`

두 기능 모두 Route 이동에 사용할 수 있습니다.

```text
Link
→ JSX에서 이동 링크 표현
→ 사용자가 Link를 클릭

useNavigate()
→ JavaScript 코드에서 이동
→ Event Handler 또는 조건 로직에서 사용
```

예:

```jsx
<Link to="/about">
  About
</Link>
```

```jsx
const navigate =
  useNavigate();

navigate("/about");
```

---

## 22. `useParams()`

`useParams()`는 현재 URL의 Route Parameter 값을 가져오는 Hook입니다.

```jsx
import {
  useParams,
} from "react-router-dom";

function UserDetail() {
  const {
    userId,
  } = useParams();

  return (
    <div
      style={{
        border:
          "2px solid blue",
        padding: "20px",
        marginTop: "20px",
      }}
    >
      <h3>
        사용자 상세 정보
      </h3>

      <p>
        URL에서 추출한 사용자 ID:
        {userId}
      </p>

      <p>
        사용자 ID
        {userId}의
        상세 정보를 표시합니다.
      </p>
    </div>
  );
}
```

URL이:

```text
/user/101
```

이라면:

```javascript
userId === "101"
```

이 됩니다.

---

## 23. 동적 Routing 전체 흐름

```text
사용자 목록
    ↓
버튼 클릭
    ↓
useNavigate()
    ↓
/user/101
    ↓
Route
/user/:userId
    ↓
UserDetail
    ↓
useParams()
    ↓
userId = 101
```

추출한 ID를 이용하여 서버에서 해당 데이터의 상세 정보를 가져올 수 있습니다.

---

## 24. React Router 종합 실습

마지막 실습에서는 React Router와 JSON Server를 함께 사용합니다.

설치:

```bash
npm install json-server
```

```bash
npm install react-router-dom
```

구현할 Route:

```text
/
→ Home

/about
→ 회사 소개

/products
→ 상품 목록

/products/:id
→ 상품 상세

*
→ 페이지 없음
   또는 Home으로 Redirect
```

---

## 25. 실습 프로젝트 구조

```text
my-react-app2/
└── src/
    ├── App.js
    ├── index.js
    │
    ├── pages/
    │   ├── Home.js
    │   ├── About.js
    │   ├── Products.js
    │   └── ProductDetail.js
    │
    └── components/
        └── Nav.js
```

### `App.js`

Route를 설정하는 메인 컴포넌트입니다.

### `index.js`

React 애플리케이션의 시작점입니다.

### `Home.js`

홈 페이지입니다.

### `About.js`

회사 소개 페이지입니다.

### `Products.js`

전체 상품 목록을 보여줍니다.

### `ProductDetail.js`

특정 상품의 상세 정보를 보여줍니다.

### `Nav.js`

상단 페이지 이동 메뉴를 구성합니다.

---

## 26. 최종 Routing 구조

실습 조건을 Route로 표현하면 다음과 같이 구성할 수 있습니다.

```jsx
<Routes>
  <Route
    path="/"
    element={<Home />}
  />

  <Route
    path="/about"
    element={<About />}
  />

  <Route
    path="/products"
    element={<Products />}
  />

  <Route
    path="/products/:id"
    element={
      <ProductDetail />
    }
  />

  <Route
    path="*"
    element={<NotFound />}
  />
</Routes>
```

기본 Routing 흐름:

```text
BrowserRouter
      ↓
Routes
      ↓
Route
      ↓
URL Matching
      ↓
Page Component
```

동적 Detail Page는 다음과 같이 연결됩니다.

```text
/products/:id
      ↓
useParams()
      ↓
Product ID
      ↓
Product Detail
```

</details>
