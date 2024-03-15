# 생성자 함수에 의한 객체 생성

생성자 함수란 new 연산자와 같이 호출해서 객체를 생성하는 함수를 말한다. 이렇게 `생성자 함수에 의해 생성된 객체`를 `인스턴스`라고 한다.

이외에도 객체 리터럴로 객체를 생성하는 방식이 있지만, 객체 리터럴은 작성하기는 간편하지만 프로퍼티 구조가 동일함에도 불구하고 객체가 필요할 때마다 모두 작성해야한다는 불편함이 있다.

## object / built-in 생성자 함수

new 연산자에 `Object 생성자 함수`를 호출하면 빈 객체를 생성해서 반환한다.
그리고 그 빈 객체에 프로퍼티나 메서드를 할당해서 객체를 완성한다.

```javascript
// 1. 빈 객체 생성
const user = new Object();

// 2. 프로퍼티 추가
user.name = "Lee";

console.log(user); // {name: 'Lee'}
```

Object 생성자 함수 말고도 `빌트인(Built-in) 생성자 함수`를 제공한다.

```javascript
// String 생성자 함수, 객체 생성
const strObj = new String("Lee");
console.log(typeof strObj); // object
console.log(strObj); // String {'Lee'}

// Number 생성자 함수, 객체 생성
const numObj = new Number(526);
console.log(typeof numObj); // object
console.log(numObj); // Number {526}

// Boolean 생성자 함수, 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj); // Boolean {true}

// Function 생성자 함수, 객체(함수) 생성
const funcObj = new Function("x", "return x * x");
console.log(typeof funcObj); // object
console.dir(funcObj); // Function (<anonymous>)

// Array 생성자 함수, 객체(배열) 생성
const arrObj = new Array(1, 2, 3);
console.log(typeof arrObj); // object
console.log(arrObj); // (3) [1, 2, 3]

// RegExp 생성자 함수, 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp); // /ab+c/i

// Date 생성자 함수, 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date); // Wed Mar 13 2024 15:38:13 GMT+0900 (한국 표준시)
```

## 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할은 `프로퍼티 구조가 동일한 인스턴스`를 생성하기 위한 템플릿처럼 동작해서

-   인스턴스를 생성하는 것. `(필수)`

-   인스턴스를 초기화-인스턴스 프로퍼티 추가 및 초기값 할당-하는 것. `(옵션)`

### 인스턴스 생성 과정

1. 생성자 함수 작성

2. 런타임 이전 -> 암묵적으로 생성된 빈 객체(인스턴스)가 this에 바인딩된다.

3. 런타임 -> this에 바인딩된 인스턴스에 프로퍼티나 메서드를 추가하고 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당해서 초기화하거나 고정값을 할당한다.
4. 완성된 인스턴스가 바인딩된 this가 암묵적 반환된다.

```javascript
// 1. 생성자 함수
function Circle(radius) {
    // 2. (런타임 이전) 암묵적으로 인스턴스가 생성되고 this binding
    console.log(this); // Circle {}

    // 3. (런타임) this binding된 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function () {
        return this.radius * 2;
    };
    // 4. 암묵적인 this 반환
}
```

4번 단계에서 this가 아닌 객체를 명시적으로 반환하면 이 암묵적인 this 반환이 무시되기 때문에 `생성자 함수 내부에서 return문을 반드시 생략`해야 한다.

```javascript
// 인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체 생성

console.log(circle1); // Circle { radius: 5, getDiameter: f }

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

만약 이때 new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.

```javascript
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
const circle3 = Circle(15);

console.log(circle3); // undefined

// 일반 함수로 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### :bookmark: this

자기 참조 변수이며, this가 가리키는 값은 `함수 호출 방식에 따라` 동적으로 결정된다. 키워드로 분류되지만 식별자 역할을 수행한다.

#### `name binding?`

    식별자와 값을 연결하는 과정을 의미한다.

예를 들면 변수 선언은 변수 이름(식별자)와 메모리 공간의 주소를 바인딩하는 것으로 설명할 수 있다.

따라서 this 바인딩 => this와 this가 가리킬 객체를 연결하는 것을 의미한다.

