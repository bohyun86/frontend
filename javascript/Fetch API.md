Fetch API는 브라우저와 서버 간의 통신을 가능하게 하는 최신 JavaScript 인터페이스로, 서버에 HTTP 요청을 보내고 그 응답을 비동기적으로 처리할 수 있게 합니다. 이로 인해, 웹 애플리케이션에서 서버와의 데이터 교환이 훨씬 더 편리해졌습니다. 하나씩 살펴보도록 하겠습니다.

### Fetch API 개요
**Fetch API**는 브라우저와 서버 간의 비동기 통신을 가능하게 하는 인터페이스로, 서버에 **HTTP 요청**을 보내고 응답을 받을 수 있습니다. 이전에는 **XMLHttpRequest**를 사용해 클라이언트와 서버가 데이터를 교환했지만, 이는 코드가 복잡하고 사용이 불편했습니다. **Fetch API**는 이러한 불편함을 해결하기 위해 만들어졌으며, **Promise** 클래스를 사용해 코드가 더 단순하고 깨끗해졌습니다.

### Fetch 사용 방법
**fetch()** 메서드는 모든 최신 브라우저에서 제공하며, 이를 호출하면 서버로 데이터를 전송하거나 서버로부터 데이터를 받을 수 있습니다. Fetch API의 사용 알고리즘은 다음과 같습니다.

#### **GET 요청 예시**
GET 요청을 보내는 기본적인 방법을 보겠습니다.

1. **fetch() 호출**: 가져오고자 하는 데이터의 **URL**을 인자로 전달합니다.
2. **응답 처리**: 응답이 JSON 형식으로 오기 때문에 `.json()` 메서드를 호출해 데이터를 추출합니다.
3. **Promise 또는 async/await로 처리**: 응답 본문을 처리하기 위해 **.then/.catch** 구문이나 **async/await** 구문을 사용할 수 있습니다.

아래는 두 가지 방식으로 동일한 GET 요청을 처리하는 예시입니다.

##### **예시 코드 1 - .then/.catch 방식**
```javascript
const test = fetch("https://jsonplaceholder.typicode.com/users")
  .then((response) => response.json())  // 응답 데이터를 JSON으로 변환
  .then((json) => console.log(json))    // JSON 데이터를 콘솔에 출력
  .catch((error) => console.log(error)); // 에러가 발생하면 처리
```

##### **예시 코드 2 - async/await 방식**
```javascript
async function fetchFunction() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    const data = await response.json(); // 응답 데이터를 JSON으로 변환
    console.log(data); // 데이터를 콘솔에 출력
  } catch (error) {
    console.log(error); // 에러가 발생하면 처리
  }  
}
```
두 예시 모두 서버로부터 받은 데이터를 **콘솔**에 출력하거나, 요청이 실패한 경우에는 **에러** 메시지를 출력합니다.

### **HTTP 요청 보내기 (GET, POST, PUT 등)**
**fetch()** 메서드를 사용하면 다양한 **HTTP 요청**을 보낼 수 있습니다. 기본적인 GET 요청 외에도 **PUT**, **POST**, **PATCH** 등 여러 종류의 요청을 사용할 수 있습니다.

#### **GET 요청에 쿼리 파라미터 추가하기**
때때로 특정 데이터를 필터링하거나 정렬하거나 일부 데이터만 가져오기 위해 **쿼리 파라미터**를 사용해야 합니다. URL 뒤에 `?` 기호를 붙이고 **name=value** 쌍으로 파라미터를 작성하며, 여러 파라미터는 `&` 기호로 구분합니다.

- **예시**:
  ```javascript
  fetch("https://jsonplaceholder.typicode.com/users?_limit=10&page=3");
  ```
  이 요청은 10개의 사용자 데이터를 가져오되 3번째 페이지의 데이터를 가져옵니다.

또는 **URL 객체**를 사용해 URL과 파라미터를 만들 수도 있습니다.

- **예시**:
  ```javascript
  const url = new URL("https://jsonplaceholder.typicode.com/users");
  url.searchParams.set("_limit", "10");
  url.searchParams.set("page", "3");

  fetch(url);
  ```
  위와 같은 방법으로 쿼리 파라미터를 동적으로 설정할 수 있습니다.

### **요청 헤더 설정**
때로는 요청에 **헤더**를 설정해야 할 때가 있습니다. 예를 들어, 인증이 필요한 경우 **인증 토큰**을 설정해야 합니다. 이때는 요청의 두 번째 인자로 **options 객체**를 전달해 **headers**를 설정할 수 있습니다.

- **예시 코드**:
  ```javascript
  const authHeaders = new Headers();
  authHeaders.set("Authorization", "ea135929105c4f29a0f5117d2960926f");

  fetch("https://jsonplaceholder.typicode.com/users", {
    headers: authHeaders
  });
  ```

