# 함수

함수는 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.

![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/wangjieun/Desktop/modernJSdeepdive/08/funcB.png?version%3D1708957666444)

```javascript
// 함수 정의
function add(x, y) {
    return x + y;
}
// 함수 호출
var result = add(2, 5);

// 함수 add에 인수 2, 5를 전달한 값을 변수 result에 할당하고 호출
console.log(result); // 7
```

-   코드 `재사용` 측면에서 효율적이다.
-   코드 중복을 억제하고 실수를 줄여서 `유지보수 편의성`과 `코드 신뢰성`을 높인다.
-   적절한 함수 식별자명으로 하여금 `코드 가독성`을 향상시킨다.

### 함수 리터럴

리터럴이란 사람이 이해할 수 있는 문자 / 약속된 기호를 통해 값을 생성하는 표기 방식을 말한다.

-   function 키워드, 함수 이름, 매개변수, 변수 목록, 함수 몸체로 구성된다.
-   함수 이름이 있는 함수를 `기명 함수`(named function)라 하고, 이름이 없는 함수를 `익명 함수`(anonymous function)라고 한다.
-   매개변수 목록은 순서에 의미가 있다.
-   매개변수도 함수 내부에서는 변수와 동일하게 취급되므로 식별자 네이밍 규칙을 따라야 한다.

## 함수 정의

| 함수 정의 방식                    | -   | 예시                                              |
| --------------------------------- | --- | ------------------------------------------------- |
| 함수 선언문                       | -   | function add(x, y){ return x + y; }               |
| 함수 표현식                       | -   | var add = function (x, y) { return x + y; }       |
| function 생성자 함수              | -   | var add = new Function('x', 'y', 'return x + y'); |
| 화살표 함수(ES6) (arrow-function) | -   | const add = (x, y) => { return x + y; }           |

### 함수 선언문

```javascript
function mul(x, y) {
    return x * y;
}
// console.dir은 함수 객체의 프로퍼티까지 출력하지만 node환경에서는 console.log와 같은 결과를 출력
console.dir(mul); // f(add(x, y))

console.log(mul(2, 3)); // 6
```

함수 리터럴과 다른 점은 함수 선언문에서는 `함수 이름을 생략할 수 없다`는 것이다.

함수는 함수 내부에서만 함수 이름으로 호출할 수 있기 때문에 자바스크립트 엔진이 자동으로 함수 이름과 같은 이름으로 식별자를 암묵적으로 생성하고 함수 객체를 할당해서 함수 외부에서도 호출할 수 있도록 한다.

```javascript
// 함수 선언문의 mul 함수를 의사 코드(pseudo code)로 표현
var mul = function mul(x, y) {
    return x * y;
};

console.log(add(2, 5)); // 10
// 여기서 호출한 add는 function name add가 아니라 Identifier add이다.
```

이처럼 함수는 함수 이름으로 호출되는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다.

### 함수 표현식

자바스크립트 객체는 일급 객체(=함수를 값처럼 자유롭게 사용할 수 있다)이다.
그래서 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다.

```javascript
// mul 함수를 기명 함수 표현식으로 표현
var min = function mul(x, y) {
    return x * y;
};
console.log(min(3, 4)); // 12
console.log(mul(3, 4)); // ReferenceError: mul is not defined
// mul을 호출하고 싶으면 함수 내부에서만 가능하다. 외부에서는 식별자 이름으로만 호출이 가능

// mul 함수를 익명 함수 표현식으로 표현하면
var min = function (x, y) {
    return x * y;
};
```

함수 표현식에서는 함수 이름을 익명으로 표기할 수 있다.

### 함수 생성 시기와 호이스팅

함수 선언문은 `표현식이 아닌 문`이고, 함수 표현식은 `표현식인 문`이다.

```javascript
console.dir(add); // f add(x, y)
console.dir(min); // undefined

console.log(add(1, 2)); // 3
console.log(min(5, 2)); // TypeError: min is not a function

// 함수 선언문
function add(x, y) {
    return x + y;
}

// 함수 표현식
var min = function (x, y) {
    return x - y;
};
```

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 함수 이름과 동일한 이름의 식별자가 암묵적으로 생성되어서 (=함수 객체가 먼저 생성된다) 런타임에는 이미 할당까지 완료된 상태이므로 코드의 맨 선두에 실행된 것처럼 동작한다. 이를 `함수 호이스팅`이라고 한다.

