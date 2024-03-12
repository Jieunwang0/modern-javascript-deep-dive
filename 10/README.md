# Property Attribute

## 내부 슬롯과 내부 메서드

ECMAScript에서 사용하는 의사 프로퍼티(pseudo property), 의사 메서드(pseudo method)를 말한다.
-   이중 대괄호([[...]])로 감싼 이름들이 내부 슬롯과 내부 메서드이다.
-   개발자가 직접 접근해서 호출하고 조작할 수 있게 하지 않은 자바스크립트 엔진의 내부 로직.
-   일부 내부 슬롯, 내부 메서드에 한하여 간접적으로 접근이 가능한 수단을 제공한다.
    -   [[Prototype]]의 내부 슬롯의 경우, **proto**를 통해 간접적으로 접근이 가능하다.

```javascript
const o = {};
o.[[Prototype]]; // SyntaxError: Unexpected token '['
o.__proto__; // Object Prototype
```

## Property Attribute/Descriptor Object

### Property Attribute

객체에서 프로퍼티를 생성할 때, 프로퍼티 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 생성된다.

-   내부 슬롯이어서 직접 접근이 불가능하다.
-   `Object.getOwnPropertyDescriptor` 메서드를 사용해서 간접적으로 확인이 가능하다.

```javascript
const person = {
    name: 'Jason';
}
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// { value: 'Jason', writable: true, enumerable: true, configurable: true }
```

### Property Descriptor

프로퍼티 어트리뷰트 정보를 제공하는 객체.

-   존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 → undefined
-   기본적으로 하나의 프로퍼티에 대해 프로퍼티 디스크립터 객체를 반환

```javascript
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

-   ES8부터 `Object.getOwnPropertyDescriptors` 메서드를 통해 심볼형 프로퍼티를 포함한 프로퍼티 설명자 전체를 반환할 수 있게 되었다.

```javascript
let descriptors = Object.getOwnPropertyDescriptors(obj);
```

## Data Property & Accessor Property

### 데이터 프로퍼티

     키와 값으로 구성된 읿반적인 프로퍼티.

객체 프로퍼티는 값(value)과 함께 플래그(flag)라 불리는 세 가지 속성을 갖는다.

-   프로퍼티 상태
    -   `정의 가능 여부` ( configurable )
    -   `열거 가능 여부` ( enumerable )
    -   `프로퍼티의 값` ( value )
    -   `값의 갱신 가능 여부` ( writable )

| property attribute | property of property descriptor object | etc                                                                 |
| ------------------ | :--------------------------------------: | ------------------------------------------------------------------- |
| [[Value]]          | value                                  | 프로퍼티 키를 통해 값을 변경하면 [[Value]]에 값을 재할당, 반환한다. |
| [[Configurable]]   | configurable                           | 프로퍼티의 `재정의 가능 여부`를 나타낸다. → boolean                 |
| [[Enumerable]]     | enumerable                             | 프로퍼티의 `열거 가능 여부`를 나타낸다. → boolean                   |
| [[Writable]]       | writable                               | 프로퍼티 `값의 변경 가능 여부`를 나타낸다. → boolean                |

`일반적으로` 프로퍼티가 생성될 때, 프로퍼티를 동적 추가할 때도 [[Value]]의 값은 프로퍼티 값으로 초기화되며, [[Configurable]],[[Enumerable]], [[Writable]]의 값은 `true`로 초기화된다.

하지만 `Object.defineProperty`를 사용해서 프로퍼티를 만든 경우, descriptor에 플래그 값을 명시하지 않으면 플래그 값이 `자동으로 false`가 된다.

```javascript
const person = {
    name: "Jason",
};
person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));
// {
//   age: {configurable : true, enumerable: true, value: 20, writable: true }
//   name: {configurable: true, enumerable: true, value: "Jason", writable: true }
//  }
```

### 접근자 프로퍼티

    자체적으로는 값을 갖지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티.

| property attribute | property of property descriptor object | etc                                                                        |
| ------------------ | -------------------------------------- | -------------------------------------------------------------------------- |
| [[Get]]            | get                                    | 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 → `getter` 함수 호출   |
| [[Set]]            | set                                    | 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 → `setter` 함수 호출 |
| [[Enumerable]]     | enumerable                             | 데이터 프로퍼티의 [[Enumerable]] 과 같다                                   |
| [[Configurable]]   | configurable                           | 데이터 프로퍼티의 [[Configurable]] 과 같다                                 |

```javascript
// 데이터 프로퍼티
const person = {
    firstName: "Hansol",
    lastName: "Kim",

// 접근자 프로퍼티
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    },
};

