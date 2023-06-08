# Vanilla Javascript

- 순수 자바스크립트에서의 http 통신
```js
// GET
const xhr = new XMLHttpRequest();
xhr.open('GET', 'http://localhost:5000/todos');
xhr.send();

xhr.onreadystatechange = function (e) {
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if(xhr.status === 200) { // 200: OK => https://httpstatuses.com
    console.log(xhr.responseText);
  } else {
    console.log("Error!");
  }
};

// POST
const xhr = new XMLHttpRequest();
xhr.open('POST', 'http://localhost:5000/todos');
xhr.setRequestHeader('Content-type', 'application/json');
xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: true }));

xhr.onreadystatechange = function (e) {
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if(xhr.status === 201) { // 201: Created
    console.log(xhr.responseText);
  } else {
    console.log("Error!");
  }
};
```

# ES6

## Promise 객체를 통한 비동기 처리
- Promise 생성자 함수가 인자로 전달받은 콜백 함수는 내부에서 비동기 처리 작업을 수행한다. 이때 비동기 처리가 성공하면 콜백 함수의 인자로 전달받은 resolve 함수를 호출
```js
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});

// promise 객체를 사용한 http통신
const promiseAjax = (method, url, payload) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open(method, url);
    xhr.setRequestHeader('Content-type', 'application/json');
    xhr.send(JSON.stringify(payload));

    xhr.onreadystatechange = function () {
      // 서버 응답 완료가 아니면 무시
      if (xhr.readyState !== XMLHttpRequest.DONE) return;

      if (xhr.status >= 200 && xhr.status < 400) {
        // resolve 메소드를 호출하면서 처리 결과를 전달
        resolve(xhr.response); // Success!
      } else {
        // reject 메소드를 호출하면서 에러 메시지를 전달
        reject(new Error(xhr.status)); // Failed...
      }
    };
  });
};

// 후속처리
promiseAjax('GET', 'http://jsonplaceholder.typicode.com/posts/1')
    .then(JSON.parse)
    .then(
        // 첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출된다.
        render,
        // 두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.
        console.error
    );

// 에러처리
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseAjax(wrongUrl)
  .then(res => console.log(res), err => console.error(err)); // Error: 404

// catch처리
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseAjax(wrongUrl)
  .then(res => console.log(res))
  .catch(err => console.error(err)); // Error: 404
```

## Promise 체이닝
```html
<!DOCTYPE html>
<html>
<body>
  <pre class="result"></pre>
  <script>
    const $result = document.querySelector('.result');
    const render = content => { $result.textContent = JSON.stringify(content, null, 2); };

    const promiseAjax = (method, url, payload) => {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.setRequestHeader('Content-type', 'application/json');
        xhr.send(JSON.stringify(payload));

        xhr.onreadystatechange = function () {
          if (xhr.readyState !== XMLHttpRequest.DONE) return;

          if (xhr.status >= 200 && xhr.status < 400) {
            resolve(xhr.response); // Success!
          } else {
            reject(new Error(xhr.status)); // Failed...
          }
        };
      });
    };

    const url = 'http://jsonplaceholder.typicode.com/posts';

    // 포스트 id가 1인 포스트를 검색하고 프로미스를 반환한다.
    promiseAjax('GET', `${url}/1`)
      // 포스트 id가 1인 포스트를 작성한 사용자의 아이디로 작성된 모든 포스트를 검색하고 프로미스를 반환한다.
      .then(res => promiseAjax('GET', `${url}?userId=${JSON.parse(res).userId}`))
      .then(JSON.parse)
      .then(render)
      .catch(console.error);
  </script>
</body>
</html>
```

## Promise.all
- 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다. 
- 전달받은 모든 프로미스를 병렬로 처리하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

