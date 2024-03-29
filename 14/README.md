# 빌트인 객체
자바스크립트 객체의 분류
- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 표준 빌트인 객체
- Math, Reflext, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다. 
    - **생성자 함수인 표준 빌트인 객체**: 프로토타입 메서드와 정적 메서드를 제공
    - **생성자 함수가 아닌 표준 빌트인 객체**: 정적 메서드만 제공

- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토 타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

## 원시값과 래퍼 객체
원시값은 객체가 아니라서 프로퍼티나 메서드를 가질 수 없는데도 원시값이 객체처럼 동작한다.

```javascript
const str = 'string';

console.log(str.length); // 6
console.log(str.toUpperCase()); // STRING

console.log(typeof str); // string
```

문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 그 순간 자바스크립트엔진이 일시적-암묵적으로 연관된 **객체를 생성**해서 접근, 호출 후 다시 원시값으로 되돌리기 때문에 객체처럼 사용할 수 있다. 이를 래퍼 객체(wrapper object)라고 한다. 

래퍼 객체는 표준 빌트인 객체의 프로토타입 프로퍼티/메서드를 참조할 수 있기 때문에 String, Number, Boolean 생성자 함수로 인스턴스를 생성할 필요가 없다. 

ES6부터 심벌 값도 래퍼 객체를 생성하지만 그 방식에 차이가 있다. 
이외의 원시값(null, undefined)은 래퍼 객체를 생성하지 않고 에러를 반환한다.


## 전역 객체
- 런타임 이전에 자바스크립트 엔진에 의해 가장 먼저 생성된다.
- 자바스크립트 환경에 따라 지칭하는 이름이 다르다. 
    - 브라우저 환경 -> window
    - Node 환경 -> global
- 개발자가 의도적으로 생성할 수 없다. 
- 최상위 객체이다. 
- 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 전역 함수는 전역 객체의 프로퍼티가 된다.
```javascript
function baz() {
    return 3;
}
console.log(window.baz()); // 3
```
- 선언하지 않은 변수에 값을 할당하면 암묵적으로 전역화된다.
```javascript
bar = 2;
console.log(window.bar); // 2
```
- var과 다르게 let, const로 선언한 전역 변수는  전역 객체의 프로퍼티가 아니다. 보이지 않는 개념적인 블록 안에 있다.

```javascript
var a = 1;
let b = 2;
const c = 3;

console.log(window.a); // 1
console.log(window.b); // undefined
console.log(window.c); // undefined
```
- 참조할 때 window(혹은 global)을 생략할 수 있다.
### 빌트인 전역 프로퍼티
전역 객체의 프로퍼티를 의미한다.
- Infinity
- NaN
- undefined
### 빌트인 전역 메서드
전역 객체의 메서드를 의미한다. 
- isNaN과 같이 전달받은 인수를 검사해서 그 결과를 반환하는 메서드가 있다.
- parseFloat과 같이 전달받은 인수를 변환해서 반환하는 메서드가 있다.
- encodeURI / decodeURI
    - encodeURI: 완전한 URI를 인수로 받아서 이스케이프 처리를 위해 인코딩, 
        - 쿼리스트링 구분자는 인코딩 하지 않음
    - decodeURI: 인코딩된 URI를 인수로 받아서 이스케이프 처리 이전으로 디코딩
- encodeURIComponent / decodeURIComponent
    - URI 구성 요소를 인수로 받아서 인코딩한다.
        - encodeURI와 다르게 쿼리스트링 구분자까지 인코딩

![URIComponent](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/72acbaa5-c6e0-490c-8912-19a30fca488c)


## 암묵적 전역
선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문에 전역 변수처럼 동작한다. 

foo 함수 내의 y는 변수 선언 없이 전역 객체의 프로퍼티로 추가되었을 뿐이라서 변수 호이스팅이 발생하지 않는다. 
```javascript
console.log(x); // undefined : 변수 x는 호이스팅되어서 선언 후 초기화된 상태
console.log(y); // ReferenceError: y is not defined

var x = 7;

function foo(){
    // 선언하지 않은 식별자에 값을 할당
    y = 111;  // window.y = 111;
    // 변수가 아니라 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작하는 것.
}
foo();

console.log(x * y); // 777

delete x;
delete y;

console.log(x); // 7
console.log(y); // ReferenceError: y is not defined
```

그리고 변수가 아니라 프로퍼티라서 y는 delete 연산자로 삭제할 수 있다. 
