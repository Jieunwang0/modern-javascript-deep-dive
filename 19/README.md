# 배열

배열이 갖고 있는 값을 요소라고 부른다. 이 배열의 요소는 자신의 위치를 나타내는 인덱스를 가진다.

```javascript
const arr = ["apple", "banana", "mango"];

for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
// apple
// banana
// mango
```

배열은 인덱스와 length 프로퍼티를 갖기 때문에 for문을 통해 요소에 접근할 수 있다.

## 자바스크립트의 배열

자바스크립트에 배열이라는 타입은 존재하지 않는다.

자바스크립트의 배열은 배열의 요소를 위한 각각의 메모리 공간이 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다. 일반적인 배열의 동작을 흉내낸 특수한 **객체**라고 볼 수 있다.

자바스크립트의 배열은 객체지만 일반 객체와 구별되는 가장 명확한 차이는 `값의 순서`와 `length 프로퍼티`이다.

| -               | 일반 객체        | 배열          |
| --------------- | ---------------- | ------------- |
| 구조            | 프로퍼티 키와 값 | 인덱스와 요소 |
| 값의 참조       | 프로퍼티 키      | 인덱스        |
| 값의 순서       | X                | O             |
| length 프로퍼티 | X                | O             |

### 일반 배열과 자바스크립트의 배열의 차이

-   `일반 배열`은 **인덱스로 요소에 빠른 접근이 가능**하다. 하지만 요소 삽입/삭제에는 효율적이지 않다.
-   `자바스크립트 배열`은 해시 테이블로 구현된 객체라서 인덱스로 요소에 접근할 경우 효율이 떨어지지만, **요소 삽입/삭제**에는 일반 배열보다 빠른 성능을 기대할 수 있다.

## length 프로퍼티와 희소 배열

-   **희소 배열**: 배열의 요소가 연속적으로 이어져 있지 않는 배열

length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

```javascript
const arr = [1, 2, 3, 4, 5];

arr.length = 2;

console.log(arr); // [1, 2]
```

하지만 length 프로퍼티 값보다 더 큰 숫자값을 할당할 경우 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.

```javascript
const arr = [1, 2];

arr.length = 5;

console.log(arr); // [1, 2, empty × 3]
```

empty × 3는 실제로 추가된 배열의 요소가 아니고, 값이 존재하지 않는다.
이런 배열을 `희소 배열`이라고 하는데 자바스크립트가 문법적으로 허용하나 생성을 권장하지 않는다.

## 배열 생성 방식

### 배열 리터럴

```javascript
const arr = [1, 2, 3];

console.log(arr); // [1, 2, 3]
console.log(arr[2]); // 3
console.log(arr.length); // 3
```

### Array 생성자 함수

-   인수를 2개 이상 전달한 경우 인수를 요소로 갖는 배열이 생성된다.

```javascript
const arr = new Array(13, 20);

console.log(arr); // [13, 20]
console.log(arr[2]); // undefined
console.log(arr.length); // 2
```

-   인수가 1개이고 숫자가 아닌 경우 인수를 요소로 갖는 배열이 생성된다.

```javascript
const arr = new Array("apple");

console.log(arr); // ['apple']
console.log(arr[0]); // apple
console.log(arr.length); // 1
```

-   인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열을 생성한다.

```javascript
const arr = new Array(20);

console.log(arr); // [empty × 20]
console.log(arr[2]); // undefined
console.log(arr.length); // 20
```

-   new 연산자와 함께 호출하지 않아도, 즉 일반 함수로 호출해도 생성자 함수로 동작한다. 내부에서 new.target을 확인하기 때문이다.

### Array.of, Array.from (ES6)

**Array.of**

Array.of는 전달된 인수를 요소로 갖는 배열을 생성한다. Array 생성자 함수와 다르게 인수가 1개이고 숫자여도 인수를 요소로 갖는 배열을 반환한다.