console.log(person.firstName + ' ' + person.lastName); // Hansol Kim
person.fullName = ‘HS K’
console.log(person); // { firstName: 'HS', lastName: 'K' }
console.log(person.firstName); // HS
console.log(person.lastName); // K


// firstName은 데이터 프로퍼티다.
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor); // {value: 'Hansol', writable: true, enumerable: true, configurable: true}


// fullName은 접근자 프로퍼티다.
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor); // {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

## 프로퍼티 정의

`Object.defineProperty`를 통해 프로퍼티의 어트리뷰트를 정의하면서 프로퍼티를 생성할 수 있다.

```javascript
// 객체에 프로퍼티 1개 정의
Object.defineProperty(obj, property name, {
  value: 값,
  writable: boolean,
  enumerable: boolean,
  configurable: boolean
})
```

`Object.defineProperties`를 사용해 여러개의 프로퍼티를 한번에 정의할 수 있다.

```javascript
// 객체에 프로퍼티 n개 정의
Object.defineProperties(obj, {
// 데이터 프로퍼티
 dataPropertyName 1: {
	  value: 값,
	  writable: boolean,
	  enumerable: boolean,
	  configurable: boolean
  },
  ...
// 접근자 프로퍼티
 AccessorPropertyName 1: {
	  get() { ... },
      set() { ... },
      writable: boolean,
	  enumerable: boolean,
	  configurable: boolean
	}
  ...
})
```

Object.defineProperty를 사용해서 프로퍼티를 생성할 때 디스크립터 객체의 일부를 생략할 수 있는데, 생략된 어트리뷰트는 다음의 기본값이 적용된다.

|property of property descriptor object|property attribute|default value|
|:---:|---|---|
|value| [[Value]] | undefined|
|get| [[Get]] |undefined|
|set| [[Set]] |undefined|
|writable| [[Writable]] |false|
|enumerable| [[Enumerable]] |false|
|configurable| [[Configurable]] |false|

## 객체 변경 방지
- 객체는 기본적으로 변경 가능한 값을 의미하고, 재할당 없이 직접 변경할 수 있다.
    - Object.defineProperty / Object.defineProperties 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.
- strict mode에서 허용하지 않는 프로퍼티 제어 동작에 대해서는 에러를 발생시킨다.
- strict mode가 아닌 경우에는 해당 명령을 무시한다.

|-|메서드|프로퍼티 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기|프로퍼티 어트리뷰트 재정의|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|객체 확장 금지|Object.preventExtensions|X|O|O|O|O|
|객체 밀봉|Object.seal|X|X|O|O|X|
|객체 동결|Object.freeze|X|X|O|X|X|

### 객체 확장 금지(Object.preventExtensions)
```javascript
Object.preventExtensions(obj);

// 확장 가능한 객체인지 여부 → boolean
Object.isExtensible(obj);
```

### 객체 밀봉(Object.seal)
```javascript
Object.seal(obj);

// 밀봉된 객체인지 여부 → boolean
Object.isSealed(obj);
```

### 객체 동결(Object.freeze)
```javascript
Object.freeze(obj);

// 동결된 객체인지 여부 → boolean
Object.isFrozen(obj);
```

### 불변 객체
위의 변경 방지 메서드들은 얕은 변경 방지로, 직속 프로퍼티까지만 변경이 방지되고 중첩 객체에는 영향을 주지 못한다. 

중첩 객체까지 동결해서 변경이 불가능한 불변 객체를 구현하려면 모든 프로퍼티에 재귀적으로 Object.freeze 메서드를 호출해야 한다.
```javascript
function deepFreeze(target){
    // 객체면서 동결되지 않은 객체만 동결
    if(target && typeof target === 'object' && ! Object.isFrozen(target)){
    Object.freeze(target);
    
    // 모든 프로퍼티를 순회하며 재귀적으로 동결한다.
    // keys로 열거 가능한 프로퍼티 키를 배열로 반환하고
    // forEach로 배열을 순회하며 각 요소에 콜백 함수를 실행
    Object.keys(target).forEach(key => deepFreeze(target[key]));
} 
return target;
} 


const person = {
    name: "Jay",
    address:{
        city: "Busan"}
};

deepFreeze(person);

console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // true

person.address.city = "Jeju";
console.log(person); // { name: "Jay", address: { city: "Busan" } };
```