# 일급 객체
- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
- 변수나 자료구조(객체, 배열)에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다

함수는 일급 객체이다. = 함수를 객체와 동일하게 사용할 수 있다는 의미.

### 일반 객체와 함수 객체의 차이점
| - |일반 객체|함수 객체|
|---|:---:|:---:|
|고유의 프로퍼티를 소유했는가? |X|O|
|호출 가능한가?|X|O|

## 함수 객체의 프로퍼티
- arguments 프로퍼티
- caller 프로퍼티
- length 프로퍼티
- name 프로퍼티
- __ proto __ 접근자 프로퍼티
- prototype 프로퍼티
```javascript
function square(number){
    return number + number;
}
console.dir(square);
```
console.dir를 통해 square의 프로퍼티를 읽어보면 이렇게 나온다.

<img width="339" alt="square property" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/b8a896ce-f9a9-416f-a207-fa989cd006c0">

이 프로퍼티들의 프로퍼티 어트리뷰트를 들여다보기 위해 Object.getOwnPropertyDescriptors 메서드로 확인한다.

```javascript
function square(number){
    return number + number;
}
console.log(Object.getOwnPropertyDescriptors(square));
```
<img width="578" alt="square property attribute" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/b1535e08-9221-423b-bc04-3b5a501293b1">

### arguments 프로퍼티
arguments 프로퍼티 값은 argument 객체다. arguements 객체는 `유사 배열 객체`이며, 함수 내부에서 `지역 변수`처럼 사용된다. 

#### 유사 배열 객체?
e.g., NodeList나 arguments를 말한다.

유사 객체 배열이라고 하는 이유는 arguments가 length 속성과 더불어 0부터 인덱스된 다른 속성을 가지고 있지만, Array의 forEach, map과 같은 내장 메서드를 가지고 있지 않기 때문이다.
```javascript
// indexing

function func1(a, b, c) {
  console.log(arguments[0]); // 1

  console.log(arguments[1]); // 2

  console.log(arguments[2]); // 3
}

func1(1, 2, 3);
```
---
대신 arguments 객체를 Array로 만들어서 Array의 내장 메서드를 사용할 수 있도록 하는 것은 가능하다.
- Array.from()

    + 순회 가능 또는 유사 배열 객체에서 얕게 복사된 새로운 Array 인스턴스를 생성
```javascript
let args = Array.from(arguments); 
```
- ES6 Rest parameter [...]

    + 복사 기능으로 배열을 만든다.
```javascript
let args = [...arguments]; 
```
- Array.prototype.slice.call()

    + call()을 사용하여 slice()에게 arguments를 인자로 넘기고, 전달받은 slice()는 최종적으로 배열을 만든다. 
```javascript
var args = Array.prototype.slice.call(arguments);
```
```javascript
var args = [].slice.call(arguments);
```
---

#### arguements 객체의 프로퍼티
자바스크립트는 매개변수와 인수의 갯수가 일치하는지 확인하지 않는다. 때문에 함수가 호출되었을 때 인수의 갯수를 확인하고 이에 따라 함수의 동작를 다르게 정의할 필요가 있을 때(`가변 인자 함수`를 구현할 때) arguments 객체가 사용된다.

선언한 매개변수의 갯수보다 `인수를 적게 전달할 경우` 매개변수는 undefined로 초기화된 상태를 유지한다.
반대로 `인수를 더 전달할 경우`에는 초과된 인수를 무시한다. 그리고 무시된 인수를 포함해서 모든 인수는 arguements 객체의 프로퍼티로 보관된다.

<img width="446" alt="argument" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/c2167823-f8c9-45b5-9bb1-20cc08468659">

```javascript
multiply(); // 인수가 없으니 인수 정보 X

multiply(1); // 0: 1

multiply(1, 2); // 0: 1
                // 1: 2
```
이때, 매개변수의 갯수보다 `인수를 더 전달`할 경우.

<img width="505" alt="more arguments" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/ac9120ed-4cfe-4db9-9611-0d42ac0ab2e1">

```javascript
multiply(1, 2, 3);  // 0: 1
                    // 1: 2
                    // 2: 3
```
arguments 객체의 `callee` 프로퍼티는 호출되어서 arguments 객체를 생성한 함수 자신을 가리키고 `length` 프로퍼티는 인수의 개수를 나타냄을 알 수 있다.

`Symbol(Symbol.iterator)`는 arguments 객체를 순회 가능한 `이터러블`로 만들기 위한 프로퍼티인데 이건 뒤의 이터러블에서 더 알아보기.

### caller 프로퍼티
ECMAScript 사양에 포함되지 않은 비표준 프로퍼티.

### length 프로퍼티
함수를 정의할 때 선언한 매개변수의 개수를 가리킨다. 
```javascript
function foo(){}
console.log(foo1.length); // 0

function foo2(x){
    return x;
}
console.log(foo2.length); // 1

function foo3(x, y){
    return x * y;
}
console.log(foo3.length); // 2
```
### name 프로퍼티
ES6에서 표준이 된 프로퍼티. 함수 이름을 나타낸다.

`주의!` 익명 함수 표현식의 경우 ES5와 ES6에서 다르게 동작한다.
```javascript
let anonymousFunc = function(){};
console.log(anonymousFunc.name); // ES5: "", 빈 문자열을 값으로 갖는다.
                            // ES6: anonymousFunc
```
기명 함수 표현식이나 함수 선언문에서는 함수 객체를 가리키는 식별자를 값로 갖는다.
```javascript
// 기명 함수 표현식
let namedFunc = function foo(){};
console.log(namedFunc.name); // foo

// 함수 선언문
function bar(){};
console.log(bar.name); // bar
```

### __ proto __ 접근자 프로퍼티
[[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티.

`문서 10`에서 설명함!

객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
```javascript
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype); // true
```
객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
```javascript
console.log(obj.hasOwnProperty('a'));         // true
console.log(obj.hasOwnProperty('__proto__')); // false
```
 + hasOwnProperty 메서드는 Object.prototype의 메서드다. 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키일 경우에만 true를, 상속받은 프로토 타입의 프로퍼티 키일 경우 false를 반환한다.
### prototype 프로퍼티
생성자 함수로 호출할 수 있는 함수 객체`(constructor)`만 소유하는 프로퍼티다. 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

```javascript
// 함수 객체는 prototype 프로퍼티가 있음
(function () {}).hasOwnProperty('prototype'); // true

// 일반 객체는 prototype 프로퍼티가 없음
({}).hasOwnProperty('prototype'); // false
```