# Ajax
: Asynchronous JavaScript and XML

자바스크립트를 이용해서 비동기적(Asynchronous)으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식.

Ajax의 등장으로 필요한 부분만 한정적으로 렌더링하는 것을 할 수 있게 되면서 브라우저에서도 애플리케이션과 같은 빠른 퍼포먼스와 부드러운 화면전환이 가능해졌다.
- 갱신하는데 필요한 데이터만 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
- 불필요한 리렌더링이 발생하지 않는다. (= 화면 깜박임 현상 X)
- 클라이언트와 서버의 통신이 비동기적으로 이루어지기 때문에 블로킹 발생 X

![ajax-webpage-lifecycle](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/2c2155ef-6ddc-45ef-8576-c5e8b2a240b8)

## JSON
: JavaScript Object Notation

클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷.
### 표기 방식
-  키는 반드시 큰따옴표로 둘러싸야 한다. (작은따옴표 사용 불가)
```javascript
{
  "name": "Lee",
  "age": 25,
  "alive": true,
  "hobby": ["traveling", "swimming"]
}
```
### JSON.stringify
- **객체를 JSON 형식의 문자열로** 변환한다.
- 클라이언트에서 서버로 객체를 전송하려면 문자열화하여야 하는데 이를 `직렬화`(Serializing)이라 한다.
-  객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.
```javascript
JSON.stringify(value[, replacer[, space]])
```
```javascript
const obj = {
  name: 'Lee',
  age: '20',
  hobby: ['soccer', 'drawing'],
  address: 'Orange Town'
}

const json = JSON.stringify(obj);
console.log(typeof json, json); 
// string {"name":"Lee","age":"20","hobby":["soccer","drawing"],"address":"Orange Town"}

// 줄바꿈으로 가독성 좋게 하는 법(null, 2 사용)
const prettyjson = JSON.stringify(obj, null, 2);
console.log(typeof prettyjson, prettyjson);
/** string {
  "name": "Lee",
  "age": "20",
  "hobby": [
    "soccer",
    "drawing"
  ],
  "address": "Orange Town"
}
*/

// 줄바꿈으로 가독성 좋게 하는 법(함수 사용)
function filter(key, value) {
return typeof value === 'number' ? undefined : value;
}

const filterjson = JSON.stringify(obj, filter, 2);
console.log(typeof filterjson, filterjson);
/** string {
  "name": "Lee",
  "age": "20",
  "hobby": [
    "soccer",
    "drawing"
  ],
  "address": "Orange Town"
}
*/
```

### JSON.parse
- JSON 포맷의 **문자열을 객체로** 변환한다.
- **서버로부터 브라우저로 전송된 `JSON 데이터는 문자열이다.`** 이 문자열을 객체로서 사용하려면 객체화하여야 하는데 이를 `역직렬화`(Deserializing)이라 한다.
- 배열이 JSON 형식의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```javascript
JSON.parse(text[, reviver])
```

```javascript
const obj = {
  name: 'Lee',
  age: '20',
  hobby: ['soccer', 'drawing'],
  address: 'Orange Town'
}

const json = JSON.stringify(obj);

const parsed = JSON.parse(json); 
console.log(typeof parsed, parsed);
// object {name: 'Lee', age: '20', hobby: Array(2), address: 'Orange Town'}
```

## XMLHttpRequest
브라우저에서는 HTTP 요청을 전송하기 위한 a 태그나 form 태그를 제공한다.

자바스크립트가 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

- 브라우저는 XMLHttpRequest 객체를 이용하여 Ajax 요청을 생성하고 전송한다. 서버가 브라우저의 요청에 대해 응답을 반환하면 같은 XMLHttpRequest 객체가 그 결과를 처리한다.
###  XMLHttpRequest 객체
```javascript
const xhr = new XMLHttpRequest();
```
**<프로토타입 프로퍼티>**
|프로토타입 프로퍼티|설명|
|---|---|
|readyState|HTTP 요청의 현재 상태를 나타내는 정수. XMLHttpRequest의 정적 프로퍼티를 값으로 갖는다.|
|status|HTTP 요청에 대한 응답 상태를 나타내는 정수 e.g., 200, 304, 302, ...|
|statusText|HTTP 요청에 대한 응답 메세지를 나타내는 문자열|
|responseType|HTTP 응답 타입|
|response|HTTP 요청에 대한 응답 몸체|
|responseText|서버가 전송한 HTTP 요청에 대한 응답 문자열|

