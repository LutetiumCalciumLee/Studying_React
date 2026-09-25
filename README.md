<details>
<summary>ENG (English Version)</summary>

# REST API / JSON Server / CRUD

## 1. What is REST?

**REST (Representational State Transfer)** is an architectural approach for representing resources on the web and transferring their state.

REST is based on HTTP.

The main concepts are:

- **Resource**: Data stored on the server, such as users, posts, products, or books
- **Representation**: A format used to represent resources, such as JSON or XML
- **State Transfer**: Exchanging the state of a resource through HTTP methods such as GET and POST

REST identifies a resource through a URL and defines the operation performed on that resource through an HTTP method.

Example:

```text
/users
/books
/movies
```

---

## 2. REST Principles

The material introduces six REST principles.

| Principle | Description |
| --- | --- |
| Client-Server | The client handles UI while the server manages data |
| Stateless | The server does not store request state between requests |
| Cacheable | Responses can be cached by the client |
| Layered System | The client does not need to know the internal server structure |
| Uniform Interface | Resources are accessed through a consistent interface |
| Code on Demand | The server may optionally provide executable code to the client |

### Client-Server

```text
Client
→ User Interface

Server
→ Data Management
```

### Stateless

Each request should contain the information required to process that request.

### Uniform Interface

Resources are accessed consistently.

```text
/users
/products
/books
```

---

## 3. REST API

A REST API is a web API designed according to REST principles.

The HTTP method indicates what operation should be performed on the resource.

| CRUD | HTTP Method | Purpose |
| --- | --- | --- |
| Read | `GET` | Retrieve data |
| Create | `POST` | Create data |
| Update | `PUT` / `PATCH` | Modify data |
| Delete | `DELETE` | Remove data |

Example:

```text
GET /users
```

Retrieve all users.

```text
GET /users/1
```

Retrieve the user whose ID is `1`.

```text
POST /users
```

Create a new user.

```text
PUT /users/1
PATCH /users/1
```

Modify the user whose ID is `1`.

```text
DELETE /users/1
```

Delete the user whose ID is `1`.

---

## 4. RESTful API Design Example

A movie resource can be designed as follows.

| Operation | URL | Method |
| --- | --- | --- |
| Get all movies | `/movies` | `GET` |
| Get one movie | `/movies/1` | `GET` |
| Create movie | `/movies` | `POST` |
| Update movie | `/movies/1` | `PUT` |
| Delete movie | `/movies/1` | `DELETE` |

The resource is expressed by the URL while the HTTP method represents the action.

---

## 5. Calling a REST API in React

A REST API can be called after a component is mounted.

```jsx
const [
  users,
  setUsers,
] = useState([]);

useEffect(() => {
  fetch(
    "http://localhost:3001/users"
  )
    .then(
      res => res.json()
    )
    .then(
      data =>
        setUsers(data)
    );
}, []);
```

The basic process is:

```text
Component Mount
      ↓
useEffect()
      ↓
fetch()
      ↓
Server Response
      ↓
response.json()
      ↓
setState()
      ↓
Re-render
```

---

## 6. What is JSON Server?

**JSON Server** is a tool that creates a mock REST API server from a local JSON file.

It can be used without implementing a separate backend server.

Main features:

- Easy installation
- Fast local server setup
- Uses a `db.json` file
- Supports REST-style requests
- Supports GET, POST, PUT, DELETE
- Can be accessed through `fetch()` or Axios

It is useful for frontend development and REST API practice.

---

## 7. Installing JSON Server

The material introduces both global and local installation.

### Global Installation

```bash
npm install -g json-server
```

Run:

```bash
json-server --watch db.json
```

### Local Installation

```bash
npm install json-server
```

Run through `npx`:

```bash
npx json-server --watch db.json
```

The course material recommends local installation when using `npx`.

---

## 8. Port Configuration

The React development server and JSON Server need to run separately.

The material uses:

```text
React
→ Port 3000

JSON Server
→ Port 3001
```

Run JSON Server on port `3001`:

```bash
npx json-server --watch db.json --port 3001
```

Run React in another terminal:

```bash
npm start
```

Therefore, two terminals are used.

```text
Terminal 1
→ React
→ localhost:3000

Terminal 2
→ JSON Server
→ localhost:3001
```

