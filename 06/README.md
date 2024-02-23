# 객체 리터럴

## 객체

자바스크립트에서 원시 타입은 변경 불가능한 값이고, 객체 타입은 변경이 가능한 값이다.

-   0개 이상의 프로퍼티로 구성되어 있으며 프로퍼티는 키와 값으로 이루어져 있다.

```javascript
//  user라는 이름의 객체
var user1 = {
    // 키: 값   <= 프로퍼티
    name: "Carol",
    age: 36,
    sayhi: function() {
        console.log(`Hi! My name is ${this.name}.`)
    }, // <= 메서드
}; 
console.log(typeof user); // object
console.log(user1); // { name: "Carol", age: 36, sayhi: f }
console.log(user1.sayhi); // Hi! My name is Carol.
```
- 프로퍼티: 객체의 상태를 나타내는 ```값```
- 메서드: 객체의 상태를 참조하고 조작할 수 있는 ```동작```

### 객체 리터럴
중괄호 안에 0개 이상의 프로퍼티를 정의한다. 자바스크립트 엔진은 변수가 할당되는 시점에 객체 리터럴을 해석해서 객체를 생성한다.
```javascript
var rel = {}; // 중괄호 안에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다. 
console.log(typeof rel); // object;
```
- 객체 리터럴의 중괄호는 코드블록을 의미하지 않는다. 코드블록은 마치고 중괄호에 세미콜론을 붙이지 않는데 객체 리터럴의 중괄호에는 세미콜론을 붙인다는 차이점이 있다.
- 클래스를 정의하고 new 연산자와 함께 생성자를 호출할 필요없이 객체를 생성할 수 있는 자바스크립트의 유연함과 강력함을 보여주는 객체 생성 방식이다. 
### 프로퍼티 
- 키에 사용하는 이름은 식별자 네이밍 규칙을 지켜서 작성하거나, 따옴표를 정확히 작성해 주어야 한다. 
```javascript
var user = {

// 프로퍼티 키: 프로퍼티 값;
    age: 25,

    firstName: 'jieun',
    'last-name': 'wang',
// last-name: 'wang', 처럼 식별자 규칙을 지키지 않은 키의 따옴표를 생략하면 -를 연산자로 해석해서 SyntaxError 발생
};
```

- 프로퍼티 키를 동적으로 생성이 가능하다.
```javascript
// ES5: 프로퍼티 키 동적 생성
var obj = {};
var key = 'hi';

obj[key] = 'hello';

console.log(obj); // { hi: "hello" }


// ES6: 계산된 프로퍼티 이름
var obj = {};
var key = 'hi';

var obj = { [key]: 'hello' };

console.log(obj); // { hi: "hello" }
```

- 빈 문자열도 프로퍼티 키에 사용 가능하다.
```javascript
var emptyValue = {
    '': '',
}
console.log(emptyValue); // { "": "" }
```

- 프로퍼티 키에 문자열이나 심볼 이외의 값을 사용해도 암묵적 타입 변환을 통해 문자열이 된다.
```javascript
var num = {
    1: 2,
    2: 3,
    3: 4,
};
console.log(num); // { 1: 2, 2: 3, 3: 4 }
// 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표가 붙지는 않지만 내부적으로는 문자열로 변환
```
- 예약어를 프로퍼티 키에 사용해도 에러가 발생하지는 않지만 권장하지 않는다.
- 이미 존재하는 프로퍼티 키를 중복으로 작성하면 나중에 선언한 프로퍼티 키로 덮어쓰게 된다. 에러 표시가 나지않음에 주의하자.
```javascript
var foo = {
    name: "James",
    name: "",
};
console.log(foo); // { name: "" }
```

### 메서드
객체에 묶여있는 함수를 메서드라고 부른다.
```javascript
var ageMultiplyTwo = { //  multiplyTwo <= 메서드
    age: 20,
    multiplyTwo: function() {
        return 2 * this.age; //  this <= ageMultiplyTwo를 가리킨다
    },
}

console.log(ageMultiplyTwo.multiplyTwo()); // 40
```
### 프로퍼티 접근법
- 마침표 프로퍼티 접근 연산자를 사용하는 ```마침표 표기법```
- 대괄호 프로퍼티 접근 연산자를 사용하는 ```대괄호 표기법```
    + 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 ```반드시 따옴표로 감싼 문자열```이어야 한다. 

