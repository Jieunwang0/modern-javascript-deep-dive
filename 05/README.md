# 타입 변환과 단축 평가

## 암묵적 타입 변환 (타입 강제 변환)

개발자의 의도와 상관없이 코드 문맥을 고려해 자바스크립트 엔진에 의해 타입이 암묵적으로 자동 변환된다.

```javascript
// 템플릿 리터럴의 표현식 삽입은 문자열 타입으로 암묵적 타입 변환한다
`1 + 1 = ${1 + 1}`; // '1 + 1 = 2'
```

### 문자열 타입으로 변환

```javascript
// number type
0 + '' // '0'
-0 + '' // '0'
1 + '' // '1'
-1 + '' // '-1'
NaN + '' // 'NaN'
Infinity + '' // 'Infinify'
-Infinity + '' // '-Infinify'

// boolean type
true + '' // 'true'
false + '' // 'false'

// null type
null + '' // 'null'

// undefined type
undefined + '' // 'undefined'

// Symbol type
(Symbol()) + '' // 'Symbol'

// Object type
({}) + '' // ''
Math + '' // 'Math'
[] + '' // ''
[10, 20] + '' // '10, 20'
(function(){}) + '' // 'function(){}'
Array + '' // 'function Array(){[native code]}'

```

### 숫자 타입으로 변환

```javascript
// string
1 - "1"; // 0
1 * "10"; // 10
1 / "one" + // NaN
"" + // 0
"0" + // 0
"string" + // NaN, 위의 1 / 'one'과 같은 상황

// boolean
+ true; // 1
+ false; // 0

// null
+ null // 0

// undefined
undefined + // 0

// Symbol
Symbol() + // TypeError: Cannot convert a Symbol value to a number

// Object
{} + // NaN
[] + // 0, 빈 배열인 배열은 0으로 변환된다
[10, 20] + // NaN
function () {}; // NaN
```

### 불리언 타입으로 변환

자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 / Falsy 값으로 구분한다. 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy -> true로, Falsy -> false로 암묵적 타입 변환한다.

```javascript
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

아래는 false로 평가되는 Falsy 값이다.

-   false
-   null
-   undefined
-   0, -0
-   NaN
-   '' (빈 문자열)

Falsy 값 이외의 모든 값들은 true로 평가되는 Truthy 값이다.

## 명시적 타입 변환

개발자의 의도에 따라 명시적으로 타입을 변경하는 방법이다.

### 문자열 타입으로 변환

-   String 생성자 함수를 new 연산자 없이 호출
-   Object.prototype.toString() 메서드 사용
-   문자열 연결 연산자를 사용하는 방법

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출

// number -> string
String(1); // '1'
String(NaN); // 'NaN'
String(Infinify); // 'Infinity'

// boolean -> string
String(true); // 'true'
String(false); // 'false'
```
```javascript
// 2.Object.prototype.toString() 메서드 사용

// number -> string
(1).toString(); // '1'
NaN.toString(); // 'NaN'
Infinity.toString(); // 'Infinity'

// boolean -> string
true.toString(); // 'true'
false.toString(); // 'false'
````
```javascript
// 3. 문자열 연결 연산자를 사용

// number -> string
1 + ""; // '1'
NaN + ""; // 'NaN'
Infinity + ""; // 'Infinity'

// booelan -> string
true + ""; // 'true'
false + ""; // 'false'
```

### 숫자 타입으로 변환

-   Number 생성자 함수를 new 연산자 없이 호출
-   parseInt, parseFloat 함수를 사용하는 방법 (문자열에만 사용 가능)
-   ' + ' 단항 산술 연산자를 사용
-   ' \* ' 단항 산술 연산자를 사용

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출

// string -> number
Number("0"); // 0
Number("-1"); // -1
Number("10.52"); // 10.52

// boolean -> number
Number(true); // 1
Number(false); // 0
````
```javascript
// 2. parseInt, parseFloat 함수를 사용하는 방법 (문자열에만 사용 가능)

// string -> number
parseInt("0"); // 0
parseInt("-1"); // -1
parseInt("10.53"); // 10.53
````
```javascript
// 3. + 단항 산술 연산자를 사용

// string -> number
+ "0"; // 0
+ "-1"; // -1
+ "10.34"; // 10.34

// boolean -> number
+ true; // 1
+ false; // 0
```
```javascript
// 4. * 단항 산술 연산자를 사용

// string -> number
1 * "2"; // 2
1 * "-4"; // -4
1 * "0.25"; // 0.25

// boolean -> number
1 * true; // 1
1 * false; // -1
```

### 불리언 타입으로 변환

-   Boolean 생성자 함수를 new 연산자 없이 호출
-   ! 부정 논리 연산자를 두 번 사용 (!!)

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출

// string -> boolean
Boolean("x"); // true
Boolean(""); // false
Boolean("false"); // true