---

## 9. `db.json` Location

The `db.json` file can be located in the project root.

```text
my-react-app/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   └── App.js
├── package.json
└── db.json
```

Run:

```bash
npx json-server --watch db.json
```

If the file is inside another directory:

```text
my-react-app/
├── src/
├── data/
│   └── db.json
└── package.json
```

Run:

```bash
npx json-server --watch ./data/db.json
```

---

## 10. JSON Server Setup Process

The overall setup process is:

```text
1. Install JSON Server
       ↓
2. Create db.json
       ↓
3. Run JSON Server
       ↓
4. Run React
       ↓
5. Use both servers together
```

Commands:

```bash
npm install json-server
```

```bash
npx json-server --watch db.json --port 3001
```

```bash
npm start
```

---

## 11. Basic `db.json` Structure

Example:

```json
{
  "posts": [
    {
      "id": 1,
      "title": "Getting Started with JSON Server",
      "author": "React"
    },
    {
      "id": 2,
      "title": "Backend Mock for Frontend Development",
      "author": "Developer"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "Very useful!",
      "postId": 1
    },
    {
      "id": 2,
      "body": "Useful for React projects.",
      "postId": 2
    }
  ],
  "profile": {
    "name": "React Agent"
  }
}
```

JSON Server converts top-level keys into API endpoints.

```text
posts
→ /posts

comments
→ /comments

profile
→ /profile
```

---

## 12. JSON Server Endpoints

If JSON Server is running at:

```text
http://localhost:3001
```

the following endpoints can be used:

```text
http://localhost:3001/posts
http://localhost:3001/comments
http://localhost:3001/profile
http://localhost:3001/posts/1
```

For an array resource such as `posts`, CRUD operations can be performed.

```text
GET /posts
GET /posts/1
POST /posts
PUT /posts/1
PATCH /posts/1
DELETE /posts/1
```

---

## 13. `fetch()`

`fetch()` is a browser function used to send network requests and receive responses.

Basic syntax:

```javascript
fetch(url, options)
  .then(response => {
    // Handle response
  })
  .catch(error => {
    // Handle error
  });
```

The `Response` object represents the network response itself.

To retrieve actual JSON data:

```javascript
response.json()
```

Example:

```javascript
fetch(
  "http://localhost:3001/books"
)
  .then(
    response =>
      response.json()
  )
  .then(
    data =>
      console.log(data)
  )
  .catch(
    error =>
      console.error(error)
  );
```

---

## 14. Book Data

The CRUD examples use a `books` resource.

```json
{
  "books": [
    {
      "id": "1",
      "title": "The Old Man and the Sea",
      "author": "Ernest Hemingway",
      "year": 1952
    },
    {
      "id": "2",
      "title": "The Great Gatsby",
      "author": "F. Scott Fitzgerald",
      "year": 1925
    },
    {
      "id": "3",
      "title": "Crime and Punishment",
      "author": "Fyodor Dostoevsky",
      "year": 1866
    }
  ]
}
```

The endpoint becomes:

```text
http://localhost:3001/books
```

---

## 15. GET - Read

`GET` retrieves data from the server.

```jsx
function BookList() {
  const [
    books,
    setBooks,
  ] = useState([]);

  useEffect(() => {
    fetch(
      "http://localhost:3001/books"
    )
      .then(
        res => res.json()
      )
      .then(data => {
        setBooks(data);
      })
      .catch(err =>
        console.error(
          "Failed to load data:",
          err
        )
      );
  }, []);

  return (
    <div>
      <h2>Book List</h2>

      <ul>
        {books.map(book => (
          <li key={book.id}>
            <strong>
              {book.title}
            </strong>

            {" "}
            ({book.year})
            {" - "}
            {book.author}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

The process is:

```text
GET Request
    ↓
JSON Server
    ↓
JSON Response
    ↓
setBooks()
    ↓
map()
    ↓