```javascript
Array.of(1); // [1]

Array.of(1, 2, 3); // [1, 2, 3]

Array.of("string"); // ['string']
```

**Array.from**

Array.from는 유사 배열 객체이나 이터러블 객체를 인수로 전달받아 배열로 변환해서 반환한다.

```javascript
Array.from({ length: 2, 0: "a", 1: "b" }); // ['a', 'b']

// 문자열은 이터러블
Array.from("string"); // ['s', 't', 'r', 'i', 'n', 'g']

// length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소에 채운다
Array.from({ length: 2 }); // [undefined, undefined]

// 두번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

## 배열 요소

### 참조

```javascript
const arr = [1, 2];

console.log(arr[0]); // 1

console.log(arr[4]); // undefined
```

존재하지 않는 요소에 접근하려고 하면 undefined를 반환한다.

같은 이유로 희소 배열의 존재하지 않는 요소를 참조해도 undefined를 반환한다.

### 추가와 갱신

```javascript
const arr = [1, 2];

console.log(arr.length); // 2

arr[2] = 10;

console.log(arr); // [1, 2, 10]

console.log(arr.length); // 3
```

현재 배열의 length 프로퍼티보다 `더 큰 인덱스로 새로운 요소를 추가하려고 하면 희소 배열이 된다.`

```javascript
const arr = [1];

arr[10] = 10;

console.log(arr); // [1, empty × 9, 10]

console.log(arr.length); // 11
```

이때 인덱스로 요소에 접근해서 값을 직접 할당하지 않은 요소는 생성되지 않는다.

<img width="607" alt="스크린샷 2024-04-02 오후 5 42 53" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/0706936c-5260-4c40-a58c-7e83ae46c848">

### 삭제

배열은 객체이기에 delete 연산자를 사용할 수 있다. 하지만 **length 프로퍼티에 영향을 주지 않아서** 희소 배열을 만들어 버릴 수 있기 때문에 사용을 지양해야 한다.

대신 **Array.prototype.splice** 메서드를 사용한다.

```javascript
const arr = [1, 2, 3];

// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);

// length 프로퍼티가 자동 갱신된다.
console.log(arr); // 1, 3
```

## 배열 메서드

-   원본 배열을 직접 변경하는 메서드 ( mutator method )

    -   원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 주의해야 한다.

-   원본 배열을 직접 변경하지 않고, 새로운 배열을 생성하여 반환하는 메서드 ( accessor method )
    -   가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.

### mutator method

**Array.prototype.push**

인수로 **전달받은 모든 값을 원본 배열의 마지막 요소로 직접 추가**하고 `변경된 length 프로퍼티 값을 반환한다.`

```javascript
const arr = [0, 1];

let result = arr.push(2, 3);

console.log(result); // 4
console.log(arr); // [0, 1, 2, 3]
```

push 메서드는 성능면에서 좋지 않고, **length 프로퍼티를 사용**하여 직접 배열의 마지막에 요소를 추가하는 방법이 더 빠르다.

```javascript
const arr = [0, 1];

arr[arr.length] = 3;

console.log(arr); // [0, 1, 3]
```

아니면 **ES6의 스프레드 문법**을 사용해서 원본 배열에 영향을 끼치는 부수 효과 없이 추가할 수 있다.

```javascript
const arr = [0, 1];

let result = [...arr, 3];

console.log(result); // [0, 1, 3]
```

**Array.prototype.pop**

```javascript
const arr = [1, 2, 3];

let result = arr.pop();

console.log(result); // 3
console.log(arr); // [1, 2]
```

**Array.prototype.unshift**

인수로 전달받은 모든 값을 원본 배열의 선두로 요소를 추가하고 `변경된 length 프로퍼티를 반환`한다.

```javascript
const arr = [1, 2];

let result = arr.unshift(3, 4);