반면 함수 표현식은 변수에 할당된 값이 함수 리터럴인 문이다. 변수 선언은 런타임 이전에 undefined로 초기화되지만 변수 할당문의 평가는 런타임에 실행되므로 함수 표현식의 함수 리터럴도 런타임에 실행된다.
따라서 여기엔 `변수 호이스팅`이 발생된다.

따라서 `함수의 생성 시점이 다르기 때문에` 함수 선언문으로 정의한 함수는 선언문 이전에 호출이 가능하고, 함수 표현식으로 정의한 함수는 표현식 이전에 호출이 불가능하다.

### function 생성자 함수

new 연산자와 함께 Function 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달한다. 하지만 `클로저`가 생성되지 않는 등 함수 선언문 / 함수 표현식과 다르게 동작한다.

```javascript
var add = new Function("x", "y", "return x + y;");
```

### 화살표 함수

ES6에서 도입된 화살표 함수(arrow function), 항상 익명 함수로 정의한다.

```javascript
const add = (x, y) => {
    return x + y;
};

console.log(add(2, 4)); // 6
```

## 함수 호출

### 매개변수와 인수

함수 외부에서 함수 내부로 함수를 실행하는데 필요한 값을 전달해야 할 때 매개변수(parameter)를 통해 인수(argument)를 전달한다. 인수는 값으로 표현할 수 있는 표현식이어야 한다.

![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/wangjieun/Desktop/modernJSdeepdive/08/parameter.png?version%3D1709022247983)

매개변수는 함수의 몸체 내부에서 변수와 동일하게 취급된다.함수가 호출될 때 암묵적으로 매개변수가 생성되고 일반 변수처럼 undefined로 초기화된 이후 인수가 매개변수로 순서대로 전달되고 함수가 실행된다.

```javascript
function add(x, y) {
    return x + y;
}

console.log(add(5, 7)); // 12

console.log(x, y); // ReferenceError: x is not defined
// 매개변수를 외부에서 참조 불가(매개변수의 스코프는 함수 내부)

console.log(add(7)); // NaN
// 7 + undefined가 되기 때문에 NaN을 반환

console.log(add(1, 5, 7)); // 6
// 매개변수보다 인수가 더 많은 경우 초과되는 인수는 무시
```

초과된 인수는 버려지는 게 아니라 모든 인수는 암묵적으로 `arguments 객체`의 프로퍼티로 보관된다.

### 인수 확인

```text
- 자바스크립트는 매개변수와 인수의 갯수가 일치하는지 확인하지 않는다.
- 동적 타입 언어라서 자바스크립트 함수는 매개변수의 타입을 사전에 설정할 수 없다.
```

따라서 자바스크립트의 경우 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.

1. `typeof 연산자`를 사용하는 방법
2. 인수가 전달되지 않은 경우 `단축 평가`를 사용하는 방법
3. `매개변수에 기본값`(default value)을 할당하는 방법
4. 정적 타입 선언이 가능한 `타입스크립트`를 사용하는 방법

#### typeof 연산자

```javascript
// 타입 확인을 하고 있으나 인수의 개수를 확인하고 있지 않음.
// arguments 객체를 통해 인수의 갯수 확인이 가능함

function add1(x, y) {
    if (typeof x !== "number" || typeof y !== "number") {
        throw new TypeError("인수가 숫자값이 아님");
    }
    return x + y; // 인수가 숫자값일 때 이곳을 실행
}

console.log(add1(2)); // TypeError: 인수가 숫자값이 아님
console.log(add1("2", 3)); // TypeError: 인수가 숫자값이 아님
console.log(add1(2, 3)); // 5
```

#### 단축 평가

인수가 전달되지 않은 경우 `단축 평가`를 이용해 매개변수에 기본값을 할당할 수 있다.

```javascript
function add2(x, y, z) {
    x = x || 0;
    y = y || 0;
    z = z || 0;
    return x + y + z;
}

console.log(add2()); // 0
console.log(add2(1)); // 1
console.log(add2(1, 2)); // 3
console.log(add2(1, 2, 3)); // 6
```

#### 매개변수 기본값

ES6에 도입된 `매개변수 기본값`을 사용해서 간소화가 가능하다.

```javascript
function add3(x = 0, y = 0, z = 0) {
    return x + y + z;
}

console.log(add3()); // 0
console.log(add3(3)); // 3
console.log(add3(3, 4)); // 7
console.log(add3(3, 4, 5)); // 12
```

### 매개변수의 최대 갯수

