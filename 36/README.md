# REST API

HTTP 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처를 가지고 서비스 API를 구현한 것을 의미한다.

## 구성

REST API는 자원resource, 행위verb, 표현representations 이 세 가지로 구성된다. REST는 자체 표현 구조로 구성되어 있어 REST API만으로도 HTTP 요청을 이해할 수 있다.

| 구성 요소       | 내용                           | 표현 방법        |
| --------------- | ------------------------------ | ---------------- |
| resource        | 자원                           | URI(endpoint)    |
| verb            | 자원에 대한 행위               | HTTP 요청 메서드 |
| representations | 자원에 대한 행위의 구체적 내용 | payload          |

## 설계 원칙

1. URI는 resource를 표현해야한다.
    - _식별할 수 있는 이름으로_ 표현
    - get 같은 행위 표현을 URI에 넣으면 X
2. 리소스에 대한 행위는 *HTTP 요청 메소드*로 표현한다.
    - HTTP 요청 메소드는 서버에게 요청의 종류와 목적을 알리는 방법이다.

# Promise(ES6)

## 비동기 처리를 위한 콜백 패턴의 단점

### 콜백 지옥

콜백 함수를 통해 비동기 처리 결과에 따른 후속 처리를 수행하는 비동기 함수가 비동기 처리 경과를 가지고 또 비동기 함수를 호출해야한다면 콜백 함수 호출이 중첩되고 복잡도가 높아지며, 개발자가 의도한대로 실행이 되지 않을 수 있다.

```javascript
const get = (url, callback) => {
    const xhr = new XMLHttpRequest();

    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            callback(JSON.parse(xhr.response));
        } else {
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};

const url = "https://jsonplaceholder.typicode.com";

get(`${url}/posts/1`, ({ userId }) => {
    console.log(userId);

    get(`${url}/users/${userId}`, (suerInfo) => {
        console.log(userInfo);
    });
});
```

위 예제를 보면 get 요청을 통해 서버에서 응답을 취득하고 그 데이터를 사용해서 또 콜백 함수로 get 요청을 한다.

### 에러 처리의 한계

에러가 catch 블록에서 캐치되지 않는다.

```javascript
try {
    setTimeout(() => {
        throw new Error("Error!");
    }, 1000);
} catch (e) {
    console.error("error:", e);
}
```

setTimeout 함수의 콜백함수가 실행될 때 이미 setTimeout 함수는 콜 스택에서 제거된 상태다. 따라서 setTimeout 함수의 콜백함수를 호출한 게 setTimeout 함수가 아니다. 호출자가 setTimeout 함수라면 setTimeout 함수가 실행 컨텍스트에 있었을 때 하위 컨텍스트에 콜백함수가 있었어야 한다.

에러는 호출자 방향으로 전파된다. 따라서 에러가 catch 블록에서 캐치되지 않는다.

## 생성

ECMAScript 사양에 정의된 표준 빌트인 객체이다.

```javascript
const promise = new Promise((resolve, reject) => {...})
```

-   resolve: 비동기 처리 성공시 호출
-   reject: 비동기 처리 실패시 호출

![promise-resolve-reject](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/f024804c-061d-4ae7-84c9-e1248e0981f9)


|프로미스의 상태정보|의미|상태 변경 조건|
|:---:|---|---|
|pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태|
|fulfilled|비동기 처리가 수행된 상태(성공)|resolve 함수 호출되면 fulfilled로 변경|
|rejected|비동기 처리가 수행된 상태(실패)|reject 함수 호출되면 rejected로 변경|


## 후속 처리 메서드

### Promise.prototype.then

```javascript
.then(callback1, callback2)
```

-   callback1: 비동기 처리 성공시 호출되는 성공 처리 콜백 함수
-   callback2: 비동기 처리 실패시 호출되는 실패 처리 콜백 함수
-   언제나 프로미스를 반환한다.

```javascript
// fulfilled
new Promise((resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.error(e)); // fulfilled

// rejected
new Promise((_, reject => reject('rejected'))
    .then(v => console.log(v), e => console.error(e)); // Error: rejected
```

### Promise.prototype.catch

```javascript
.catch(callback)
```

-   callback: 프로미스가 reject 상태일 경우에만 호출한다.
-   then(undefined, onRejected)와 동일하게 동작한다.
-   언제나 프로미스를 반환한다.

```javascript
// rejected
new Promise((_, reject => reject(new Error('rejected')))
    .catch(e => console.error(e)); // Error: rejected
```

### Promise.prototype.finally

```javascript
.finally(callback)
```

-   callback: 성공과 실패 상관없이 무조건 한번 호출된다.
-   언제나 프로미스를 반환한다.
-   프로미스의 상태와 상관없이 공통적으로 수행해야하는 내용이 있을 경우에 사용한다.