console.log(result); // 4
console.log(arr); // [3, 4, 1, 2]
```

ES6의 **스프레드 문법**으로 대체할 수 있다. 단, 부수 효과가 없기 때문에 원본 배열에는 영향을 끼치지 않는다.

```javascript
const arr = [1, 2];

let result = [3, ...arr];

console.log(result); // [3, 1, 2]
console.log(arr); // [1, 2]
```

**Array.prototype.shift**

원본 배열에서 첫번째 요소를 제거하고 `제거된 첫번째 요소를 반환`한다.

원본 배열이 빈 배열이면 undefined를 반환한다.

```javascript
const arr = [1, 2];

let result = arr.shift();

console.log(result); // 1
console.log(arr); // [2]
```

**Array.prototype.splice**

```
splice(start, deleteCount, items)
```

-   start → 제거하기 시작할 인덱스 넘버
    -   음수일 경우 -1이면 마지막 요소를 가리키고, -n면 배열의 끝부터 n번째 요소를 가리킨다.
    -   start만 지정되면 start번째 인덱스 요소부터 모든 요소를 제거한다.
-   deleteCount → start번째 인덱스 요소부터 제거할 요소의 갯수
-   items → 제거한 위치에 삽입할 요소들의 목록. 생략할 경우 제거만 한다.

splice 하면 `제거한 요소의 배열을 반환`하고, 원본 배열을 확인하면 해당 요소들이 제거된 모습을 볼 수 있다.

```javascript
const arr = [1, 3, 4];

let result = arr.splice(1, 2);
console.log(result); // [3, 4]
console.log(arr); // [1]

result = arr.splice(0, 1, 2, 3);
console.log(result); // [1]
console.log(arr); // [2, 3]
```

**Array.prototype.fill**

인수로 `전달받은 하나의 값을 요소로 채워서` 원본 배열을 직접 변경 후 반환한다.

```
fill(item, start, end)
```

-   items → 채울 값
-   start → 요소 채우기를 시작할 인덱스
-   end → 요소 채우기를 멈출 인덱스

1. item만 전달했을 경우

```javascript
const arr = [0, 0, 0, 0, 0];

arr.fill(1);
console.log(arr); // [1, 1, 1, 1, 1]
```

2. n부터 n까지의 인덱스만 item으로 바꾸는 경우

```javascript
const arr = [0, 0, 0, 0, 0];

arr.fill(7, 2, 4);
console.log(arr); // [0, 0, 7, 7, 0]
```

fill로 요소를 채울 경우 **하나의 값만 요소로 채울 수 있다는 단점**이 있다.

대신 Array.from 메서드로 두번째 인수로 전달한 콜백 함수를 통해 첫번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하며 호출하고, 콜백함수의 반환값으로 구성된 배열을 반환하면 요소값을 만들면서 배열을 채울 수 있다.

**Array.prototype.reverse**

`원본 배열을 반대로 뒤집는다.` 반환값은 직접 변경된 원본 배열이다.

```javascript
const arr = [1, 2, 3, 4];

let result = arr.reverse();
console.log(result); // [4, 3, 2, 1]
```

### accessor method

**Array.prototype.concat**

인수로 전달된 값(배열 or 원시값)을 `원본 배열의 마지막 요소로 추가한 새로운 배열`을 반환한다. 원본 배열은 변경되지 않는다.

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4, 5];

// 원본.concat(마지막 요소로 추가할 것)
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열인 arr1에는 영향을 끼치지 않기 때문에
// [1, 2, 3, 4, 5, 6]이 아닌 [1, 2, 6]
result = arr1.concat(6);
console.log(result); // [1, 2, 6]
```

ES6의 스프레드 문법으로 대체할 수 있다.

```javascript
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

push/unshift 메서드와 concat 메서드가 서로를 대체할 수는 있지만 원본 배열을 변경하는지의 차이가 있다. 이것들 대신 ES6 스프레드 문법으로 일관성있게 작성하는 것을 권장한다.

**Array.prototype.join**

```
join(separator);
```

-   원본 배열의 `모든 요소들을 문자열로 변환하고, 구분자로 연결한 문자열을 반환`한다.
    -   구분자는 생략이 가능하며, 기본 구분자는 콤마이다.

```javascript
const arr = [1, 2, 3, 4];

