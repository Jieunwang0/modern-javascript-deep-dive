# Symbol
다른 값과 중복되지 않는 유일무이한 값. 주로 유일한 프로퍼티 키를 만들기 위해 사용한다.

## 값의 생성 방법

생성자 함수로 객체를 생성할 것처럼 보이지만 `Symbol은 new 연산자와 함께 호출하지 않는다.`
```javascript
new Symbol(); // TypeError: Symbol is not a constructor
```

Symbol 값은 `변경 불가능한 원시값`이다.
```javascript
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부에 노출되지않아서 내부를 확인할 수 없다.
console.log(mySymbol); // Symbol()
```
- 인수에 문자열을 전달할 수 있다.
    - 심벌 값에 대한 설명이 같더라도 각자 다른 심벌 값이다. **유일무이**하다.
- 객체처럼 접근하면 **래퍼 객체를 생성**한다. 
```javascript
const mySymbol = Symbol('mySymbol');

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString); // Symbol(mySymbol)
```
- 암묵적 타입 변환이 일어나지 않는다. **단, 불리언 타입으로는 암묵적 타입 변환된다.**
```javascript
const mySymbol = Symbol();

console.log(mySymbol + ' '); // TypeError: Cannot convert a Symbol value to a string

console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a number

console.log(!mySymbol); // false
```

### Symbol.for / Symbol.keyFor 메서드
- Symbol.for 메서드
    - 인수로 받은 문자열을 키로 심벌 값의 쌍들이 저장되어있는 `전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색`한다. 
        - 검색했는데 **없으면 새로운 심벌 값을 생성**
        - 검색했는데 **있으면 해당 심벌 값을 반환**
    - 애플리케이션 전역에서 중복되지 않는 심벌 값을 단 하나만 생성해서 전역 심벌 레지스트리를 통해 공유할 수 있도록 한다.
- Symbol.keyFor 메서드
    - `전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.`
    - Symbol.for이 아닌 **Symbol 함수를 호출해서 생성한 심벌 값**은 전역 심벌 레지스트리에 등록되어 **관리되지 않는다.**
```javascript
const s1 = Symbol.for('mySymbol');
const s2 = Symbol.for('mySymbol');
const s3 = Symbol('foo');

console.log(s1 === s2);

Symbol.keyFor(s1); // mySymbol
Symbol.keyFor(s3); // undefined
```
## Symbol과 프로퍼티 키
Symbol값을 프로퍼티 키로 사용할 때, 접근할 때 모두 Symbol값에 대괄호를 사용해야 한다.
```javascript
const obj = {
    [Symbol.for('mySymbol1')]: 1,
    [Symbol.for('mySymbol2')]: 2
}

obj[Symbol.for('mySymbol1')]; // 1
obj[Symbol.for('mySymbol2')]; // 2
```

## Symbol과 프로퍼티 은닉
Symbol값을 프로퍼티값으로 사용해서 생성하 프로퍼티는 for..in문이나 getOwnPropertyNames 메서드 등으로 찾을 수 없다. 

하지만 ES6에서 도입된 Object.getOwnPropertySymbols 메서드로 심벌 값을 프로퍼티 키로 사용애서 생성한 프로퍼티를 찾을 수 있게 되었다.
```javascript
const obj = {
    [Symbol.for('mySymbol')]: 1
}
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolkey = Object.getOwnPropertySymbols(obj)[0];
console.log(obj(symbolkey)); // 1
```
## Symbol과 표준 빌트인 객체 확장

표준 빌트인 객체는 읽기 전용으로 사용하는 게 좋고, 사용자 정의 메서드를 직접 추가하는 건 나중에 중복될 수 있기 때문에 권장하지 않는다.
대신 심벌 값으로 프로퍼티 키를 생성해서 확장하면 충돌 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```javascript
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function() {
    return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```
## Well-Known Symbol

자바스크립트가 기본 제공하는 빌트인 객체 Symbol값을 ECMAScirpt 사양에서 Well-Known Symbol라고 부르고, 이는 자바스크립트 내부 알고리즘에 쓰인다.

예를 들어, for문으로 순회 가능한 빌트인 이터러블은 Well-Known Symbol인 Symbol.iterator를 키로 가지는 메서드를 가진다. 
만약 빌트인 이터러블인 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 Symbol.iterator를 키로 갖는 메서드를 추가하면 된다. 
```javascript
const iterable = {
// Symbol.iterator 메서드를 구현해서 이터러블 프로토콜을 준수
    [Symbol.iterator](){
        let cur = 1;
        const max = 5;

        return {
    // Symbol.iterator는 next 메서드를 소유한 이터레이터를 반환
            next() {
            return {value: cur++, done: cur > max +1};
           } 
        };
    }
};

for (const num of iterable) {
    console.log(num); // 1 2 3 4 5
}

```
작성한 Symbol.iterator는 기존의 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않을 것이다.

이처럼 심벌은 중복되지 않는 상수 값을 생성하는 것은 물론, 기존에 작성한 코드에 영향없이 새로운 프로퍼티를 추가하기 위해 만들어졌다. 