// number -> booelan
Boolean(20); // true
Boolean(0); // false
Boolean(Infinity); // true
Boolean(NaN); // false

// null -> boolean
Boolean(null); // false

// undefined -> boolean
Boolean(undefined); // false

// Object -> boolean
Boolean({}); // true
Boolean([]); // true
````
```javascript
//  2. ! 부정 논리 연산자를 두 번 사용

// string -> boolean
!!"x"; // true
!!""; // false
!!"false"; // true

// number -> boolean
!!0; // false
!!-20; // true
!!10; // true
!!NaN; // false
!!Infinity; // true

// null -> boolean
!!null; // false

// undefined -> boolean
!!undefined; // false

// Object -> boolean
!!{}; // true
!![]; // true
```

## 단축 평가

표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

### 논리 연산자 (논리합(||), 논리곱(&&))

-   논리합(||)
    -   a || b 는 a 나 b `둘 중에 하나만 true여도 true`
-   논리곱(&&)
    -   a && b 는 a와 b `둘 다 true여야 true`

### 논리 연산자를 사용한 단축 평가

| 단축 평가 표현식    | -   | 평가 결과 |
| ------------------- | --- | --------- |
| true ㅣㅣ anything  | -   | true      |
| false ㅣㅣ anything | -   | anything  |
| true && anything    | -   | anything  |
| false && anything   | -   | false     |

-   논리합(||), 논리곱(&&) 연산자는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는데, 이를 `단축 평가`라고 한다.

```javascript
// 논리합(||)
"Cat" || "Dog"; // 'Cat'
false || "Dog"; // 'Dog'
"Cat" || false; // 'Cat'

// 논리곱(&&)
"Cat" && "Dog"; // 'Dog'
false && "Dog"; // false
"Cat" && false; // false
```

-   논리값이 필요한 장소에서는 논리값으로 타입이 변환되어서 논리식 자체는 true or false로 정상 동작한다.

### if 문 대체

```javascript
// Falsy 값이 조건일 떄
var done = false;
var message = "";

// if 문
if (!done) message = "미완료";

// 논리합(||)
message = done || "미완료";
console.log(message); // 미완료



// Truthy 값이 조건일 때
var done = true;
var message = "";

// if 문
if (done) message = "완료";

// 논리곱(&&)
message = done && "완료";
console.log(message); // 완료
```

```javascript
// 삼항 조건 연산자로 if..else 문 대체하는 예시
var done = true;
var message = "";

// if..else 문의 경우
if (done) {
    message = "완료";
} else {
    message = "미완료";
}
// 삼항 조건 연산자의 경우
message = done ? "완료" : "미완료";
console.log(message); // 완료
```

### 객체를 가리키기를 기대하는 변수가 null 혹은 undefined가 아닌지 확인하고 프로퍼티를 참조할 때
```javascript
var elem = null;

// var value = elem.value;  
// <= TypeError!! cannot read property 'value' of null

var value = elem && elem.value;
// elem이 null/undefined면 falsy값이므로 elem으로 평가되고 truthy 값이면 elem.value 값으로 평가된다. 여기서는 elem이 null이므로 var value = elem 이 된다. 
```

### 함수 매개변수에 기본값을 설정할 때
함수를 호출할 때 인수를 설정하지 않으면 자바스크립트엔진이 자동으로 undefined를 할당한다. 이때 기본값을 설정해주면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.
```javascript
// 단축평가를 이용한 기본값 설정
function getStringLength(str){
    str = str || '';
    return str.length();
}
getStringLength(); // 0
getStringLength('heyy'); // 4
```
```javascript
// ES6의 매개변수의 기본값 설정
function getStringLength(str = ''){
    return str.length();
}

getStringLength(); // 0
getStringLength('heyy'); // 4
```

## 옵셔널 체이닝 연산자
ES11에서 도입된 연산자('?')는 좌항의 피연산자가 ```null 또는 undefined일 경우 undefined를 반환``` 하고 그게 아니면 우항의 프로퍼티 참조를 이어간다. 이외의 Falsy 값 (0, -0, '', false, NaN)이라도 ```null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다. ```

```javascript
var str = '';

var length = str?.length;
console.log(length); // 0

// str = null 이었다면 undefined 반환했을 것
// null or undefined가 아니면 우항 프로퍼티를 참조한다
```

## null 병합 연산자
ES11에서 도입된 연산자('??')는 ```좌항의 피연산자가 null 또는 undefined일 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.```
변수에 기본값을 설정할 때 유용하다.
```javascript
var test1 = null ?? 'default string';
connsole.log(test1); // 'default string'

var test2 = '' ?? 'default string';
connsole.log(test2); // ''
// 좌항의 피연산자가 falsy 값이어도 null이나 undefined가 아니면 좌항의 피연산자를 그대로 반환한다. 
```