Render List
```

---

## 16. POST - Create

`POST` creates new data.

```jsx
function AddBook() {
  const [
    title,
    setTitle,
  ] = useState("");

  const [
    author,
    setAuthor,
  ] = useState("");

  const [
    year,
    setYear,
  ] = useState("");

  const handleAddBook =
    () => {
      const newBook = {
        title,
        author,
        year:
          Number(year),
      };

      fetch(
        "http://localhost:3001/books",
        {
          method: "POST",

          headers: {
            "Content-Type":
              "application/json",
          },

          body:
            JSON.stringify(
              newBook
            ),
        }
      )
        .then(
          res =>
            res.json()
        )
        .then(data => {
          alert(
            `New book added: ${data.title}`
          );

          console.log(
            "Created data:",
            data
          );
        })
        .catch(err =>
          console.error(
            "Create failed:",
            err
          )
        );
    };

  return (
    <div>
      <h2>
        Add New Book
      </h2>

      <input
        type="text"
        placeholder="Title"
        value={title}
        onChange={e =>
          setTitle(
            e.target.value
          )
        }
      />

      <input
        type="text"
        placeholder="Author"
        value={author}
        onChange={e =>
          setAuthor(
            e.target.value
          )
        }
      />

      <input
        type="number"
        placeholder="Year"
        value={year}
        onChange={e =>
          setYear(
            e.target.value
          )
        }
      />

      <button
        onClick={
          handleAddBook
        }
      >
        Add
      </button>
    </div>
  );
}
```

---

## 17. POST Request Body

Example HTTP request:

```text
POST /books
Content-Type: application/json
```

```json
{
  "title": "Demian",
  "author": "Hermann Hesse",
  "year": 1919
}
```

Example response:

```json
{
  "title": "Demian",
  "author": "Hermann Hesse",
  "year": 1919,
  "id": "a12b"
}
```

The JavaScript object is converted to JSON text with:

```javascript
JSON.stringify(
  newBook
)
```

---

## 18. PUT - Update

`PUT` is used to replace an existing resource.

Example:

```javascript
fetch(
  "http://localhost:3001/books/1",
  {
    method: "PUT",

    headers: {
      "Content-Type":
        "application/json",
    },

    body: JSON.stringify({
      id: "1",
      title:
        "The Old Man and the Sea (Revised)",
      author:
        "Ernest Hemingway",
      year: 1953,
    }),
  }
)
  .then(
    res => res.json()
  )
  .then(data =>
    console.log(
      "PUT result:",
      data
    )
  )
  .catch(err =>
    console.error(
      "PUT failed:",
      err
    )
  );
```

The material describes `PUT` as replacing the complete existing data.

```text
PUT
→ Full Update
```

---

## 19. PATCH - Partial Update

`PATCH` modifies selected fields.

```javascript
fetch(
  "http://localhost:3001/books/1",
  {
    method: "PATCH",

    headers: {
      "Content-Type":
        "application/json",
    },

    body: JSON.stringify({
      year: 1995,
    }),
  }
)
  .then(
    res => res.json()
  )
  .then(data =>
    console.log(
      "PATCH result:",
      data
    )
  )
  .catch(err =>
    console.error(
      "PATCH failed:",
      err
    )
  );
```

The material distinguishes the two methods as:

```text
PUT
→ Full Update

PATCH
→ Partial Update
```

---

## 20. DELETE - Delete

`DELETE` removes a resource.

```jsx
function DeleteBook() {
  const handleDelete =
    id => {
      fetch(
        `http://localhost:3001/books/${id}`,
        {
          method:
            "DELETE",
        }
      )
        .then(res => {
          if (!res.ok) {
            throw new Error(
              "Delete failed"
            );
          }

          alert(
            `Book id=${id} deleted.`
          );
        })
        .catch(err =>
          console.error(err)
        );
    };

  return (
    <div>
      <h2>
        Delete Book Test
      </h2>

      <button
        onClick={() =>
          handleDelete(3)
        }
      >
        Delete Book 3
      </button>
    </div>
  );
}
```

The endpoint is dynamically generated with the selected ID.

```javascript
`http://localhost:3001/books/${id}`
```

---

## 21. CRUD Summary

```text
Create
POST

Read
GET

Update
PUT / PATCH

Delete
DELETE
```

In a React application:

```text
User Interaction
      ↓
Event Handler
      ↓
fetch()
      ↓
HTTP Request
      ↓
JSON Server
      ↓
JSON Response
      ↓
State Update
      ↓