```javascript
var person = {
    age: 25,
    hobby: 'Running',
}

// 마침표 표기법
console.log(person.age); // 25

// 대괄호 표기법
console.log(person[hobby]); // ReferenceError: hobby is not defined
console.log(person['hobby']); // Running
```
- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.
```javascript
var person = {
    age: 25,
    hobby: 'Running',
}

// 객체에 존재하지 않는 값에 접근하면 undefined를 반환하는데 ReferenceError를 발생시키지 않기 때문에 주의해야 한다.
console.log(person.name); // undefined
```

#### ++ 브라우저랑 node.js의 실행결과가 다른 이유

```javascript
var person = {
    age: 25,
    'last-name': 'wang',
};

person.last-name; // 브라우저 -> NaN
                  // Node.js -> ReferenceError: name is not defined
person.'last-name'; // SyntaxError: Unexpected string
person[last-name]; // ReferenceError: last is not defined
person['last-name']; // wang
```
person.last-name을 평가할 때 브라우저는 -를 연산자로 보고 person.last를 찾지만 last라는 프로퍼티 키가 없기 때문에 person.last에 undefined를 반환한다. 이때 undefined-name와 같게 된다. 

여기서 name은 프로퍼티 키가 아니라 식별자로 해석되는데, Node.js 환경에서는 name이라는 식별자를 찾지만 선언한 적이 없기 때문에 ReferenceError를 반환한다. 브라우저 환경에서는 name이라는 이름의 전역변수가 암묵적으로 존재하는데, 전역변수 name은 창의 이름을 가리키며 기본값이 빈 문자열이다. 결론적으로 undefined-''가 되기 때문에 브라우저는 NaN을, Node.js는 ReferenceError를 반환하게 되는 것이다.

### 프로퍼티 동적 생성과 삭제

```javascript
var person = {
    name: 'jieun',
    age: 25,
};

// 존재하지 않는 프로퍼티에 값을 할당하면 동적으로 생성되어 추가되고 프로퍼티값이 할당된다.
person.hobby = 'Running';

console.log(person); // { name: "jieun", age: 25, hobby: "Running"}



// delete 연산자를 사용해 객체의 프로퍼티를 삭제할 수 있다.
delete person.age;

console.log(person); // { name: "jieun", hobby: "Running"}


// 존재하지 않는 프로퍼티에 delete를 사용하면 에러도 발생하지 않고 그냥 무시된다. 
delete person.job;

console.log(person); // { name: "jieun", hobby: "Running"}
```

### ES6에서 추가된 객체 리터럴의 확장 기능

#### 프로퍼티 축약 표현
- 변수를 프로퍼티 값으로 쓸 때 프로퍼티 키와 변수 이름이 같다면 프로퍼티 키를 생략하는 것이 가능하다. 
```javascript
var x = 1;
var y = 2;

// ES5
var contrac = {
    x: x,
    y: y
};
console.log(contrac); // { x: 1, y: 2 }

// ES6
const contrac = { x, y };
console.log(contrac); // { x: 1, y: 2 }
```

#### 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성

```javascript
// ES5
var prefix = 'prop';
var i = 0
var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}

// ES6
const prefix = 'prop';
let i = 0
const obj = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i
    };

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

#### 메서드 축약 표현
ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다
```javascript
// ES5
var obj = {
    name: '',
    sayhi: function() {
        console.log('Hi!' + 'My name is ' + name);
    }
}
```
ES6에서는 메서드를 정의할 때 function 키워드를 생략할 수 있다.
```javascript
// ES6
var obj = {
    name: '',
    sayhi() {
        console.log(`Hi! My name is ${name}`)
    }
}
```
ES6에서 메서드 축약 표현으로 정의한 함수는 ES5에서처럼 프로퍼티에 할당한 함수와 다르게 ```인스턴스를 생성할 수 없는 non-constructor 함수```이다. 이건 뒤에서 학습할 예정.