# 이벤트

- `이벤트 핸들러:` 이벤트가 발생했을 때 브라우저에 의해 호출될 **함수**
- `이벤트 핸들러 등록:` 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 **호출을 위임하는 것**

이렇게 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 **이벤트 드리븐 프로그래밍**이라고 한다.


### 이벤트 타입

**(1) Mouse Event**

|타입|이벤트 발생 시점|
|---|---|
|click|마우스를 클릭했을 때|
|dbclick|마우스를 더블 클릭했을 때|
|mousedown|마우스를 눌렀을 때|
|mouseup|누르고 있던 마우스를 뗐을 때|
|mousemove|마우스를 움직였을 때|
|mouseenter|마우스를 HTML 요소 안으로 이동했을 때 (버블링 X)|
|mouseover|마우스를 HTML 요소 안으로 이동했을 때 (버블링 O)|
|mouseleave|마우스를 HTML 요소 밖으로 이동했을 때 (버블링 X)|
|mouseout|마우스를 HTML 요소 밖으로 이동했을 때 (버블링 O)|

---
**(2) KeyBoard Event**
|타입|이벤트 발생 시점|
|---|---|
|keydown|키를 눌렀을 때 발생 (문자,숫자,특수문자,enter -> 연속적 발생 / 그 외의 키 -> 한 번만 발생)|
|keypress|문자키를 눌렀을 때 연속적 발생, 폐지됨. 사용 권장 X|
|keyup|누르던 키를 놓았을 때 한 번만 발생|

**(3) Focus Event**
|타입|이벤트 발생 시점|
|---|---|
|focus|HTML 요소가 포커싱되었을 때 (버블링 X)|
|blur|HTML 요소가 포커스를 잃었을 때 (버블링 X)|
|focusin|HTML 요소가 포커싱되었을 때 (버블링 O)|
|focusout|HTML 요소가 포커스를 잃었을 때 (버블링 X)|

**(4) Form Event**

|타입|이벤트 발생 시점|
|---|---|
|submit|form 내의 input(text, checkbox, radio), select(textarea 제외) 입력 필드 키를 눌렀을 때, submit(button, input type="submit") 버튼을 클릭했을 때 |
|reset|form 내의 reset을 눌렀을 때(요샌 사용 X)|

**(5) Change Value Event**
|타입|이벤트 발생 시점|
|---|---|
|input|input(text, checkbox, radio), select, textarea 요소의 값이 입력되었을 때|
|change|input(text, checkbox, radio), select, textarea 요소의 값이 변경되었을 때 (change 이벤트는 포커스를 잃었을 때 사용자 입력이 종료되었다고 인식한다.)|
|readystatechange|HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값이 변경되었을 때 ('loading', 'interaction', 'complete')|

**(6) DOM Mutation Event**

|타입|이벤트 발생 시점|
|---|---|
|DOMContentLoaded|HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때|

**(7) View Event**
|타입|이벤트 발생 시점|
|---|---|
|resize|브라우저 윈도우의 크기를 리사이즈할 때 연속적 발생|
|scroll|웹페이지(document) or HTML 요소를 스크롤할 때 연속적 발생|

**(8) Resource Event**
|타입|이벤트 발생 시점|
|---|---|
|load|DOMContentLoaded 이벤트가 발생한 이후 모든 리소스의 로딩이 완료되었을 때 발생|
|unload|리소스가 언로드될 때|
|abort|리소스 로딩이 중단되었을 때|
|error|리소스 로딩이 실패했을 때|


## 이벤트 핸들러 등록

### 1) EventHandler Attribute

DOM Level 0부터 도입된 EventHandler Attribute