UI Re-render
```

---

## 22. Book Manager Practice

The branch includes an integrated Book Manager using JSON Server.

The example interface contains:

- Book title input
- Author input
- Publication year input
- Book registration
- Book list
- Refresh
- Edit button
- Delete button

It combines the individual REST requests into one CRUD application.

```text
Book Manager

Create
→ POST

Read
→ GET

Update
→ PUT / PATCH

Delete
→ DELETE
```

---

## 23. Todo Mini Project

The final practice uses the API provided by JSON Server to build a Todo management application.

Example `db.json`:

```json
{
  "todos": [
    {
      "id": "1",
      "text": "Review React Components",
      "date": "2025. 10. 27",
      "completed": false
    },
    {
      "id": "2",
      "text": "Study for Machine Learning Exam",
      "date": "2025. 10. 27",
      "completed": false
    }
  ]
}
```

API endpoint:

```text
http://localhost:3001/todos
```

The example interface includes:

- Current date
- Todo input
- Add button
- Todo list
- Search input
- Completion state
- Delete button

The project applies REST API and CRUD operations to actual schedule data.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# REST API / JSON Server / CRUD

## 1. REST란?

**REST(Representational State Transfer)** 는 웹에서 자원을 표현하고 그 상태를 전송하는 방식을 의미합니다.

HTTP를 기반으로 구현합니다.

주요 개념:

- **Resource(자원)**: 서버에 존재하는 사용자, 게시글, 상품, 도서 등의 데이터
- **Representation(표현)**: JSON, XML 등 자원을 표현하는 형식
- **State Transfer(상태 전송)**: GET, POST 등의 HTTP Method를 이용하여 자원의 상태를 주고받는 것

REST에서는 URL로 자원을 나타내고 HTTP Method를 이용하여 자원에 대한 동작을 정의합니다.

예:

```text
/users
/books
/movies
```

---

## 2. REST의 6원칙

자료에서는 REST의 특징을 여섯 가지 원칙으로 설명합니다.

| 원칙  | 설명  |
| --- | --- |
| Client-Server | 클라이언트는 UI, 서버는 데이터 관리 |
| Stateless | 서버가 요청 간 상태를 저장하지 않음 |
| Cacheable | 클라이언트가 응답 결과를 캐시할 수 있음 |
| Layered System | 클라이언트가 서버 내부 구조를 알 필요가 없음 |
| Uniform Interface | 동일한 방식으로 자원에 접근 |
| Code on Demand | 서버가 필요에 따라 코드를 클라이언트에 전달 가능 |

### Client-Server

```text
Client
→ UI 담당

Server
→ 데이터 관리 담당
```

### Stateless

각 요청에 필요한 모든 정보가 해당 요청에 포함됩니다.

### Uniform Interface

자원을 일관된 형태로 표현합니다.

```text
/users
/products
/books
```

---

## 3. REST API

REST API는 REST 원칙을 기반으로 설계된 Web API입니다.

HTTP Method에 따라 자원에 수행할 동작을 정의합니다.

| CRUD | HTTP Method | 역할  |
| --- | --- | --- |
| Read | `GET` | 데이터 조회 |
| Create | `POST` | 데이터 생성 |
| Update | `PUT` / `PATCH` | 데이터 수정 |
| Delete | `DELETE` | 데이터 삭제 |

예:

```text
GET /users
```

전체 사용자 조회

```text
GET /users/1
```

ID가 `1`인 사용자 조회

```text
POST /users
```

새 사용자 생성

```text
PUT /users/1
PATCH /users/1
```

ID가 `1`인 사용자 수정

```text
DELETE /users/1
```

ID가 `1`인 사용자 삭제

---

## 4. RESTful API 설계 예

영화 데이터를 자원으로 사용하면 다음과 같이 구성할 수 있습니다.

| 동작  | URL | Method |
| --- | --- | --- |
| 전체 영화 조회 | `/movies` | `GET` |
| 특정 영화 조회 | `/movies/1` | `GET` |
| 새 영화 등록 | `/movies` | `POST` |
| 영화 정보 수정 | `/movies/1` | `PUT` |
| 영화 삭제 | `/movies/1` | `DELETE` |

URL은 자원을 표현하고 HTTP Method는 해당 자원에 대한 동작을 표현합니다.

---

## 5. React에서 REST API 호출

컴포넌트가 처음 Mount될 때 API를 호출할 수 있습니다.

```jsx
const [
  users,
  setUsers,
] = useState([]);