```javascript
// always
new Promise(() => {}).finally(() => console.log("finally")); // finally
```

### 에러 처리

앞에서 소개한 후속 처리 메서드를 통해 Promise는 에러 처리를 문제없이 할 수 있다.

```javascript
const promiseGet = (url) => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();
        xhr.onload = () => {
            if(xhr.status === 200) {
                resolve(JSON.parse(xhr.response));
            } else {
                reject(new Error(xhr.status));
            }
        };
    });
};

promiseGet("https://jsonplaceholder.typicode.com/todos/1")
    .then(res => console.log(res))
    .catch(err => console.error(err));
```

### 프로미스 체이닝
: 프로미스의 후속 처리 메서드들은 `언제나 프로미스를 반환하므로 연속 호출이 가능하다.`

앞에서 봤던 비동기 처리를 위한 콜백 패턴에서와 다르게 프로미스에서는 프로미스 체이닝을 통해서 비동기 처리 결과에 따라 후속 처리를 결정할 수 있으므로 콜백 패턴에서 발생하던 콜백 지옥이 발생하지 않는다.

## 프로미스 정적 메서드

### Promise.resolve / Promise.reject
이미 존재하는 값을 래핑해서 프로미스를 생성하기 위해 사용한다.

- Promise.resolve는 인수로 받은 값을 resolve 하는 프로미스 생성
```javascript
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```
- Promise.reject는 인수로 받은 값을 reject 하는 프로미스 생성
```javascript
const rejectedPromise = Promise.reject(new Error("error!"));
resjectedPromise.then(console.log); // error!
```
### Promise.all
여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.
- 여러 개의 Promise가 모두 fulfilled가 되면 배열에 저장해 새로운 프로미스를 반환한다.
- 여러 개의 Promise 중 하나라도 reject를 반환하거나 에러가 날 경우, 모든 Promise를 reject 시킨다.
- 인수로 받은 이터러블이 요소가 프로미스가 아닌 경우 resolve 메서드를 통해 프로미스로 래핑한다.
```javascript
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1(), requestData2(), requestData3()])
    .then(console.log) //  Promise {<pending>} 후 [1, 2, 3] => 약 3초 소요
    .catch(console.error);
```

### Promise.race
- 여러 개의 Promise 중 가장 먼저 fulfilled 된 프로미스의 처리 결과를 resolve 하는 새로운 프로미스를 반환한다.
- 여러 개의 Promise 중 하나라도 reject를 반환하거나 에러가 날 경우, 모든 Promise를 reject 시킨다.
```javascript
Promise.race([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
    new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
    new Promise(resolve => setTimeout(() => resolve(3), 1000)) // 3
])
    .then(console.log) // 3
    .catch(console.error);
```

### Promise.allSettled
- 모두 settled 상태가 되면 모든 프로미스들의 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다.
    - fulfilled 상태일 경우: status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티
    - reject 상태일 경우: status 프로퍼티와 에러를 나타내는 reason 프로퍼티

```javascript
Promise.allSettled([
    new Promise(resolve => setTimeout(() => resolve(1), 2000)),
    new Promise((_, rejected) => setTimeout(() => reject(new Error('Error!')), 1000));
]).then(console.log);

/**
[
    {status: 'fulfilled', value: 1}
    {status: 'rejected', reason: Error: Error! at <anonymous>:3:56}
]
*/
```

## microtask queue
마이크로태스크 큐는 태스크 큐보다 우선순위가 높다. 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐를 확인하고 태스크 큐를 확인한다.

```javascript
setTimeout(() => console.log(1), 0);

Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));

// 2 3 1
```
프로미스의 후속 처리 메서드의 콜백함수는 마이크로태스크 큐에 저장되기 때문에 1 2 3이 아니라 2 3 1 순서로 출력된다. 반면 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 저장된다.

## fetch
XMLHttpRequest 객체처럼 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 API.
```javascript
const promise = fetch(url[, options])
```
- HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.
- 404나 500 같은 HTTP 에러가 발생해도 reject하지 않고 불리언 타입의 ok 상태를 false로 설정한 Respoonse 객체를 resolve한다.
    - 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject한다.
    - 불리언 타입의 ok 상태를 확인해서 명시적으로 에러를 처리해야한다.



```javascript
const request = {
    get(url) {
        return fetch(url)
    },
    post(url, payload) {
        return fetch(url, {
        method: 'POST',
        headers: { 'content-Type': 'application/json' },
        body: JSON.stringify(payload)
    });
    },
    patch(url, payload) {
        return fetch(url, {
        method: 'PATCH',
        headers: { 'content-Type': 'application/json' },
        body: JSON.stringify(payload)
    });
    },
    delete(url) {
        return fetch(url, {method: 'DELETE' });
    }
};
```