arr.join(); // '1,2,3,4'
arr.join(''); // '1234'
arr.join(:); // '1:2:3:4'
```

**Array.prototype.slice**

```
slice(start, end)
```

-   start → 복사를 시작할 인덱스
    -   음수일 경우 -n면 배열의 끝부터 n번째 요소를 가리킨다.
-   end → 복사를 종료할 인덱스. **end 인덱스의 요소는 복사되지 않는다.**
    -   생략 가능하다. 생략시 기본값은 length 프로퍼티 값이다.

인수로 `전달된 범위의 요소를 복사해서 새로운 배열로 반환`한다.
이때 생성한 복사본은 얕은 복사를 통해 생성된다.

```javascript
const users = [
    { id: 1, name: "Eren" },
    { id: 2, name: "Mikasa" },
    { id: 3, name: "Armin" },
];

const _users = users.slice();
// const _users = [...users];

console.log(_users === users); // false, 참조값이 다른 별개의 객체이다.
console.log(_users[0] === users[0]); // true, 배열 요소의 참조값이 같다.
// = 얕은 복사
```

## 배열 고차 함수

`고차 함수(Higher-Order Function)`: 함수를 인자로 받거나 또는 함수를 반환함으로써 작동하는 함수
외부 변경이나 가변 데이터를 피하고 불변성을 지향하는 **\*함수형 프로그래밍**을 지향한다.

`함수형 프로그래밍`: 순수 함수와 보조 함수의 조합으로 조건문과 반복문을 제거하여 로직의 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍.

### Array.prototype.sort

-   배열의 요소를 정렬한다.
-   default 정렬 순서는 오름차순이다.
-   mutator method

문자열 요소의 배열이면 문제없이 작동된다.

```javascript
// 문자열 요소의 배열
const stringArr = ["banana", "apple", "grape"];
const strResult = stringArr.sort();
console.log(strResult); // ['apple', 'banana', 'grape']

// 숫자 요소의 배열
const numArr = [10, 2, 5, 4];
const numResult = numArr.sort();
console.log(numResult); // [10, 2, 4, 5]
```

하지만 숫자 요소의 배열일 경우 유니코드 순으로 정렬된다는 특징이 있다. 그래서 내가 의도한 대로 정렬이 되지 않을 수 있다.

```javascript
const arr = [30, 22, 32, 54, 21, 17];

// 오름차순 정렬, 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
const point = arr.sort((a, b) => a - b);

console.log(point); // [17, 21, 22, 30, 32, 54]
```

따라서 숫자 요소의 배열인 경우 sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 정렬해야 한다.

### Array.prototype.forEach

-   forEach 메서드는 for문을 대체할 수 있는 고차 함수이다.
-   accessor method지만, 콜백 함수를 통해 원본 배열을 변경할 수가 있다.
-   배열의 모든 요소를 빠짐없이 순회하고 중간에 중단할 수 없다.
-   forEach 메서드의 반환값은 undefined이다.


```javascript
forEach(callback, thisArg)
```
- callback → 배열의 각 요소에 대해 실행할 함수
- thisArg → 콜백 함수 내부에서 this로 사용할 객체, 생략이 가능하다.
```javascript
const numbers = [1, 20, 43];
const sum = [];

numbers.forEach((number) => sum.push(number ** 2));
console.log(sum); // [1, 400, 1849]
```

forEach의 두번째 인수로 this에 사용할 객체를 전달하거나, 화살표 함수를 사용해서 this가 상위 스코프를 가리키게 해서 this를 일치시키는 방법이 있다.

```javascript
// 문제 상황
class Numbers {
    numberArray = [];
    multiply(arr) {
        arr.forEach(function(item) {
       // TypeError: Cannot read property 'numberArray' of undefined
             this.numberArray.push(item * item);
        }); 
    }
}
const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray)


