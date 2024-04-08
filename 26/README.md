# 이터러블 iterable

## 이터레이션 프로토콜 : 
ES6 이전에는 다양한 방식으로 순회할 수 있었는데, ES6에서 도입된 이터레이션 프로토콜은 순회가능한 데이터 자료구조를 이터레이션 프로토콜을 준수하는 **이터러블로 통일**해서 for..of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 하나로 묶었다.

이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다.

### 이터러블 프로토콜 :

직접 구현하거나 체인을 통해 상속받은 **Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환**한다. 이 규약을 이터러블 프로토콜이라고 하고 **이터러블 프로토콜을 준수한 객체를 `이터러블`이라고 한다.**

### 이터레이터 프로토콜 :

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. **이터레이터는 next 메서드를 소유**하며 next 메서드를 호출하면 이터러블을 순회하고 **value와 done 프로퍼티를 가진 `이터레이터 리절트 객체`를 반환**한다.
이 규약을 이터레이터 프로토콜이라고 하고 **이터레이터 프로토콜을 준수한 객체를 `이터레이터`라고 한다.**

![iteration protocol](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/dd71f4f8-35dc-49c2-b47e-19e6b51a853f)

### 이터러블

-   Symbol.iterator 메서드를 가진다.
-   순회 가능하다.
-   스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능하다.

```javascript
const array = [1, 2, 3];

// 배열은 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for...of문으로 순회 가능
for (const item of array) {
    console.log(item); // 1 2 3
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용 가능
console.log([...array]); // [1, 2, 3]

// 배열 디스트럭처링 할당의 대상으로 사용 가능
const [a, ...rest] = array;
console.log([a, rest]); // 1, [2, 3]
```

일반 객체에서는 스프레드 문법만 사용할 수 있지만, 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 된다.

### 이터레이터

-   이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
    - next 메서드 호출시, 이터러블을 순회하며 그 결과를 나타내는 이터레이터 리절트 객체를 반환한다.
    - 이터레이터 리절트 객체는 value, done 프로퍼티를 가진다.

```javascript
const array = [1, 2, 3];
const iterator = array[Symbol.iterator()];

console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```

## 빌트인 이터러블

|빌트인 이터러블|Symbol.iterator 메서드|
|---|---|
|Array|Array.prototype[Symbol.iterator]|
|String|String.prototype[Symbol.iterator]|
|Map|Map.prototype[Symbol.iterator]|
|Set|Set.prototype[Symbol.iterator]|
|TypedArray|TypedArray.prototype[Symbol.iterator]|
|arguments|arguments[Symbol.iterator]|
|DOM Collection|NodeList.prototype[Symbol.iterator],  HTMLCollection.prototype[Symbol.iterator]|


## for .. of문
이터러블을 순회하며 요소를 변수에 할당한다. 
```javascript
for(변수선언문 of 이터러블) {...}
```

## 이터러블과 유사 배열 객체

- 유사 배열 객체: 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 일반 객체
```javascript
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3 
};

// for문 순회 가능, 인덱스로 접근 가능
for(let i = 0; i < arrayLike.length; i++){
    console.log(arrayLike[i]); // 1 2 3
}

// 이터러블이 아니므로 for..of문 순회 불가
for (const item of arrayLike){
    console.log(item); // TypeError: arrayLike is not iterable
}
```
Array.from 메서드를 통해 유사 배열 객체 또는 이터러블을 인수로 받아서 배열로 변환해서 반환하면 for..of문을 사용할 수 있다.
```javascript
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3 
};

const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```
## 이터레이션 프로토콜의 필요성

![image (6)](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/cbfbf72a-07ba-40e6-a572-e77e00271135)

다양한 데이터 공급자가 **하나의 순회 방식을 갖도록 규정**하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할을 한다. 

## 사용자 정의 이터러블

- 이터러블을 생성하는 함수 구현 예시- 피보나치 수열(최대값 고정)
```javascript
const fibonachi = {
    [Symbol.iterator]() {
        let [pre, cur] = [0, 1];
        const max = 10;

    return {
        next() {
            [pre, cur] = [cur, pre + cur];

            return { value: cur, done: cur >= max }}
    };
    }

};

for(const num of fibonachi) {
    console.log(num); // 1 2 3 5 8
}
```
- 이터러블을 생성하는 함수 구현 예시 - 피보나치 수열(최대값 외부에서 변경)
```javascript
const fibonachi = function (max) {
    let [pre, cur] = [0, 1];

// Symbol.iterator 메서드는 이터레이터를 반환
    return {
       [Symbol.iterator]() {
      return {
         next() {
         [pre, cur] = [cur, pre + cur];
         return { value: cur, done: cur >= max };
            }
             };
             }
        };
};

for(const num of fibonachi(20)) {
    console.log(num); // 1 2 3 5 8 13
}
```
- 이터러블이면서 이터레이터인 객체를 생성하는 함수
```javascript
const fibonachi = function (max) {
    let [pre, cur] = [0, 1];

// Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
  return {
    [Symbol.iterator]() { return this; }, // this는 
         next() {
         [pre, cur] = [cur, pre + cur];
         return { value: cur, done: cur >= max };
         }
     };
};

let iter = fibonachi(10);

for(const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fibonachi(10);

console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 3, done: false}
console.log(iter.next()); // {value: 5, done: false}
console.log(iter.next()); // {value: 8, done: false}
console.log(iter.next()); // {value: 13, done: true}
```
- 무한 이터러블과 지연 평가
```javascript
const fibonachi = function (max) {
    let [pre, cur] = [0, 1];
  return {
    [Symbol.iterator]() { return this; },
         next() {
         [pre, cur] = [cur, pre + cur];
         return { value: cur };
         }
     };
};

for(const num of fibonachi()) {
    if(num > 10000) break;
    console.log(num); // 1 2 .... 4181 6765
}

const [f1, f2, f3] = fibonachi();
console.log(f1, f2, f3); // 1 2 3
```
위 함수에서는 for...of문이나 배열 디스트럭처링 할당 등이 실행되기 전까지는 데이터를 생성하지 않는다. for..of문의 경우 next 메서드를 호출할 때 데이터가 생성된다. 

배열이나 문자열 등은 데이터를 미리 확보해두고 필요할 때 데이터를 공급하는데, **지연 평가**는 데이터가 필요한 시점이 오기 전까지는 미리 데이터를 생성하지 않다가 **필요해지면 그때 생성하는 기법이다.**
