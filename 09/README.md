# var, let, const 키워드와 블록 레벨 스코프
## var
### 변수 중복 선언 허용
- 같은 스코프 내에서의 변수 중복 선언이 가능하다. 
```javascript
var x = 1;
var y = 2;

// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 12;
// 초기화문 없이 작성된 변수 선언문은 그대로 무시
var y;

console.log(x); // 12
console.log(y); // 2
```
### 함수 레벨 스코프
- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.
- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 `전역 변수`가 된다.
```javascript
var x = 1;

if(true) {
    var x = 10;
}

console.log(x); // 10
```
- for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역변수가 된다.
```javascript
var x = 10;

for(var x = 0; x < 5; x++;){
    console.log(x); // 0 1 2 3 4
}
console.log(x); // 5
```
### 변수 호이스팅
- var 키워드로 변수를 할당하면 `변수 호이스팅`으로 실행 단계의 선두로 끌어올려진 것처럼 동작한다. 
- 이로 인해 var 키워드로 선언된 변수는 `변수 선언문 이전에 참조할 수 있지만` 이는 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.
```javascript
// 변수 foo의 변수 선언 : 1. 선언 단계
// 변수 foo의 값 초기화(undefined) : 2. 초기화 단계
console.log(foo); // undefined

// 변수 foo에 값 할당 : 3. 할당 단계
foo = 100;

console.log(foo); // 100

// 변수 호이스팅으로 인해 런타임 이전에 암묵적으로 선언과 초기화가 이루어진다.
var foo;
```

## let
### 변수 중복 선언 금지
- let과 const 키워드는 `같은 스코프 내에서`의 `중복 선언을 허용하지 않는다.`
```javascript
var x = 1;
var x = 5;
console.log(x); // 5

let y = 10;
let y = 100; 
console.log(y); // SyntaxError: Identifier 'y' has a already been declared.
```
### 블록 레벨 스코프
- let 키워드로 선언한 변수는 `모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프`를 따른다. (함수, if문, for문, while문, try/catch문 등)
```javascript
let x = 1; // 전역 변수

 {
    let x = 10; // 지역 변수
    let y = 5; // 지역 변수
}

console.log(x); // 1
console.log(y); // ReferenceError: y is not defined.
```
- 함수 내의 코드 블록은 함수 레벨 스코프에 중첩된다. 
![scope](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/b6312a7c-a36c-4904-84bd-cc7cf791fb44)

### 변수 호이스팅
- 변수 호이스팅이 발생하지 않는 것처럼 동작한다. 
- var과 달리 변수 선언 단계와 초기화 단계가 다른 시점에 이루어지기 때문에 `변수 선언문 전에 참조가 불가능`하다.
```javascript
// 런타임 이전에 1. 선언 단계가 실행.
console.log(foo); // ReferenceError: foo is not defined.

let foo; // 변수 선언문에서 2. 초기화 단계가 실행

console.log(foo); // undefined

foo = 1; // 할당문에서 3. 할당 단계가 실행

console.log(foo); // 1
```
 선언 단계와 초기화 단계 사이의 일시적 사각지대(Temporal Dead Zone)에서는 변수를 참조할 수 없다.

![letvariable](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/6a46ed49-1c95-4e99-9fc7-189853b58168)



그리고 변수 호이스팅이 발생하지 않는 것처럼 보인다고 했는데 사실은 그렇지 않다. 
```javascript
let x = 1; // 전역 변수
{
    console.log(x); // ReferenceError: Cannot access 'x' before initialization
    let x = 2; // 지역 변수
}
```
변수 호이스팅이 발생하지 않는다면 전역 변수 x의 값을 출력해야 하는데, `여전히 호이스팅이 발생`하고 있기 때문에 참조 에러가 발생하고 있다.

### 전역 객체와 let
- var 키워드로 선언한 전역 변수와 전역 함수, 암묵적 전역은 전역 객체 window의 프로퍼티가 된다.
```javascript
var x = 1; // 전역 변수

y = 3; // 암묵적 전역

function foo() {} // 전역 함수

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 3
console.log(y); // 3

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo(){}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // f foo(){}
```

- let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다. 
```javascript
let x = 1;

console.log(window.x); // undefined
console.log(x); // 1
```
## const 
let과 대부분 동일한 특징을 가지고 있지만 다른 점을 중심으로 하단에 작성했다.
### 선언과 동시에 초기화
- const 키워드로 선언한 변수는 선언과 `동시에` 초기화해야한다.
```javascript
const x; // SyntaxError: Missing initializer in const declaration

const x = 1;
```
- let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
### 무조건 재할당 금지 ?
- const 키워드로 선언한 변수에 `원시 값을 할당한 경우` 변수 값을 변경할 수 `없다`.

 원시 값은 변경이 불가능한 값이므로 'const 키워드에 원시값을 할당한다' = '상수를 선언' 하는 것으로 이해가 가능하다.
```javascript
const x = 1;

x = 13; // TypeError: Assignment to constant variable.
```
- const 키워드로 선언한 변수에 `객체를 할당한 경우` 변수 값을 변경할 수  `있다`.

객체는 재할당 없이 직접 변경이 가능하기 때문에 객체가 변경되더라도 변수에 할당된 참조값은 변경되지 않는다.
```javascript
const person = {
    name: 'Yujin'
};

person.name = 'Jamin';

console.log(person); // {name: 'Jamin'};
```