- 하나의 이벤트에 하나 이상의 이벤트 핸들러를 바인딩할 수 있다.
- onclick과 같이 on접두사 + 이벤트 타입으로 이루어져 있으며, 함수 참조가 아닌 함수 호출문 등의 문을 할당한다.
- HTML과 자바스크립트가 혼재된다. 
- 이벤트 핸들러 어트리뷰트 값은 암묵적으로 생성될 이벤트 핸들러의 함수 몸체이다.
- 이벤트 핸들러 어트리뷰트에서 이벤트 핸들러에 의해 호출된 함수는 일반 함수로서 호출되므로 함수 내부의 this는 전역 객체 window를 가리킨다.
    - 단, 이벤트 핸들러를 호출할 때 인수로 전달한 this는 바인딩한 DOM 요소를 가리킨다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <button onclick="sayHi('Lee')">Click</button>
 <script>
    function sayHi(lastName) {
        console.log(`Hi, ${lastName}`);
    }
    </script>
</body>
<html>
```
위의 onclick="sayHi('Lee')" 어트리뷰트는 파싱되어 다음과 같은 함수를 암묵적으로 생성하고 이밴트 핸들러 어트리뷰트의 이름과 동일한 키 onclick 이벤트 핸들러 프로퍼티에 할당한다.
```javascript
    function onclick(event) {
        sayHi("Lee");
    }
```

- CBD(Component Based Development) 방식의 Angular/React/Svelte/Vue.js 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리한다.

```javascript
// Angular
<button (click)="handleClick($event)">Save</button>

// React
<button onClick={handleClick}>Save</button>

// Svelte
<button on:click={handleClick}>Save</button>

// Vue.js
<button v-on:click="handleClick($event)">Save</button>
```

### 2) EventHandler Property

DOM Level 0부터 도입된 EventHandler Property

- 하나의 이벤트에 하나의 이벤트 핸들러만 바인딩할 수 있다.
- onclick과 같이 on접두사 + 이벤트 타입으로 이루어져 있으며, 함수를 바인딩한다.
- 전파된 이벤트를 DOM 객체에 바인딩하므로 이벤트 핸들러 어트리뷰트 방식과 동일하다고 할 수 있지만, HTML과 자바스크립트가 뒤섞이지 않는다는 차이가 있다.
- 이벤트 핸들러 프로퍼티의 이벤트 핸들러는 메소드이므로 핸들러 내부의 this는 이벤트에 바인딩된 요소를 가리킨다. **(this === e.currentTarget)**

![eventhandlerProperty](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/adf3a67b-ff7c-4b9c-89a4-df567f6d6278)


```javascript
<!DOCTYPE html>
<html>
<body>
    <button>Click</button>
 <script>
 const $button = document.querySelector(button);

// 1
    $button.onclick = function () {
        console.log('button click 1');
    }
// 2
    $button.onclick = function () {
        console.log('button click 2');
    }
    </script>
</body>
<html>

// 하나의 이벤트에 하나의 이벤트 핸들러만 바인딩할 수 있다
// 1은 2에 의해 재할당되었기 떄문에 2만 실행된다
```

### 3) addEventListener
DOM Level 2에서 도입된 EventTarget.prototype.addEventListener

- 하나의 이벤트에 (참조가 다른) 하나 이상의 이벤트 핸들러를 바인딩할 수 있다.
- 캡처링과 버블링을 지원한다.
- 브라우저는 웹 문서(HTML, XML, SVG)를 로드한 후, 파싱하여 DOM을 생성한다.
- 타겟 DOM 요소를 지정하지 않으면 전역객체 window(DOM 문서를 포함한 브라우저의 윈도우)에서 발생하는 click 이벤트에 이벤트 핸들러를 바인딩한다.
- 이벤트 핸들러 내부의 this는 이벤트 리스너에 바인딩된 요소를 가리킨다. **(this === e.currentTarget)**

![event_listener](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/074c867d-ce6b-4541-afd6-b7f899b8b53c)

```javascript
<!DOCTYPE html>
<html>
<body>
<button>Click</button>
 <script>
 const $button = document.querySelector(button);

    $button.addEventListener('click', function () {
      alert(`1st click!`);
    });
    $button.addEventListener('click', function () {
      alert(`2nd click!`);
    });

  </script>