함수는 한 가지 일만 해야되며 가급적 작게 만들어야 한다. 매개변수는 최대 3개를 권장하며 그 이상이 필요하다면 객체로 만들어서 인수로 넘기는 것이 유리하다.

```javascript
// jQuery의 ajax메서드에 객체를 인수로 넘기는 예시

$.ajax({
    method: "POST",
    url: "/user",
    data: { id: 1, name: "Olivia" },
    cache: false,
});
```

하지만 함수 외부에서 내부로 전달한 객체를 내부에서 변경하면 외부의 객체까지 변경되는 `부수 효과`(side effect)가 발생한다는 주의점이 있다.

### 반환문

반환문은 return 키워드와 표현식(반환값)으로 이루어져 있다.

```javascript
function add(x, y) {
    return x + y; // 반환문
}
```

#### 반환값의 역할

-   함수의 실행을 중단하고 함수 몸체를 빠져나간다. 반환문 이후에 다른 문이 있다면 무시된다.
-   return 키워드 뒤의 표현식(반환값)을 평가해 반환한다. 표현식을 명시적으로 지정하지 않으면 undefined가 반환된다.

#### 반환문의 특징

-   return문의 생략이 가능하나 함수 몸체를 순차적으로 실행한 후 undefined를 반환한다.
-   return과 반환값의 표현식 사이에 줄바꿈이 있으면 세미콜론 삽입 기능으로 인해 반환값의 표현식이 실행되지 않고 return까지만 평가되어 undefined를 반환한다.
-   함수 몸체 내부에서만 사용이 가능하다. 전역으로 사용하면 SyntaxError: Illegal return statement가 발생한다. (하지만 node 환경은 독립적인 파일 스코프를 가졌기 때문에 파일 가장 바깥에서 반환문을 사용해도 에러 발생 X)

### 참조에 의한 전달과 외부 상태의 변경

매개변수도 함수 몸체 내부에서 변수와 같이 취급된다고 했으므로 `값에 의한 전달`과 `참조에 의한 전달`방식을 따른다.

```javascript
function changeValue(primitive, obj) {
    primitive += 200;
    obj.name = "Kim";
}

var num = 100;
var person = { name: "Lee", age: 20 };

// 원시 값은 원본 값 자체가 복사되어 사본에 새로 전달되고, 객체는 사본에 참조값이 복사되어 참조값을 원본과 공유한다.
changeValue(num, person);

console.log(num); // 100
console.log(person); // { name: 'Kim', age: 20 }
```

따라서 원시값인 num은 원본이 훼손되지 않았고, 객체값인 person은 원본이 훼손되었다.

의도치 않은 객체의 변경을 추적하기 위해 옵저버 패턴 등 다양한 방식으로 추가 대응을 할 수 있다.

## 함수의 형태

### 즉시 실행 함수 표현 (IIFE, Immediately Invoked Function Expression)

-   ()로 인해 전역 스코프에 불필요한 변수를 추가해서 오염시키는 것을 방지할 수 있을 뿐만 아니라 IIFE 내부 안으로 다른 변수들이 접근하는 것을 막을 수 있다.
-   Grouping Operator ()를 통해 JavaScript 엔진은 함수를 즉시 해석해서 실행한다.

```javascript
// function 기명함수
(function foo() {
    ...
})();

foo(); // ReferenceError: foo is not defined


// 익명함수
((){

})();

// async IIFE
(async () => {
    ...
})()
```

#### 일반 함수와 같이 값을 반환할 수 있고 인수를 전달할 수도 있다.

```javascript
// 값 반환 가능
var res = (function () {
    var a = 1;
    var b = 2;
    return a + b;
})();

console.log(res); // 3

// 인수 전달 가능
var res = (function (a, b) {
    return a + b;
})(2, 3);

console.log(res); // 5
```

#### 전역을 오염시키는 것을 방지할 수 있다.

```javascript
((){
// 초기화 객체
    let firstVariable;
    let secondVariable;
})();

// firstVariable과 secondVariable은 이 함수 실행 이후 사용 불가
```

#### 비동기 함수 실행

async IIFE를 사용해서 top-level await이 없는 이전 브라우저 및 JavaScript 런타임에서도 await 및 for-await을 사용할 수 있다.

```javascript
const getFileStream = async (url) => {
    // code
};

(async () => {
    const stream = await getFileStream("https://domain.name/path/file.ext");
    for await (const chunk of stream) {
        console.log({ chunk });
    }
})();
```

### 재귀 함수 (Recursive Function)