useEffect(() => {
  fetch(
    "http://localhost:3001/users"
  )
    .then(
      res => res.json()
    )
    .then(
      data =>
        setUsers(data)
    );
}, []);
```

동작 흐름:

```text
컴포넌트 Mount
      ↓
useEffect()
      ↓
fetch()
      ↓
서버 응답
      ↓
response.json()
      ↓
setState()
      ↓
재렌더링
```

---

## 6. JSON Server란?

**JSON Server** 는 로컬의 JSON 파일을 기반으로 가상의 REST API 서버를 쉽게 만들 수 있도록 하는 도구입니다.

별도의 Backend를 구현하지 않고 API 요청과 응답을 테스트할 수 있습니다.

주요 특징:

- 간단한 설치
- 빠른 실행
- `db.json` 사용
- REST 형태 API 제공
- GET, POST, PUT, DELETE 지원
- `fetch()` 또는 Axios로 요청 가능

Frontend 개발 및 REST API 실습에 사용할 수 있습니다.

---

## 7. JSON Server 설치

자료에서는 전역 설치와 로컬 설치를 모두 다룹니다.

### 전역 설치

```bash
npm install -g json-server
```

실행:

```bash
json-server --watch db.json
```

### 프로젝트 로컬 설치

```bash
npm install json-server
```

`npx`를 이용하여 실행합니다.

```bash
npx json-server --watch db.json
```

수업 자료에서는 `npx`를 사용할 경우 전역 설치 대신 로컬 설치를 권장합니다.

---

## 8. Port 설정

React 개발 서버와 JSON Server를 서로 다른 Port에서 실행합니다.

자료의 구성:

```text
React
→ Port 3000

JSON Server
→ Port 3001
```

JSON Server:

```bash
npx json-server --watch db.json --port 3001
```

다른 Terminal에서 React 실행:

```bash
npm start
```

즉 두 개의 Terminal을 사용합니다.

```text
Terminal 1
→ React
→ localhost:3000

Terminal 2
→ JSON Server
→ localhost:3001
```

---

## 9. `db.json` 위치

`db.json`을 프로젝트 Root에 둘 수 있습니다.

```text
my-react-app/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   └── App.js
├── package.json
└── db.json
```

실행:

```bash
npx json-server --watch db.json
```

다른 폴더에 저장하는 경우:

```text
my-react-app/
├── src/
├── data/
│   └── db.json
└── package.json
```

실행:

```bash
npx json-server --watch ./data/db.json
```

---

## 10. JSON Server 사용 과정

전체 과정:

```text
1. JSON Server 설치
       ↓
2. db.json 생성
       ↓
3. JSON Server 실행
       ↓
4. React 실행
       ↓
5. 두 서버를 함께 사용
```

명령어:

```bash
npm install json-server
```

```bash
npx json-server --watch db.json --port 3001
```

```bash
npm start
```

---

## 11. `db.json` 기본 구조

예:

```json
{
  "posts": [
    {
      "id": 1,
      "title": "json-server 시작",
      "author": "김리액트"
    },
    {
      "id": 2,
      "title": "프론트엔드 개발자를 위한 백엔드 목업",
      "author": "박개발"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "정말 유용하네요!",
      "postId": 1
    },
    {
      "id": 2,
      "body": "리액트 프로젝트에 딱입니다.",
      "postId": 2
    }
  ],
  "profile": {
    "name": "리액트 에이전트"
  }
}
```

JSON Server는 최상위 Key를 API Endpoint로 변환합니다.

```text
posts
→ /posts

comments
→ /comments

profile
→ /profile
```

---

## 12. JSON Server Endpoint

JSON Server가 다음 주소에서 실행된다고 가정합니다.

```text
http://localhost:3001
```

접근 가능한 Endpoint:

```text
http://localhost:3001/posts
http://localhost:3001/comments
http://localhost:3001/profile
http://localhost:3001/posts/1
```

`posts`와 같은 배열 데이터에는 CRUD 기능을 사용할 수 있습니다.

```text
GET /posts
GET /posts/1
POST /posts
PUT /posts/1
PATCH /posts/1
DELETE /posts/1
```

---

## 13. `fetch()`

`fetch()`는 브라우저에 내장된 네트워크 요청 함수입니다.

기본 구조:

```javascript
fetch(url, options)
  .then(response => {
    // 서버 응답 처리
  })
  .catch(error => {
    // 오류 처리
  });