</body>
</html>
```
## 이벤트 전파 (Event Propagation)

![eventflow](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/b005b5cd-93f2-4479-8d96-ed50322e9104)

계층적 구조에 포함되어 있는 HTML 요소에 이벤트가 발생할 경우 이벤트가 전파되는 방향에 따라 캡처링(Event Capturing)과 버블링(Event Bubbling)으로 구분되는 반응이 나타난다.

**3단계:** 
1. *캡처링:* 상위 요소부터 시작해서 => 이벤트 타겟(하위)으로 전파
2. *타겟:* 이벤트를 발생시킨 이벤트 타겟에 도달
3. *버블링:* 이벤트 타겟부터 시작해서(하위) => 상위 요소로 전파

**=> 이벤트가 발생했을 때 캡처링과 버블링은 순차적/연쇄적으로 발생한다.**

이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타겟/버블링만 포착이 가능하지만 `addEventListener`로 등록한 이벤트 핸들러는 **3번째 인수로 true를 전달하면 캡처링도 선별적으로 캐치가 가능**하다. (false => 타겟/버블링만 캐치)

## 이벤트 위임
이 예시에서는 같은 이벤트를 버튼별로 바인딩하고 있다.
```javascript
<!DOCTYPE html>
<html>
<body>
  <div class="fruits">
    <button id="apple">apple</button>
    <button id="strawberry">strawberry</button>
  </div>

  <script>
    function hideEvent(e) {
      e.target.style.visibility = 'hidden';
      // = this.style.visibility = 'hidden';
    }

    document.getElementById('apple').addEventListener('click', hideEvent);
    document.getElementById('strawberry').addEventListener('click', hideEvent);
  </script>
</body>
</html>
```
이벤트 위임을 통해 코드를 재작성하면,
```javascript
<!DOCTYPE html>
<html>
<body>
  <div class="fruits">
    <button id="apple">apple</button>
    <button id="strawberry">strawberry</button>
  </div>

  <script>
  const fruits = document.getElementById('fruits');

    function hideEvent(e) {
      e.target.style.visibility = 'hidden';
      // = this.style.visibility = 'hidden';
    }

    fruits.addEventListener('click', hideEvent);
  </script>
</body>
</html>
```
이게 가능하려면 this와 e.target이 일치해야하는데, 항상 일치하지는 않는다.

## 이벤트 핸들러 제거

- 이벤트 핸들러의 참조를 변수나 자료구조에 저장하고 있어야 제거할 수 있다. (= 무명함수 제거 불가)
- 단, 기명 이벤트 핸들러 내부에서 removeEventListener 메서드를 호출한 경우 제거가 가능하다. 
- 기명 함수로 이벤트 핸들러 호출이 불가하다면 arguments.callee로 자신을 호출할 수 있다. (하지만 callee는 최적화를 방해하므로 strict 모드에서 사용 금지된다. 가급적이면 핸들러의 참조를 변수나 자료구조에 저장하는 것을 지향해야 한다.)

```javascript
// (1) 무명 함수는 등록된 이벤트 핸들러를 참조할 수 없어서 제거 불가
$button.removeEventListener('click', () => console.log('button click'));


// (2) 기명 이벤트 핸들러 내부에서 메서드 호출
$button.addEventListener('click', function clickEvent() {console.log("I'm clicked!")};

    // 이벤트 핸들러가 제거되므로 이벤트 핸들러는 한번만 호출된다
    $button.removeEventListener('click', clickEvent);
     );


// (3) 기명 이벤트 핸들러로 등록할 수 없으면 내부에서 arguments.callee 호출
$button.addEventListener('click', function () {console.log("I'm clicked!")};

    // 이벤트 핸들러가 제거되므로 이벤트 핸들러는 한번만 호출된다
    $button.removeEventListener('click', arguments.callee); // 제거 성공
     );
```

- 전달한 인수가 addEventListener와 동일해야 제거할 수 있다.

```javascript
const clickEvent = () => {
     console.log("I'm clicked!")};
$button.addEventListener('click', clickEvent);

// $button.removeEventListener('click', clickEvent, true); 
// 전달한 인수가 addEventListener와 동일하지 않아서 제거 실패
    
    $button.removeEventListener('click', clickEvent); // 제거 성공
