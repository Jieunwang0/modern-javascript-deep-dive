# 디스트럭처링 할당

**Destructuring Assignment(구조 분해 할당)**

구조화된 배열과 같은 이터러블 또는 객체를 비구조화(Destructuring)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

## 배열 디스트럭처링 할당

ES6에서의 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 한다. 

- `할당 기준은 배열의 인덱스`이다.

```javascript
const arr = [1, 2];

const [x, y] = arr;
console.log(x, y); // 1 2
```

-   우변에 이터러블을 할당하지 않으면 에러가 발생한다.

-   변수의 갯수와 이터러블의 `요소 갯수가 일치할 필요는 없다.` 어차피 할당의 기준은 배열의 인덱스이기 때문이다.
-   배열 디스트럭처링 할당을 위한 변수에 **기본값**을 설정할 수 있다.

```javascript
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3
```

-   배열 디스트럭처링 할당을 위한 변수에 Rest 요소 ...을 사용할 수 있다. 단, 마지막에 위치해야 한다.

```javascript
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

## 객체 디스트럭처링 할당

ES6의 객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.
- **프로퍼티 키를 기준**으로 디스트럭처링 할당이 이루어진다. `순서는 의미 없다.`
```javascript
const user = { firstName: "Trayce", lastName: "Davis" };

const { lastName, firstName } = user;
console.log(firstName, lastName); // Trayce Davis
```
- 우변에 객체 또는 개체로 평가될 수 있는 표현식을 할당하지 않으면 에러가 발생한다.
- `프로퍼티 축약 표현`을 통해 프로퍼티 키와 값의 이름이 같으면 값을 생략할 수 있다.
```javascript
const { lastName: lastName, firstName: firstName} = user;
// 이 둘은 동치다.
const { lastName, firstName } = user;
```
- 객체 디스트럭처링 할당을 위한 변수에 **기본값**을 설정할 수 있다
- 객체 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음 예시처럼 변수를 선언하면 된다.
```javascript
const { firstName, lastName = "Davis" } = { firstName : "Trayce"};
console.log(firstName, lastName); // Trayce Davis

const { firstName: fn, lastName: ln = "Davis" } = { firstName : "Trayce"};
console.log(fn, ln); // Trayce Davis

```
- 객체에서 원하는 프로퍼티 키로 **원하는 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때** 사용된다.
- 객체 디스트럭처링 할당은 객체를 인수로 전달받는 **함수의 매개변수에도 사용이 가능**하다. 
```javascript
const str = 'Hello';
const {length} = str;
console.log(length); // 5

const todo = { id: 1, content: 'HTML', completed: false };
const {id} = todo;
console.log(id); // 1
```

- 배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.
```javascript
const todos = [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'JavaScript', completed: true }
    ];

// todos의 두번쨰 요소인 객체의 id 프로퍼티만 추출한다.
const [, {id}] = todos;
console.log(id); // 2

// 중첩 함수의 경우
const user = {
    name: 'Lee',
    address: {
        city: 'Seoul',
        zipCode: 26109,
    }
};
// address 프로퍼티 키로 객체를 추출하고 객체의 city 프로퍼티 키로 값을 추출한다.
const { address: {city}} = user;
console.log(city); // Seoul
```

-  객체 디스트럭처링 할당을 위한 변수에 Rest 요소 ...을 사용할 수 있다. 단, 마지막에 위치해야 한다.

```javascript
const {x, ...y} = [x: 1, y: 2, z: 3];
console.log(x, y); // 1 {y: 2, z: 3}
```