```

`Response` 객체에서 실제 JSON 데이터를 가져오기 위해 다음 메소드를 사용합니다.

```javascript
response.json()
```

예:

```javascript
fetch(
  "http://localhost:3001/books"
)
  .then(
    response =>
      response.json()
  )
  .then(
    data =>
      console.log(data)
  )
  .catch(
    error =>
      console.error(error)
  );
```

---

## 14. 도서 데이터

CRUD 실습에서는 `books` 자원을 사용합니다.

```json
{
  "books": [
    {
      "id": "1",
      "title": "노인과 바다",
      "author": "어니스트 헤밍웨이",
      "year": 1952
    },
    {
      "id": "2",
      "title": "위대한 개츠비",
      "author": "F. 스콧 피츠제럴드",
      "year": 1925
    },
    {
      "id": "3",
      "title": "죄와 벌",
      "author": "표도르 도스토옙스키",
      "year": 1866
    }
  ]
}
```

Endpoint:

```text
http://localhost:3001/books
```

---

## 15. GET - Read

`GET`은 서버의 데이터를 조회할 때 사용합니다.

```jsx
function BookList() {
  const [
    books,
    setBooks,
  ] = useState([]);

  useEffect(() => {
    fetch(
      "http://localhost:3001/books"
    )
      .then(
        res => res.json()
      )
      .then(data => {
        setBooks(data);
      })
      .catch(err =>
        console.error(
          "데이터 불러오기 실패:",
          err
        )
      );
  }, []);

  return (
    <div>
      <h2>도서 목록</h2>

      <ul>
        {books.map(book => (
          <li key={book.id}>
            <strong>
              {book.title}
            </strong>

            {" "}
            ({book.year})
            {" - "}
            {book.author}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

동작 흐름:

```text
GET 요청
    ↓
JSON Server
    ↓
JSON 응답
    ↓
setBooks()
    ↓
map()
    ↓
목록 렌더링
```

---

## 16. POST - Create

`POST`는 새로운 데이터를 생성할 때 사용합니다.

```jsx
function AddBook() {
  const [
    title,
    setTitle,
  ] = useState("");

  const [
    author,
    setAuthor,
  ] = useState("");

  const [
    year,
    setYear,
  ] = useState("");

  const handleAddBook =
    () => {
      const newBook = {
        title,
        author,
        year:
          Number(year),
      };

      fetch(
        "http://localhost:3001/books",
        {
          method: "POST",

          headers: {
            "Content-Type":
              "application/json",
          },

          body:
            JSON.stringify(
              newBook
            ),
        }
      )
        .then(
          res =>
            res.json()
        )
        .then(data => {
          alert(
            `새 책이 추가되었습니다: ${data.title}`
          );

          console.log(
            "등록된 데이터:",
            data
          );
        })
        .catch(err =>
          console.error(
            "등록 실패:",
            err
          )
        );
    };

  return (
    <div>
      <h2>새 책 등록</h2>

      <input
        type="text"
        placeholder="책 제목"
        value={title}
        onChange={e =>
          setTitle(
            e.target.value
          )
        }
      />

      <input
        type="text"
        placeholder="저자"
        value={author}
        onChange={e =>
          setAuthor(
            e.target.value
          )
        }
      />

      <input
        type="number"
        placeholder="출판년도"
        value={year}
        onChange={e =>
          setYear(
            e.target.value
          )
        }
      />

      <button
        onClick={
          handleAddBook
        }
      >
        등록하기
      </button>
    </div>
  );
}
```

---

## 17. POST Request Body

HTTP 요청 예:

```text
POST /books
Content-Type: application/json
```

```json
{
  "title": "데미안",
  "author": "헤르만 헤세",
  "year": 1919
}
```

JSON 응답 예:

```json
{
  "title": "데미안",
  "author": "헤르만 헤세",
  "year": 1919,
  "id": "a12b"
}
```

JavaScript 객체를 JSON 문자열로 변환할 때 다음을 사용합니다.

```javascript
JSON.stringify(
  newBook
)
```

---

## 18. PUT - Update

`PUT`은 기존 데이터를 수정할 때 사용합니다.

자료에서는 기존 데이터를 전체적으로 교체하는 방식으로 설명합니다.

```javascript
fetch(
  "http://localhost:3001/books/1",
  {
    method: "PUT",

    headers: {
      "Content-Type":
        "application/json",
    },

    body: JSON.stringify({
      id: "1",
      title:
        "노인과 바다 (수정본)",
      author:
        "Ernest Hemingway",
      year: 1953,
    }),
  }
)
  .then(
    res => res.json()
  )
  .then(data =>
    console.log(
      "PUT 결과:",
      data
    )
  )
  .catch(err =>
    console.error(
      "PUT 실패:",
      err
    )
  );
```

```text
PUT
→ 전체 수정
```

---

## 19. PATCH - 부분 Update

`PATCH`는 변경할 속성만 전송하여 수정합니다.

```javascript
fetch(
  "http://localhost:3001/books/1",
  {
    method: "PATCH",

    headers: {
      "Content-Type":
        "application/json",
    },

    body: JSON.stringify({
      year: 1995,
    }),
  }
)
  .then(
    res => res.json()
  )
  .then(data =>
    console.log(
      "PATCH 결과:",
      data
    )
  )
  .catch(err =>
    console.error(
      "PATCH 실패:",
      err
    )
  );
```

자료에서의 구분:

```text
PUT
→ 전체 수정

PATCH
→ 부분 수정
```

---

## 20. DELETE - Delete

`DELETE`는 서버의 데이터를 삭제할 때 사용합니다.

```jsx
function DeleteBook() {
  const handleDelete =
    id => {
      fetch(
        `http://localhost:3001/books/${id}`,
        {
          method:
            "DELETE",
        }
      )
        .then(res => {
          if (!res.ok) {
            throw new Error(
              "삭제 실패"
            );
          }

          alert(
            `도서(id=${id})가 삭제되었습니다.`
          );
        })
        .catch(err =>
          console.error(err)
        );
    };

  return (
    <div>
      <h2>
        도서 삭제 테스트
      </h2>

      <button
        onClick={() =>
          handleDelete(3)
        }
      >
        id=3 도서 삭제
      </button>
    </div>
  );
}
```

선택한 ID를 이용하여 Endpoint를 동적으로 생성합니다.

```javascript
`http://localhost:3001/books/${id}`
```

---

## 21. CRUD 정리

```text
Create
POST

Read
GET

Update
PUT / PATCH

Delete
DELETE
```

React에서는 다음 흐름으로 연결됩니다.

```text
사용자 동작
      ↓
Event Handler
      ↓
fetch()
      ↓
HTTP Request
      ↓
JSON Server
      ↓
JSON Response
      ↓
State 변경
      ↓
UI 재렌더링
```

---

## 22. Book Manager 종합 실습

각 HTTP Method를 하나의 도서 관리 애플리케이션으로 통합합니다.

예제 화면의 주요 기능:

- 도서 제목 입력
- 저자 입력
- 출판년도 입력
- 도서 등록
- 도서 목록 출력
- 새로고침
- 수정
- 삭제

기능과 REST Method의 관계:

```text
Book Manager

Create
→ POST

Read
→ GET

Update
→ PUT / PATCH

Delete
→ DELETE
```

---

## 23. Todo Mini Project

마지막 실습에서는 JSON Server에서 제공하는 API를 이용하여 실제 일정 관리 기능을 구현합니다.

`db.json` 예:

```json
{
  "todos": [
    {
      "id": "1",
      "text": "리액트 컴포넌트 복습",
      "date": "2025. 10. 27",
      "completed": false
    },
    {
      "id": "2",
      "text": "기계학습 시험 공부",
      "date": "2025. 10. 27",
      "completed": false
    }
  ]
}
```

Endpoint:

```text
http://localhost:3001/todos
```

예제 화면에는 다음 기능이 포함됩니다.

- 현재 날짜 표시
- 새로운 Todo 입력
- Todo 추가
- Todo 목록
- 검색
- 완료 여부
- 삭제

REST API와 CRUD를 실제 일정 관리 데이터에 적용하는 미니 프로젝트입니다.

</details>
