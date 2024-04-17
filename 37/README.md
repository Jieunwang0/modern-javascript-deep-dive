# 제너레이터와 async/await

## 제너레이터generator
- 제너레이터 함수는 함수 호출자에게 함수 실행 제어권을 양도할 수 있다
- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받기가 가능하다.
- 제너레이터 함수를 호출하면 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

### 제너레이터 함수
- 화살표 함수로 정의할 수 없다.
- new 연산자와 함께 생성자 함수로 호출 불가하다.
```javascript
// 제너레이터 함수 선언문
function* genDecFunc() {
    yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
    yield 1;
}

// 제너레이터 메서드
const obj = {
    * genObjMethod() {
        yield 1;
    };
};

// 제너레이터 클래스 메서드
class MyClass {
     * genClsMethod() {
        yield 1;
    };
}
```
### 제너레이터 객체
- Symbol.iterator를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유한 이터레이터이다.
    - next 메서드뿐만아니라 return, throw 메서드도 가지고 있다.

- **next 메서드**를 호출하면 yield 표현식까지 코드를 실행하고 `yield된 값`을 value 프로퍼티값으로, `false`를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- **return 메서드**를 호출하면 `인수로 전달받은 값`을 value 프로퍼티 값으로, `true`를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

- **throw 메서드**를 호출하면 인수로 전달받은 에러를 발생시키고 `undefined`를 value 프로퍼티 값으로, `true`를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

### 일시 중시와 재개

제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 코드를 실행하고 코드 실행을 일시 중지한다.
```javascript
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```
- 이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다.
- next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

## 제너레이터 활용
### 이터러블 구현
```javascript
// 무한 피보나치 수열 제너레이터 함수로 구현
const fibonachi = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
}());

for (const num of fibonachi) {
    if (num > 10000) break;
    console.log(num);
}
// 1 2 3 ... 4181 6765
```
### 비동기 처리
제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받기가 가능하다는 점을 활용해 프로미스를 사용한 비동기 처리를 then/catch/finally 없이 반환해서 마치 동기 처리처럼 구현할 수 있다.
- 하지만 코드가 복잡해져서 가독성이 나빠진다. 
- 이를 극복하기 위해 ES8에서 async/await 가 도입된다.
## async/await
프로미스 기반으로 동작하며, 프로미스의 후속 처리 메서드를 사용할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

```javascript
async function foo () {
    const a =  await new Promise(resolve => setTimeout(() => resolve(1), 3000));
    const b=  await new Promise(resolve => setTimeout(() => resolve(2), 2000));
    const c =  await new Promise(resolve => setTimeout(() => resolve(3), 1000));

    console.log([a, b, c]); // [1, 2, 3]
};

foo(); // 약 6초 소요
```
모든 프로미스에 await를 거는 건 주의해야한다. 

여기서 foo 함수는 서로 연관없이 개별적으로 수행되는 비동기 처리라서 앞선 비동기 처리를 기다릴 필요가 없다. 그래서 Promise.all로 묶어서 처리를 해보면,
```javascript
async function foo () {
    const res = await Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)),
        new Promise(resolve => setTimeout(() => resolve(2), 2000)),
        new Promise(resolve => setTimeout(() => resolve(3), 1000))
    ]);
    console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요
```
반면 bar 함수는 앞선 처리 결과를 받아서 다음 비동기 처리를 수행하기 때문에 비동기 처리 순서가 보장되어야 하므로 모든 프로미스에 wait를 걸어주고 순차적으로 실행되도록 한다.

```javascript
async function bar(n) {
    const a =  await new Promise(resolve => setTimeout(() => resolve(n), 3000));
    const b=  await new Promise(resolve => setTimeout(() => resolve(a + 1), 2000));
    const c =  await new Promise(resolve => setTimeout(() => resolve(b + 1), 1000));

    console.log([a, b, c]); // [1, 2, 3]
};

bar(1); 약 6초 소요
```
### async
- 언제나 프로미스를 반환한다.
    - 명시적으로 반환하지 않아도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
- 클래스의 constructor 메서드는 async 메서드가 될 수 없다.
    - 클래스의 constructor 메서드는 인스턴스를 반환하지만 async 메서드는 프로미스를 반환한다.

### await
- async 함수 내부에서 await를 사용해야 한다. 
    - await 키워드는 반드시 프로미스 앞에서 사용한다.
- 모든 프로미스에 await를 사용하는 건 주의해서 작성해야 한다.
- settled 상태가 될 때까지 대기하다가 프로미스가 resolve한 결과를 반환한다.
### 에러 처리
- try..catch문 사용 가능하다.
- 에러는 호출자 방향으로 전파된다.
    - 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기에 호출자가 명확하다.
- async 함수 내에서 catch로 에러를 잡아내지 않으면 발생한 에러를 reject 하는 프로미스를 반환한다.
```javascript
const fetch = require('node-fetch');

const foo = async () => {
    try {
        const wrongUrl = 'https://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch (err) {
        console.error(err); // TypeError: Failed to fetch
    }
};

foo();
```