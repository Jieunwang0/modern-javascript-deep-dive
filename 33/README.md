# 타이머

## 호출 스케일링
타이머 함수는 ECMAScript에 정의된 빌트인 객체가 아니라 브라우저와 Node.js 환경에서 모두 전역 객체의 메서드로서 사용할 수 있는 **호스트 객체**다.
## 타이머 함수
### setTimeout / clearTimeout
타이머가 만료되면 콜백 함수가 단 한번 호출된다.
```javascript
const timeout = setTimeout(func|code[, delay, param1, param2, ...]);
```
- func: 타이머 만료 후 호출될 콜백 함수
- delay: 타이머 만료 시간(ms), 생략시 기본값 0
    - 만료되었다고 바로 콜백 함수가 실행되는 것이 아니라 task que에 콜백 함수를 등록하는 것을 지연시켰기에 delay에 지정한 시간보다 더 걸릴 수 있다.
    - 4ms 이하인 경우 최소 지연시간 4ms가 지정된다.
- param1, param2, ...: 콩백함수에 전달해야할 인수가 존재하면 세번째 이후의 인수로 지정이 가능하다.
    - IF9 이하는 불가.

---

setTimeout 함수는 생성된 타이머를 식별할 수 있는 타이머 id를 반환한다.
- 브라우저 => typeof id = number
- Node.js => typeof id = object
이 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```javascript
const timerId = setTimeout(() => console.log('Hi'), 1000);

clearTimeout(timerId); // 호출 스케일링을 취소
```

### setInterval / clearInterval
취소될 때까지 타이머가 만료될 때마다 콜백 함수가 `반복 호출되는 타이머`를 생성한다. 
```javascript
const timeout = setInterval(func|code[, delay, param1, param2, ...]);
```
setTimeout과 인수가 동일하다. 

---

setInterval 함수도 생성된 타이머를 식별할 수 있는 타이머 id를 반환한다.

- 브라우저 => typeof id = number
- Node.js => typeof id = object

이 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```javascript
let count = 1;

const timerId = setInterval(() => {
    console.log(count);
    if(count++ === 5) clearInterval(timerId); // count가 5가 되면 호출 스케일링을 취소
    }, 1000);
```

## 디바운스와 스로틀
짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 이벤트 핸들러의 과도한 호출을 방지하는 프로그래밍 기법.

```javascript
const $button = document.querySelector('button');
const $normalMsg = document.querySelector('.normal-msg');
const $debounceMsg = document.querySelector('.debounce-msg');
const $throttleMsg = document.querySelector('.throttle-msg');


const debounce = (callback, delay) =>{
  let timeId;
  return (...args) => {
    if(timeId) clearTimeout(timerId)
    timerId = setTimeout(callback, delay, ...args);
  };
};

const throttle = (callback, delay) =>{
  let timeId;
  return (...args) => {
    if(timeId) return;
    timerId = setTimeout(()=>{
      callback(...args);
      timeId = null;
    }, delay);
  };
};

$button.addEventListener('click', () => {
    $normalMsg.textContent = +$normalMsg.textContent + 1;
}, 500); // 20

$button.addEventListener('click', debounce(() => {
    $debounceMsg.textContent = +$debounceMsg.textContent + 1;
}, 500)); // 1

$button.addEventListener('click', throttle(() =>{
     $throttleMsg.textContent = +$throttleMsg.textContent + 1;
}, 500)); // 6
```

### 디바운스
짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간동안 이벤트가 발생하지 않으면 핸들러가 한번만 호출되도록 한다.

- 이벤트가 발생하면 이전 **타이머를 계속 리셋**하고 설정한 **일정 시간(delay)동안 이벤트가 발생하지 않았을 때** 콜백 함수를 딱 한번 호출한다.

- e.g., 화면 resize 이벤트, input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성, 버튼 중복 클릭 방지 처리

![debounce](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/780186fa-f965-462c-a165-ac6018ef8f37)

### 스로틀
짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 **일정 시간(delay) 단위로 이벤트 핸들러가 호출**되도록 한다.
- e.g., scroll 이벤트처리나 무한 스크롤 UI구현
- 실무에서 사용시 Underscore 혹은 Lodash의 throttle 함수 사용을 권장

![throttle](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/d234d7c3-51c0-490c-85fe-272e5cb9d9e8)