// 1. 두번째 인수로 this에 사용할 객체 전달
class Numbers {
    numberArray = [];
    multiply(arr) {
        arr.forEach(function(item){
             this.numberArray.push(item * item);
        }, this); // 콜백 함수 내부에서 this로 사용할 객체를 전달
    }
}
const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray) // [1, 4, 9]

// 2. 화살표 함수의 this 참조시 상위 스코프의 this 참조
class Numbers {
    numberArray = [];
    multiply(arr) {
        arr.forEach(item => this.numberArray.push(item * item));
    }
}
const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray) // [1, 4, 9]
```

희소 배열인 경우 순회 대상에서 제외한다. (map, filter, reduce 메서드도 동일)
```javascript
const arr = [1, , 3];

// for문으로 희소 배열의 존재하지 않는 요소까지 순회
for(let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1 undefined 3
}

// forEach 메서드로 희소 배열의 존재하지 않는 요소 제외
arr.forEach((item)=> console.log(item)); // 1 3
```

### Array.prototype.map

- 요소를 순회하고 콜백 함수의 반환값으로 구성된 새로운 배열을 반환한다.
- accessor method
- 반환한 새로운 배열의 length 프로퍼티 값과 map 메서드를 호출한 배열의 length 프로퍼티 값은 반드시 일치한다.

```javascript
map(callback, thisArg)
```
- callback → 배열의 각 요소에 실행할 함수
- thisArg → 콜백 함수 내부에서 this로 사용할 객체, 생략이 가능하다.

```javascript
const arr = [1, 2, 3, 4, 5];

const roots = arr.map(item => item * 2);

console.log(roots); // [2, 4, 6, 8, 10]
```


---

|-|forEach|map|
|---|---|---|
|차이점|반환값이 undefined|콜백 함수의 반환값으로 구성된 새로운 배열을 반환한다.|
|공통점|자신을 호출한 배열의 모든 요소를 순회하며 인수로 받은 콜백 함수를 반복 호출한다.|"|

---
### Array.prototype.filter

- 반환값이 true인 요소만 추출해서 새로운 배열로 반환한다.
- accessor method
- 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작다.


```javascript
filter(callback, thisArg)
```
- callback → 배열의 각 요소에 실행할 함수
- thisArg → 콜백 함수 내부에서 this로 사용할 객체, 생략이 가능하다.

```javascript
const words = ['spray', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter((word) => word.length > 6);

console.log(result); // ["exuberant", "destruction", "present"]

```
희소 배열인 경우 순회 대상에서 제외한다.

```javascript
console.log([1, , undefined].filter((x) => x === undefined)); // [undefined]
console.log([1, , undefined].filter((x) => x !== 2)); // [1, undefined]
```



### Array.prototype.reduce

자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수 반환값을 다음 순회시에 콜백 함수의 첫번째 인수로 전달하면서 콜백 함수 호출을 반복해서 하나의 결과값을 반환한다. 원본 배열은 영향받지 않는다.

-   accessor method
-   하나의 결과값을 반환한다.
```javascript
reduce(callback, initialValue);
```

-   callback → 첫번째 인수로 콜백 함수를 전달한다.
-   initialValue → 두번째 인수로 초기값을 전달한다. 생략이 가능하다.

```javascript
const arr = [1, 2, 3, 4];
const initialValue = 0;

const sum = arr.reduce((accumulator, currentValue) => accumulator + currentValue, initialValue); 
// 0 + 1 + 2 + 3 + 4

console.log(sum); // 10
```

여기서 accumulator는 이전 호출의 결과 값을 의미한다. 
initialValue가 지정된 경우 첫번째 호출에서 해당 값은 initialValue이다. 지정되지 않은 경우 arr[0]이 된다.