또는 **headers**를 객체로 전달할 수도 있습니다.

- **예시 코드**:
  ```javascript
  fetch("https://jsonplaceholder.typicode.com/users", {
    headers: {
      "Authorization": "ea135929105c4f29a0f5117d2960926f"
    }
  });
  ```

### **POST, PUT 등 요청과 요청 본문**
GET 요청 외에 **POST**나 **PUT** 같은 요청에서는 서버에 데이터를 보낼 수 있습니다. 이때는 **options** 객체에 **method**와 **body**를 설정해야 합니다. 일반적으로 요청 본문에는 **JSON** 형식의 문자열 데이터를 사용합니다. 객체를 문자열로 변환하려면 **`JSON.stringify()`**를 사용합니다.

- **예시 코드 (POST 요청)**:
  ```javascript
  const requestBody = {
    name: "John",
    age: "16"
  };

  fetch("https://jsonplaceholder.typicode.com/users", {
    method: "POST",
    body: JSON.stringify(requestBody),
    headers: {
      "Content-Type": "application/json" // 요청 본문이 JSON 형식임을 명시
    }
  });
  ```

### **응답 처리**
fetch() 메서드는 **Promise** 객체를 반환하며, 이 객체에는 다양한 속성이 있습니다.

1. **status**: 응답 코드 (예: 200, 404 등).
2. **statusText**: 응답 코드에 대한 설명.
3. **ok**: 요청이 성공했는지를 나타내는 불리언 값 (보통 `status`가 200대일 때 `true`).
4. **headers**: 응답 헤더 목록.
5. **body**: 서버가 전송한 데이터 (하지만 초기 상태에서는 읽을 수 없는 **ReadableStream** 형식).
6. **bodyUsed**: 데이터가 읽히고 있는지 여부를 나타내는 불리언 값.

응답 본문은 **ReadableStream** 형식이므로, 즉시 사용할 수 없습니다. 일반적으로는 `.json()` 또는 `.text()` 같은 메서드를 사용해 데이터를 변환해야 합니다.

### **에러 처리**
에러는 **두 가지 방식**으로 처리할 수 있습니다.

#### **1. .then/.catch 방식**
- **예시 코드**:
  ```javascript
  fetch("https://jsonplaceholder.typicode.com/users")
    .then((response) => response.json())
    .then((json) => console.log(json))
    .catch((e) => console.log(e.message));
  ```

#### **2. async/await 방식**
- **예시 코드**:
  ```javascript
  async function fetchFunction() {
    try {
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      const data = await response.json();
      console.log(data);
    } catch (e) {
      console.log(e.message);
    }
  }
  ```

한 가지 중요한 점은 **fetch() 메서드는 네트워크 오류나 요청 실패 시에만 에러를 던진다는 것**입니다. 예를 들어, **404 (찾을 수 없음)** 같은 오류는 Promise가 거부되지 않으며, 단순히 **`response.ok`가 false**로 설정됩니다. 따라서 응답 코드가 200~209 범위에 해당하는 경우에만 성공으로 간주하고 싶다면, 직접 조건을 확인해야 합니다.

- **예시 코드 (.then/.catch 방식)**:
  ```javascript
  fetch("https://jsonplaceholder.typicode.com/users")
    .then((response) => {
      if (response.status >= 200 && response.status <= 209) {
        return response.json();
      } else {
        throw new Error("Incorrect server response");
      }
    })
    .then((json) => console.log(json))
    .catch((error) => console.log(error.message));
  ```

- **예시 코드 (async/await 방식)**:
  ```javascript
  async function fetchFunction() {
    try {
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      if (response.status >= 200 && response.status <= 209) {
        const data = await response.json();
        console.log(data);
      } else {
        throw new Error("Incorrect server response");
      }
    } catch (error) {
      console.log(error.message);
    }
  }
  ```

### **결론**
**Fetch API**는 클라이언트와 서버 간의 비동기 통신을 간편하게 만들어주는 최신 JavaScript 인터페이스입니다. **fetch()** 메서드를 사용해 서버와 데이터를 주고받으며, 이때 **URL**을 지정하고 필요한 경우 **옵션 객체**를 추가로 전달해 요청 설정을 상세하게 지정할 수 있습니다. 

Fetch API는 **Promise**를 기반으로 하여, `.then/.catch` 또는 **async/await** 방식으로 비동기 응답을 처리할 수 있습니다. 이 덕분에 코드가 간결해지고 유지보수성이 좋아졌습니다. 

웹 애플리케이션 개발에서 서버와의 상호작용은 필수적이며, Fetch API를 사용함으로써 사용자 경험을 크게 향상시킬 수 있습니다.