```

- 이벤트 핸들러 프로퍼티 방식은 removeEventListener로 제거가 불가하고, null을 할당해줘야 한다. 

```javascript
// 이벤트 핸들러 프로퍼티 방식으로 이벤트 핸들러 등록
const clickEvent = () => console.log("I'm clicked!");
    $button.onclick = clickEvent;

    $button.removeEventListener('click', clickEvent); // 제거 실패

    $button.onclick = null; // 제거 성공
```
## 이벤트 객체

**클릭 이벤트로 생성된 이벤트 객체는 이벤트 핸들러의 첫번째 인수로 암묵적 전달된다.**
그래서 이벤트 객체를 전달받으려면 이벤트 핸들러를 선언할 때, 이벤트 객체를 전달받을 `첫번째 매개변수를 명시적으로` 선언하여야 한다.
```javascript
<!DOCTYPE html>
<html>
<head>
    <style>
    html, body {height: 100%;} 
    </style>
</head>
// 이벤트 핸들러 어트리뷰트 방식의 경우 매개변수명이 event가 아니면 이벤트 객체를 전달받지 못한다
<body onclick="showCoords(event)">
    <p>클릭 시 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
<script>
    const $msg = document.querySelector(".message");
    
    // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫번째 인수로 전달된다
    function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
    }
</script>
</body>
</html>
```

이벤트 핸들러 어트리뷰트 값은 암묵적 생성되는 이벤트 핸들러의 함수 몸체를 의미하기 때문에, **이벤트 핸들러 어트리뷰트 방식에서 이벤트 객체를 전달받으려면** 반드시 핸들러의 `첫번째 매개변수명은 event`여야 한다.

```javascript
function onclick(event) {
    showCoords(event);
}
```
### 상속 구조
![image-3](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/3304efce-f88c-474e-9e30-32511c393669)

이벤트가 발생하면 이벤트 타입에 해당하는 이벤트 객체가 생성된다.

### 이벤트 객체의 공통 프로퍼티

|공통 프로퍼티|설명|타입|
|---|---|---|
|type|이벤트 타입|string|
|target|이벤트를 발생시킨 DOM 요소|DOM 요소 노드|
|currentTarget|이벤트 핸들러가 바인딩된 DOM 요소|DOM 요소 노드|
|eventPhase|이벤트 전파 단계 (0: 이벤트 X 1: 캡처링 단계 2: 타겟 단계 3: 버블링 단계)|number|
|bubbles|이벤트를 버블링으로 전파하는지 여부. (e.g., focusEvent: focus/ blur, resourceEvent: load/unload/abort/error, mouseEvent: mouseenter/mouseleave `=> false`)|boolean|
|cancelable|preventDefault 메서드를 호출해서 이벤트의 기본 동작을 취소할 수 있는지 여부. (e.g., focusEvent: focus/ blur, resourceEvent: load/unload/abort/error, mouseEvent: **dbclick**/mouseenter/mouseleave `=> false`)|boolean|
|defaultPrevented|preventDefault 메서드를 호출해서 이벤트를 취소했는지 여부|boolean|
|isTrusted|사용자의 행위에 의해 발생한 이벤트인지 여부 (e.g., click이나 dispatchEvent로 인위적으로 발생시킨 이벤트인 경우 `=> false`)|boolean|
|timeStamp|이벤트가 발생한 시각|number|


## DOM 요소의 기본 동작 조작

### preventDefault : 기본 동작 조작 중단
```javascript
<!DOCTYPE html>
<html>
<body>
    <a href="https://www.google.com">google</a>
    <input type="checkbox">
<script>
    document.querySelector('a').onclick = e => {
        e.preventDefault(); // a의 기본 동작 중단
    };

    document.querySelector('input[type=checkbox]').onclick = e => {
        e.preventDefault(); // input type="checkbox"의 기본 동작 중단
    };
</script>
</body>
</html>
```
### stopPropagation : 이벤트 전파 방지
```javascript
<!DOCTYPE html>
<html>
<body>
    <div class="container">
    <button class="btn1">btn1</button>
    <button class="btn2">btn2</button>
    <button class="btn3">btn3</button>
    </div>