**<이벤트 핸들러 프로퍼티>**
|이벤트 핸들러 프로퍼티|설명|
|---|---|
|onreadystatechange|readyState 값이 변경된 경우|
|onloadstart|HTTP 요청에 대한 응답을 받기 시작한 경우|
|onprogress|HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|
|onabort|abort 요청에 의해 HTTP 요청이 중단된 경우|
|onerror|HTTP 요청에 에러가 발생한 경우|
|onload|HTTP 요청이 성공적으로 완료된 경우|
|ontimeout|HTTP 요청시간이 초과한 경우|
|onloadend|HTTP 요청이 (성공과 실패 상관없이) 완료된 경우. 성공 또는 실패하면 발생|

**<메서드>**
|메서드|설명|
|:---:|---|
|open|HTTP 요청 초기화|
|send|HTTP 요청 전송|
|abort|이미 전송된 HTTP 요청 중단|
|setRequestHeader|특정 HTTP 요청 헤더의 값을 설정|
|getResponseHeader|특정 HTTP 요청 헤더의 값을 문자열로 변환|


**<정적 프로퍼티>**
|정적 프로퍼티|값|설명|
|:---:|:---:|---|
|UNSENT|0|open 메서드 호출 이전|
|OPENED|1|open 메서드 호출 이후|
|HEADERS_RECEIVED|2|send 메서드 호출 이후|
|LOADING|3|서버 응답 중(응답 데이터 미완성 상태)|
|DONE|4|서버 응답 완료|

### HTTP 요청 전송
```javascript
const xhr = new XMLHttpRequest();

xhr.open('GET', '/user');

// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

xhr.send();
```
1. open 메서드로 HTTP 요청 초기화
2. 필요하면 setRequestHeader 메서드로 요청 헤더값을 설정
3. send 메서드로 HTTP 요청 전송

### open: 

HTTP 요청 초기화

```javascript
xhr.open('method', 'url[, async]')
```

|매개변수|설명|
|:---:|---|
|method|HTTP 요청 메서드|
|url|요청을 전송할 URL|
|async|비동기 요청 여부. 기본값 true, 비동기 방식으로 동작한다.|

HTTP 요청 메서드로 CRUD를 구현한다. 

|HTTP 요청 메서드|종류|목적|페이로드|
|:---:|:---:|:---:|:---:|
|GET|index / retreive|모든/특정 리소스 취득|X|
|POST|create|리소스 생성|O|
|PUT|replace|리소스의 전체 교체|O|
|PATCH|modify|리소스의 일부 수정|O|
|DELETE|delete|모든/특정 리소스 삭제|X|


### send: 
open으로 요청 초기화된 HTTP 요청을 서버에 전송한다.
```javascript
xhr.send()
```
- GET 요청에 send: 데이터를 url의 일부인 쿼리 문자열로 서버에 보낸다
  - GET 요청에 send할 때 인수를 전달할 경우 페이로드로 전달한 인수는 무시되고 요청 몸체는 null
- POST 요청에 send: 데이터(payload)를 요청 몸체에 담아 보낸다.
- 페이로드가 객체인 경우 반드시 직렬화한 다음 전달해야 한다.


### setRequestHeader:
특정 HTTP 요청의 헤더값을 설정한다.

```javascript
xhr.setRequestHeader('MIME type', 'sub type')
```
- 반드시 open 호출 이후에 호출 가능

|MIME 타입|서브 타입|
|:---:|---|
|text|text/plain, text/html, text/css, text/javascript|
|application|application/json, application/x-www-form-urlencode|
|multipart|multipart/formed-data|


### HTTP 응답 처리
XMLHttpRequest 객체의 **이벤트 핸들러 프로퍼티**를 사용해서 현재 상태를 확인하고 필요한 에러 처리를 하거나 데이터를 취득할 수 있다.

```javascript
const xhr = new XMLHttpRequest();

xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

xhr.send();

xhr.onload = () => {
  if(xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
// 상태코드가 200이면 응답을 받아서 parse하고, 200이 아니면 Error 처리를 한다.
```