| 함수 호출 방식 | this binding                                                                     |
| -------------- | -------------------------------------------------------------------------------- |
| 일반 함수      | 기본적으로 `전역 객체` (`Node` 환경 -> `global` 객체, `browser`-> `window` 객체) |
| 메서드         | 메서드를 호출한 객체 (마침표 앞의 객체)                                          |
| 생성자 함수    | 생성자 함수가-미래에-생성할 인스턴스                                             |

```javascript
function foo() {
    console.log(this);
}

// 일반 함수, browser 환경일 때
foo(); // window

// 일반 함수, Node.js 환경일 때
foo(); // global

// 메서드
const obj = { foo };
obj.foo(); // obj

// 생성자 함수
const inst = new foo(); // inst
```

## 내부 메서드 [[Call]]과 [[Construct]]

![call](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/1f1c9027-7b4f-4217-8578-a65c7af38706)

모든 함수 객체는 callable이지만 모든 함수 객체가 constructor인 것은 아니다.

### constructor와 non-constructor

-   constructor: 함수 선언문, 함수 표현식, 클래스
-   non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

```javascript
// constructor
// 일반 함수 정의: 함수 선언문, 함수 표현식
    function foo(){};
    const bar = function() {};

new foo(); // foo {}
new bar(); // bar {}

// 프로퍼티 x값으로 할당된 게 일반 함수로 정의된 함수라서 메서드로 인정하지 않는다.
const notMethod = {
        x: function(){}
    }

new notMethod(); // TypeError: notMethod is not a constructor
new notMethod.x(); // x {}

// non-constructor
// 화살표 함수
const arrowFunc = () => {};
new arrowFunc(); // TypeError: arrowFunc is not a constructor

// ES6 메서드 축약 표현으로 정의된 메서드만 메서드로 인정
const obj = {
    x(){}
};
new obj.x(); // TypeError: obj.x is not a constructor
```
## new 연산자
생성자 함수로서 호출될 것이라 기대하지 않고 작성한 일반 함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있음을 주의해야한다.

```javascript
// 생성자 함수로서 호출될 것이라 기대하지 않고 작성한 일반 함수
function add(x, y){
    return x + y;
}

let inst = new add();
// 함수가 객체를 반환하지 않았으므로 반환문 무시, 빈 객체만 생성되어 반환됨.
console.log(inst); // {}
```

new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.
```javascript
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return this.radius * 2;
    };
}
const circle = Circle(5);

// 브라우저 기준 일반 함수 내부의 this는 전역 객체 window를 가리킴
console.log(circle); // undefined
console.log(radius); // 5
console.log(getDiameter(()); // 10

console.log(circle.getDiameter()); // TypeError: Cannot read properties of undefined (reading 'getDiameter')
```

### new.target
new 연산자와 `함께` 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 `없이` 일반 함수로서 호출되면 함수 내부의 new.target은 undefined이다.

new 연산자없이 호출되었는지 확인하고, new 연산자가 없다면 new 연산자와 함께 재귀 호출해서 생성자 함수로서 호출하는 방식을 사용할 수 있다.
```javascript
// 생성자 함수
function Circle(radius){
// 함수가 호출되었을 때 new 연산자없이 호출되었다면 new.target은 undefined이다
    if(!new.target){
// 그 경우 new 연산자와 함께 생성자 함수를 재귀 호출해서 생성된 인스턴스를 반환하도록 함
        return new Circle(radius);
    }
    this.radius = radius;
    this.getDiameter = function () {
        return this.radius * 2;
    };
}
const circle = Circle(5);

// new.target을 사용해서 new 연산자 없이도 생성자 함수로서 호출된다
console.log(circle.getDiameter()); // 10
```

### Scope-safe Constructor Pattern
new.target이 ES6에서 도입되었고 IE에서는 지원하지 않기 때문에 new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자 패턴을 사용할 수 있다.

```javascript
function Circle(radius){
// - 함수가 호출되었을 때 new 연산자와 함께 호출되었다면 빈 객체를 생성하고 this에 바인딩된다.

// - 함수가 호출되었을 때 new 연산자없이 호출되었다면 this는 전역 객체 window를 가리키기 때문에 this와 Circle이 바인딩되지 않는다.
    if(!this instanceof Circle){
        return new Circle(radius);
    }
    this.radius = radius;
    this.getDiameter = function () {
        return this.radius * 2;
    };
}
const circle = Circle(5);

// new 연산자 없이도 생성자 함수로서 호출된다
console.log(circle.getDiameter()); // 10
```