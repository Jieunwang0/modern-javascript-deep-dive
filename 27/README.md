# 스프레드 문법

```javascript
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token '...'

console.log(...[1, 2, 3]); // 1 2 3
```

- 스프레드 문법을 사용할 수 있는 대상은 `순회할 수 있는 이터러블에 한정`한다.
- 스프레드 문법의 결과물은 `값으로 사용할 수 없고`, 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다. 
- 값이 아니므로 변수에 할당할 수도 없다.

Rest 파라미터와 형태가 동일해서 혼동하기 쉽다. 하지만 정반대의 내용이다.
- **스프레드 문법**: 여러개의 값이 뭉쳐있는 배열과 같은 이터러블을 펼쳐서 `개별적인 값들의 목록`을 만드는 것.
- **Rest 파라미터**: 함수에 전달된 인수들의 `목록을 배열로 전달받기 위해` 매개변수 이름 앞에 ...를 붙이는 것.

## 함수 호출문의 인수 목록에서 사용하는 경우
```javascript
const arr = [1, 2, 3];

const max = Math.max(arr); // NaN
```
Math.max에 숫자가 아닌 배열을 인수로 전달하면 최댓값을 구할 수 없어서 NaN을 반환한다.
```javascript
const arr = [1, 2, 3];

const max = Math.max(...arr); // 3, Math.max(1, 2, 3)과 같다. 
```

## 배열 리터럴 내부에서 사용하는 경우

### concat
ES5에서 2개 배열의 1개의 배열로 결합하고 싶은 경우 concat 메서드를 사용해야 한다.
```javascript
var arr = [1, 2].concat([3, 4]);

console.log(arr); // [1, 2, 3, 4]
```
ES6에서는 스프레드 문법을 통해 배열 리터럴만으로도 결합이 가능하다.
```javascript
const arr = [...[1, 2], ...[3, 4]];

console.log(arr); // [1, 2,3, 4]
```

### split
ES5에서 배열 중간에 다른 배열의 요소를 추가/제거하려면 splice 메서드를 사용한다. 3번째 인수로 배열을 전달하면 배열 자체가 추가된다.
```javascript
var arr1 = [1, 3];
var arr2 = [2, 4];

arr1.splice(1, 0, arr2); 
console.log(arr1); // [1, [2, 4], 3]
// 기대한 결과는 [1, 2, 4, 3]였는데 ...
```
위 예제에서는 [2, 4]를 해체해서 전달해야한다. 이 경우 apply 메서드를 같이 사용하거나 스프레드 문법을 사용하는 방법이 있다.

```javascript
// apply
var arr1 = [1, 3];
var arr2 = [2, 4];

Array.prototype.splice.apply(arr1,[1, 0].concat(arr2)); 
console.log(arr1); // [1, 2, 4, 3] 

// spread
const arr1 = [1, 3];
const arr2 = [2, 4];

arr1.splice(1, 0, ...arr2); 
console.log(arr1); // [1, 2, 4, 3] 
```
### 배열 복사 (얕은 복사)
ES5에서 배열을 복사하려면 slice 메서드를 사용한다. 
```javascript
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```
스프레드 문법을 사용하면 더 가독성 좋게 표현 가능하다.
```javascript
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```
### 이터러블을 배열로 변환
ES5에서 이터러블을 배열로 변환하려면 apply 또는 call을 사용해서 slice를 호출해야 한다.
```javascript
function sum() {
    var args = Array.prototype.slice.call(arguments);

    return args.reduce(fucntion(pre, cur) {
        return pre + cur;
    }, 0);
}

console.log(sum(1, 2, 3)); // 6
```
위 방법은 이터러블뿐만 아니라 유사 배열 객체에도 적용된다.

스프레드 문법이나 Rest 파라미터를 사용해서 더 간편하게 변환 가능하다.
```javascript
// spread
function sum() {
return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6


// rest parameter
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```
그러나 **유사 배열 객체는 스프레드 문법을 사용할 수 없다.** `Array.from` 메서드를 통해 배열로 변환이 가능하다.

```javascript
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
}

const arr = [...arrayLike]; // TypeError: arrayLike is not iterable

Array.from(arrayLike); // [1, 2, 3]
```

## 객체 리터럴 내부에서 사용하는 경우

Rest 프로퍼티와 함께 `스프레드 프로퍼티`를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다. 
```javascript
const obj = {x: 1, y: 2};
const copy = {...obj};
console.log(copy); // {x: 1, y: 2}
console.log(obj === copy); // false


const merged = { x: 1, y: 2, ... { a: 3, b: 4}};
console.log(merged); // {x: 1, y: 2, a: 3, b: 4}
```
스프레드 프로퍼티가 제안되기 이전에는 `Object.assign` 메서드를 사용해서 여러개의 객체를 병합하거나 특정 프로퍼티를 변경, 추가했다.
```javascript
const merged = Object.assign({}, {x: 1, y: 2}, {y: 10, z: 3});
console.log(merged); // {x: 1, y: 10, z: 3}

const changed = Object.assign({}, {x: 1, y: 2}, {y: 100});
console.log(changed); // {x: 1, y: 100}

const added = Object.assign({}, {x: 1, y: 2}, {z: 0});
console.log(added); // {x: 1, y: 2, z: 0}
```
위 예제를 스프레드 프로퍼티로 대체할 수 있다.
```javascript
const merged = {...{x: 1, y: 2}, ...{y: 10, z: 3}};
console.log(merged); // {x: 1, y: 10, z: 3}

const changed = {...{x: 1, y: 2}, y: 100};
console.log(changed); // {x: 1, y: 100}

const added = {...{x: 1, y: 2}, z: 0};
console.log(added); // {x: 1, y: 2, z: 0}
```