`재귀 호출`(Recursive Call)을 실행하는 함수.
무한 루프에 빠질 위험이 있고, `스택 오버플로우`로 에러를 발생시킬 수 있으므로 반복문보다 재귀함수를 사용할 때 더 직관적으로 이해할 수 있을 때만 사용하는 편이 좋다.

```javascript
const factorial = (n) => {
    if (n === 0) {
        return 1;
    } else {
        return n * factorial(n - 1); // 재귀 호출.
    }
};
console.log(factorial(10));
// 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1
// 3628800
```

DOM 같은 트리 구조의 모든 노드를 가져오는 건 재귀를 사용하는 게 더 간편하다.

```javascript
function walkTree(node) {
    if (node == null) {
        return;
    }
    // 노드
    for (var i = 0; i < node.childNodes.length; i++) {
        walkTree(node.childNodes[i]);
    }
}
```

### 중첩 함수 (Nested Function)

중첩 함수는 외부 함수 내에서만 호출할 수 있고 외부 함수를 돕는 헬퍼 역할을 한다. ES6부터 문이 위치할 수 있는 위치면 어디든지 가능하지만 `호이스팅`으로 인해 혼란이 발생할 수 있으므로 if문이나 for문 등의 코드 블록에서 함수 선언문으로 작성하는 건 권장하지 않는다.

```javascript
function outer() {
    var a = 10;

    // inner funciton
    function inner() {
        var b = 5;
        console.log(a + b); // outer 함수의 변수를 참조할 수 있다.
    }
    inner();
}
outer();
```

### 콜백 함수

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 `콜백 함수`라고 한다. 그리고 매개변수를 통해 함수의 외부에서 콜백 함수를 전달 받은 함수를 `고차 함수`라고 한다.

```javascript
function repeat(n, f) { // 고차함수
    for(var i = 0; i < n; i++){
        f(i);
    }
}
var logAll = function(i) => { // 콜백함수
    console.log(i);
}

repeat(5, logAll); // 0 1 2 3 4
```

콜백 함수를 다른 곳애서도 호출할 필요가 있거나, 콜백 함수를 전달받는 고차 함수가 자주 호출된다면 함수 외부에서 콜백 함수를 정의하고 함수 참조를 통해 고차 함수에 전달하는 편이 효율적이다.

```javascript
var logOdds = function(i) => {
    if(i % 2) console.log(i);
}

repeat(5, logOdds); // 1 3
```

만약 콜백함수가 `고차함수 내부에만 호출된다면` 콜백함수를 `익명 함수 리터럴`로 정의하면서 바로 고차함수에 전달하는 게 일반적이다. 이때 콜백 함수로서 전달된 함수 리터럴은 고차함수가 호출될 때마다 평가되어 함수 객체를 생성한다.

```Javascript
// logOdds 함수
repeat(5, function(i) => {
    if(i % 2) console.log(i);
}); // 1 3
```

#### 콜백함수를 사용하는 배열 고차함수 map, filter, reduce

```javascript
const mapArr = [1, 2, 3].map((x) => x * 2);

console.log(mapArr); // Array [2, 4, 6]
```


```javascript
const filterArr = [1, 2, 3].filter((x) => x % 2);

console.log(mapArr); // Array [1, 3]
```

```javascript
const reduceArr = [1, 2, 3].reduce(function(x, y) {
            return x + y
          }, 0);

console.log(mapArr); // 6
```

### 순수 함수와 비순수 함수 (Pure Function, Impure Function)
어떤 외부 상태에도 의존하지 않고 변경하지도 않는, 즉 부수 효과가 없는 함수를 `순수 함수`라고 하고 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수를 `비순수 함수`라고 한다. 

순수 함수는 인수의 불변성을 유지하고, 함수의 외부 상태를 변경하지 않는다.
```javascript
var count = 0;

function increase(n) { // 순수함수 increase는 동일한 인수라면 동일한 값을 반환
    return ++n;
}

// 순수함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count); 
console.log(count); // 1

count = increase(count);
console.log(count); // 2
```
함수 내부에서 외부 상태를 직접 참조하지 않더라도 매개변수를 통해 객체를 전달 받으면 비순수 함수가 된다. (*참조에 의한 전달과 값에 의한 전달)
```javascript
var count = 0;

function increase() { // 외부 상태(여기서는 count)에 의존하며 외부 상태를 변경한다.
    return ++count;
}

// 비순수함수는 외부 상태(count)를 변경하기 때문에 추적하기 까다로워짐
increase(); 
console.log(count); // 1

increase(); 
console.log(count); // 2
```