```js
Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(console.log) // [ 1, 2, 3 ]
  .catch(console.log);

// 첫번째 프로미스는 3초 후에 1을 resolve하여 처리 결과를 반환한다.
// 두번째 프로미스는 2초 후에 2을 resolve하여 처리 결과를 반환한다.
// 세번째 프로미스는 1초 후에 3을 resolve하여 처리 결과를 반환한다.  
```

## Promise.race
- Promise.all 메소드와 동일하게 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다.
- Promise.all과는 다르게 병렬 처리하는 것이 아니라 가장 먼저 처리된 프로미스가 resolve한 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

```js
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(console.log) // 3
  .catch(console.log);
```

## async & await

### 기본 문법
```js
async function 함수명() {
  await 비동기_처리_메서드_명();    // 비동기 처리 메서드는 반드시 promise 객체를 반환해야 함
}
```
- 일반적으로 await의 대상이 되는 비동기 처리 코드는 Axios 등 프로미스를 반환하는 API 호출 함수

```js
function fetchItems() {
  return new Promise(function(resolve, reject) {
    var items = [1,2,3];
    resolve(items)
  });
}

async function logItems() {
  var resultItems = await fetchItems();
  console.log(resultItems); // [1,2,3]
}

// 예외처리
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title); // delectus aut autem
    }
  } catch (error) {
    console.log(error);
  }
}
```

### 예제
```js
const fetch = require('node-fetch');

// Promise를 반환하는 함수 정의
function getUser(username) {
  return fetch(`https://api.github.com/users/${username}`)
    .then(res => res.json())
    .then(user => user.name);
}

async function getUserAll() {
  let user;
  user = await getUser('jeresig');
  console.log(user);

  user = await getUser('ahejlsberg');
  console.log(user);

  user = await getUser('ungmo2');
  console.log(user);
}

getUserAll();
```

## axios
- 브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리

### axios 사용법
```text
- 패키지 매니저를 통해 다운로드
yarn add axios
npm i axios

- 소스에 import
import axios from 'axios'
```

### http methods
```text
- GET
axios.get(url,[,config])

- POST
axios.post("url주소",{
    	data객체
    },[,config])

- DELETE
axios.delete(URL,[,config]);

- PUT
axios.put(url[, data[, config]])
```

### 예제
```js
const axios = require('axios');

const getBreeds = async () => {
  try {
    return await axios.get('https://dog.ceo/api/breeds/list/all');
  } catch (error) {
    console.error(error);
  }
};

const countBreeds = async () => {
  const breeds = await getBreeds();
  
  if (breeds.data.message) {
    console.log(`현재 강아지의 수는 ${Object.entries(breeds.data.message).length}입니다.`);
  }
};

countBreeds();
```

```html
<body>
        <div>
            <input type="email" placeholder="email을 입력해주세용" id="email" value="" >
        </div>
        <div>
            <input type="password" placeholder="비밀번호를 입력해주세용" id="pw" value="">
        </div>
        
        <input type="button" onclick='onLoggin()' value="로그인">
</body>
<script>
    // https://github.com/lifeainteasy/FrontEnd/blob/master/axios/login.html
    function onLoggin(){
        const email = document.getElementById("email");
        const password = document.getElementById('pw')
        axios({
            method:"POST",
            url: 'https://reqres.in/api/login',
            data:{
                "email": email.value,
                "password": password.value
            }
        }).then((res)=>{
            console.log(res);
        }).catch(error=>{
            console.log(error);
            throw new Error(error);
        });
    }
</script>
```

## Array spread 연산자
- [ ... arr] 와 같이 표기한다
- 원본 배열을 바꾸지 않고 열거 가능한 요소를 하나씩 전개한다.

```javascript
    let data = ["godori", "irodog", "roodig"];
    let newData = [...data];
    console.log( newData === data );  // false


    function sum(a,b,c){
        return a+b+c;
    }
    let arr = [100,200,300];
    
    console.log( sum.apply(null, arr) );  // 인자를 배열 형태로 받아 출력
    console.log( sum(...arr) );           // 이렇게 써도 동일한 결과 출력
```