<script>
    document.querySelector('.container').onclick = ({target}) => {
        if(!target.matches('.container > button')) return;
        target.style.color = "pink";
        };

    document.querySelector('.btn2').onclick = (e) => {
        e.stopPropagation(); // 이벤트 전파 중단
        e.target.style.color = "green";
        };
</script>
</body>
</html>
```

## 이벤트 핸들러에 인수 전달

이벤트 핸들러 어트리뷰트 방식과 다르게 이벤트 핸들러 프로퍼티/addEventListener 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 자체를 등록하기에 인수를 전달할 수 없으나, 핸들러 내부에서 함수를 호출하면서 인수를 전달하는 방법이 있다.

```javascript
<!DOCTYPE html>
<html>
<body>
    <label>User Name 
        <input type="text"></label> 
    <em class="message"></em>
<script>
    const MIN_USER_NAME_LENGTH = 5;
    const $input = document.querySelector('input[type=text]');
    const $msg = document.querySelector('.message');

    const checkUserNameLength = (min) => {
        $msg.textContent = $input.value.length < min ? `Write username up to ${min} characters.` : "";
    }

    $input.onblur = () => {
        checkUserNameLength(MIN_USER_NAME_LENGTH);
    } 
</script>
</body>
</html>
```
아니면 이런 식으로 핸들러를 반환하는 함수를 호출하면서 인수를 전달할 수 있다. 
```javascript
const MIN_USER_NAME_LENGTH = 5;
const $input = document.querySelector('input[type=text]');
const $msg = document.querySelector('.message');

const checkUserNameLength = (min) => e => {
    $msg.textContent = $input.value.length < min ? `Write username up to ${min} characters.` : "";
    }

$input.onblur = checkUserNameLength(MIN_USER_NAME_LENGTH);
```

## 커스텀 이벤트
Event, UIEvent, MouseEvent 같은 생성자 함수를 호출해서 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.
- 버블링되지 않으며 preventDefault로 중단할 수도 없다.
    - bubbles와 cancelable: false가 default
    - true로 설정하려면 이벤트 생성자 함수의 두번째 인수로 bubbles나 cancelable를 갖는 객체를 전달
- 이벤트 객체 고유의 프로퍼티 값을 지정할 수 있는데 생성자 함수의 두번째 인수로 전달하면 된다.
- isTrusted가 항상 false
    - 사용자의 행위에 의해 발생한 이벤트는 항상 true

### dispatchEvent
- dispatchEvent 메서드에 인수로 이벤트 객체를 전달하면서 호출하면 **인수로 전달한 이벤트 타입의 이벤트가 발생**한다.
- 커스텀 이벤트 생성자 함수의 **두번째 인수**로 detail 프로퍼티를 포함하는 객체를 전달할 수 있다.
- `반드시 addEventListener 방식`으로 이벤트를 등록해야한다. 
    - 'on+이벤트 타입'로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 없기 때문
- 일반적 이벤트 핸들러는 **비동기 처리 방식**으로 동작하지만 dispatchEvent는 **동기 처리 방식**으로 호출한다. 
    - dispatchEvent 호출 전에 이벤트 핸들러를 등록해야 한다.
```javascript
<!DOCTYPE html>
<html>
<body>
<button class="btn">click</button>
<script>
    const $btn = document.querySelector('.btn');

// btn 요소에 foo 커스텀 이벤트 핸들러를 등록. 디스패치 이전에 이벤트 핸들러를 등록해야 한다.
    $btn.addEventListener('foo', (e) => {
        alert(e.detail.message);
    });

// CustomEvent 생성자 함수로 foo 이벤트 객체 생성  
    const customEvent = new CustomEvent('foo', {
        detail: { message: 'Hello' }
    });

// 커스텀 이벤트 디스패치. foo 이벤트가 발생한다.
    $btn.dispatchEvent(customEvent); // Hello
</script>
</body>
</html>
```