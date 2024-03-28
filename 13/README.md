# 프로토타입

상태 데이터와 이를 조작할 수 있는 동작을 속성을 통해 하나의 논리적인 단위로 묶은 복합적인 자료 구조를 객체라고 한다.

-   상태 데이터: 프로퍼티
-   동작: 메서드

객체 지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임.

## 상속

어떤 객체의 프로퍼티나 메서드를 다른 객체가 상속받아 그대로 재사용할 수 있는 것을 말한다.

```javascript
function Circle(radius) {
    this.radius = radius;
    this.getArea = function () {
        return Math.PI * this.radius ** 2;
    };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

여기서 getArea 메서드는 모든 인스턴스에서 동일한 메서드를 사용하므로 하나를 두고 공유해서 사용하는 것이 효율적이다. 그러나 위의 예시에서는 Circle 생성자 함수로 생성한 circle1과 circle2가 각각 getArea 메서드를 가지고 있게 된다. 이렇게 중복으로 생성되지 않게 하려면 하단의 예시처럼 상속을 구현하면 된다.

```javascript
function Circle(radius) {
    this.radius = radius;
}

Circle.prototype.getArea = function () {
    return Math.PI * this.radius ** 2;
};
// ** : 왼쪽의 숫자를 오른쪽 숫자만큼 제곱합니다.

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

Circle.prototype.getArea를 통해 생성자 함수의 프로토타입에 추가함으로써 Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 `부모 객체 역할`을 하는 Circle.prototype의 `모든 프로퍼티와 메서드를 상속`받게 된다.
![inheritance](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/6652e43e-e089-4921-a869-4a2b5e7892cf)
이로서 circle1과 circle2는 getArea 메서드를 공유해서 사용할 수 있게 된다.

## 프로토타입 객체

프로토타입은 객체 간 상속을 구현하기 위해 사용된다. 어떤 객체의 부모 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티를 제공하고, 상속받은 자식 객체는 부모 객체의 프로퍼티를 본인 것처럼 사용할 수 있다.

프로토타입은 객체가 생성될 때 객체 생성 방식에 의해 결정되고 [[Prototype]]에 저장된다.

![chain](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/53a2de1a-b348-429b-b4e4-aa71d56b55ab)
모든 객체는 하나의 프로토타입을 가지고, 모든 프로토타입은 생성자 함수와 연결되어 있다.

### __ proto __ 접근자 프로퍼티

모든 객체는 __ proto __ 접근자 프로퍼티를 통해 [[Prototype]] 내부 슬롯에 간접 접근이 가능하다.

-   __proto__는 접근자 프로퍼티다.

<img width="461" alt="proto property" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/32a23dad-59a6-4d4c-97ae-564a35494bf9">

자체적으로 값([[Value]])를 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수([[Get]], [[Set]])로 구성된 프로퍼티이다.
__ proto __ 를 통해 프로토타입에 접근하면 내부적으로 getter 함수인 [[Get]]이 호출되고, __ proto __ 를 통해 새로운 프로토타입을 할당하면 setter 함수인 [[Set]]이 호출된다.

```javascript
const obj = {};
const parent = { x: 1 };

// get.__proto__(getter)가 호출되어 obj 객체의 프로토타입 취득
obj.__proto__;
// set.__proto__(setter)가 호출되어 obj 객체의 프로토타입 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

-   __proto__는 상속을 통해 사용된다.

__proto__는 객체가 직접 소유한 프로퍼티가 아니고, Object.prototype의 프로퍼티다.

```javascript
const person = { name: "Lee" };
// person 객체는 __proto__ 프로퍼티를 직접 소유하지 않음.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__")); // {enumerable: false, configurable: true, get: ƒ, set: ƒ}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```
---
#### __ proto __ 를 통해 프로토타입에 접근하는 이유

참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```javascript
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

parent 객체를 child 객체의 프로토타입으로 설정한 후 child 객체를 parent 객체의 프로토타입으로 설정하면 서로가 서로의 프로토타입이 되는 비정산적인 프로토타입 체인이 만들어진다.

![protochain](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/cd65854d-9db0-4a2f-92f5-3d9422cb2eca)

순환 참조하게 되면 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠지기 때문에 프로토타입 체인은 *`단방향 연결 리스트`(Singly-Linked List)로 구현되어야 한다.

---
#### *Singly-Linked List 

연결 리스트는 자료 구조에서 노드를 가리키고 탐색하는 구조를 말한다. 
양방향와 단방향으로 나뉘는데, 단방향 연결 리스트는 다음 노드로만 이동할 수 있는 것을 뜻한다.

![Singly-Linked-List](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/d2460d51-c8da-4b78-8c2f-c116eb98c39b)


---
---

- __ proto __를 코드 내에서 직접 사용하는 것은 권장하지 않는다. 

직접 상속을 통해 prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 모든 객체가 __ proto __ 접근자 프로퍼티를 사용할 수 있지 않다.

```javascript
const obj = Object.create(null); // Object.create()로 새로 만들어진 객체에는 constructor 프로퍼티가 없다.

// 프로토타입이 없는 빈 객체
// 따라서 Object.__proto__를 상속받을 수 없다, 

console.log(obj.__proto___); // undefined

console.log(Object.getPrototypeOf(obj)); // null
// 프로토타입의 참조 -> Object.getPrototypeOf()
// 프로토타입의 교체 -> Object.setPrototypeOf()
```

__ proto __ 대신 메서드(Object.create(), Object.getPrototypeOf(), Object.setPrototypeOf())를 사용하도록 하는 것을 권장한다.

### 함수 객체의 prototype 프로퍼티
함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수(`constructor`)가 생성할 객체-인스턴스-의 프로토타입을 가리킨다.
```javascript
(function() {}).hasOwnProperty('prototype'); // true
```
`non-constructor`(화살표 함수, ES6 메서드 축약 표현으로 생성한 함수)는 prototype 프로퍼티가 없고, 프로토타입도 생성하지 않는다.
```javascript
// arrow function 
const Person = (name) => {
    this.name = name;
};

console.log(Person.hasOwnProperty('prototype')); // false

// non-constructor는 프로토타입을 생성하지 않음
console.log(Person.prototype); // undefined
```
|-|소유|값|사용 주체|사용 목적|
|:---:|:---:|:---:|:---:|:---:|
|__ proto __|모든 객체|프로토타입의 참조|모든 객체|객채가 자신의 프로토타입에 접근 또는 교체하기 위해 사용|
|prototype|constructor|프로토타입의 참조|생성자 함수|생성자 함수가 인스턴스에 프로토타입을 할당하기 위해 사용|


모든 객체의 __ proto __ 접근자 프로퍼티와 함수 객체의 prototype 프로퍼티는 `동일한 프로퍼티를 가리킨다.`


```javascript
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__); // true
```

![prototype](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/9c3871b2-72ba-4654-8767-569fe86300fd)


모든 prototype은 constructor 프로퍼티를 갖는데, prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다. 

위 예제에서 me 인스턴스는 프로토타입인 Person.prototype의 constructor를 상속받아 사용할 수 있다.
## 리터럴 표기법으로 생성한 객체의 생성자 함수와 프로토타입
생성자 함수로 만든 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 인스턴스를 생성한 생성자 함수와 연결된다. 
```javascript
const obj = new Object();
console.log(obj.constructor === Object);  // true
```
객체 리터럴로 객체를 만들면 프로토타입이 존재하기는 하지만 constructor 프로퍼티가 가리키는 생성자 함수와 이 객체를 생성한 생성자 함수가 반드시 일치하는 것은 아니다.
```javascript
// 객체 리터럴로 생성한 obj 배열 객체
const obj = {};

// 그런데 obj 객체의 생성자 함수는 Object 생성자 함수이다.
console.log(obj.constructor === Object);
```
Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산을 호출해서 빈 객체를 생성하는 점은 동일하지만, new.target의 확인이나 프로퍼티를 처리하는 등의 세부 방식이 다르다. 따라서 객체 리터럴로 생성된 객체는 Object 생성자 함수가 생성한 객체가 `아니다`. 
하지만 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하기 때문에 가상적인 생성자 함수를 갖기 때문에 위 예시와 같이 결과가 나온 것이다. 

따라서 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 항상 쌍으로 존재한다.

## 프로토타입 체인
자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Property]] 내부 슬롯의 참조를 따라 부모 객체인 프로토타입의 프로퍼티를 순차적으로 검색한다.

체인의 종점인 Object.prototype에도 없다면 undefined를 반환한다.

### 오버라이딩과 프로퍼티 섀도잉
- **오버라이딩**: 부모 클래스가 가진 메서드를 자식 클래스가 상속받아 재정의하여 사용하는 방식

```javascript
 const Person = (function () {
    function Person(name){
        this.name = name;
    }

    Person.prototype.sayHello = function () {
        console.log(`Hi! My name is ${this.name}.`);
    };

    return Person;
 }());

 const me = new Person('Lee');

 me.sayHello = function () {
    console.log(`Hey, My name is ${this.name}.`);
 };

 me.sayHello(); // Hey, My name is Lee.
```

![overriding](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/2261057c-0f4f-47ba-b393-c70943c2dbc5)

- 프로토타입이 소유한 프로퍼티: *프로토타입 프로퍼티*
- 인스턴스가 소유한 프로퍼티: *인스턴스 프로퍼티*

위 그림으로 설명하면 Person.prototype에 있는 sayHello는 프로토타입 메서드(상위)이고, me의 sayHello는 인스턴스 메서드(하위)이다.

me.sayHello();를 하게되면 Person.prototype.sayHello의 내용인 Hi! ...가 나오는 것이 아니라 재정의한 내용인 Hey, ... 가 나오게 된다. 

인스턴스 메서드 sayHello는 프로토타입 메서드인 sayHello를 덮어쓰는 것이 아니라 재정의해서 사용한 것인데, 이를 `오버라이딩`이라고 한다. 그리고 프로토타입 메서드인 sayHello는 가려진다. 이를 `프로퍼티 섀도잉`이라고 한다.

---

인스턴스 메서드 sayHello를 삭제하는 과정을 하단의 예시를 통해 알아보자.
```javascript
delete me.sayHello;

me.sayHello(); // Hi! My name is Lee.
```
인스턴스 메서드가 삭제되고 상위 클래스인 프로토타입 메서드가 호출된다.

프로토타입 프로퍼티를 변경 혹은 삭제하려면 프로토타입에 직접 접근해서 조작해야한다.
```javascript
delete Person.prototype.sayHello;

me.sayHello(); // TypeError: me.sayHello is not a function
```
---
### 프로토타입의 교체
- **생성자 함수**에 의한 교체
- 인스턴스에 의한 교체

프로토타입의 교체는 가능하지만 기존의 생성자 함수의 프로토타입 객체가 가지고 있던 **constructor 프로퍼티가 사라지고**, 이때 생성자 함수에 의해 생성된 `인스턴스가 상속받는 프로토타입 객체는` 생성자 함수의 프로토타입 객체가 아닌, 그 위의 최상위 객체이자 constructor 프로퍼티를 가지고 있는 `Object.prototype`이 된다.


즉 생성자 함수와 생성자 함수의 프로토타입 객체 간 연결이 끊어지게 될 뿐만 아니라 생성자 함수의 프로토타입 프로퍼티가 기존의 프로토타입 객체의 프로퍼티나 메서드를 상속받던 다른 인스턴스에게 영향을 끼칠 수 있기 때문에 프로토타입의 변경은 코드 테스트나 기존의 라이브러리의 확장을 위한 용도로나 사용하지, 일반적인 상황에서 프로토타입을 변경하는 것은 `권장하지 않는다`.

#### 생성자 함수에 의한 교체
```javascript
const Person = (function() {
    function Person(name){
        this.name = name;
    }
    Person.prototype = {
        // constructor: Person,
        sayHello() {
            console.log(`Hi! My name is ${this.name}`);
        }
    };
    return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person); // false
// 코드의 constructor 주석 해제하면 true

console.log(me.constructor === Object); // true
// 코드의 constructor 주석 해제하면 false
```

- 생성자 함수로 프로토타입을 교체하면 생성자 함수의 prototype 프로퍼티가 `교체된 프로토타입을 가리킨다.` 

<img width="229" alt="byconstructor" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/35df3da6-352e-4687-8c0e-8fabd901a74b">


#### 인스턴스에 의한 교체
인스턴스의 __ proto __ 접근자 프로퍼티를 통해 교체할 수 있다.
```javascript
function Person(name){
    this.name = name;
}

const me = new Person('Lee');

const parent = {
    sayHello(){
    console.log(`Hi! My name is ${this.name}`);
    }
};
// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// me.__proto__ = parent; 와 동일하게 동작한다.
me.sayHello(); // Hi! My name is Lee

console.log(me.constructor === Person); // false

console.log(me.constructor === Object); // true
```

- 인스턴스로 프로토타입을 교체하면 생성자 함수의 prototype 프로퍼티가 `교체된 프로토타입을 가리키지 않는다.`

<img width="255" alt="byinstance" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/61fdac4d-40cf-4577-8fd3-c9044d91f691">

## instanceof 연산자
이항 연산자이며, 우변의 피연산자가 함수가 아니면 TypeError 발생.
```
객체 instanceof 생성자 함수
```
우변의 prototype에 바인딩된 객체가 좌변의 프로토타입 체인 상에 **존재하면** -> true, 아니면 false.

프로토타입 교체로 인해 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도, 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 영향을 받지 않는다. 

## 직접 상속
- Object.create
    - new 연산자가 없어도 객체 생성 O
    - 프로토타입을 지정하면서 객체 생성 O
    - 객체 리터럴에 의해 생성된 객체도 상속 O

- 객체 리터럴 내부에서 __ proto __ (ES6)
## 정적 프로퍼티/메서드
생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다. 

```javascript
function Person(name){
    this.name = name;
}
Person.prototype.sayHello = function() {
    console.log(`Hi! My name is ${this.name}.`)
};

// 정적 프로퍼티
Person.staticProperty = 'static Property';
// 정적 메서드
Person.staticMethod = function(){
    console.log('staticMethod');
};

const me =  new Person('Lee');

Person.staticMethod(); // staticMethod

me.staticMethod(); // TypeError: me.staticMethod is not a function
```
## 프로퍼티 존재 확인
- in 연산자
- Reflect.has(object, key)
- Object.prototype.hasOwnProperty
### in 연산자
객체 내에 특정 프로퍼티의 존재 여부를 확인한다.
```javascript
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 */

key in object
```
확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 `상속받은 모든 프로토타입의 프로퍼티`를 확인한다. 상속받은 프로퍼티 키일 경우도 true를 반환한다.

ES6부터 in 연산자와 동일하게 동작하는 Reflect.has메서드를 사용할 수도 있다.
```javascript
Reflect.has(object, key)
```

### Object.prototype.hasOwnProperty
```javascript
객체의 식별자명.hasOwnProperty(key);
```
in 연산자와 다르게 인수로 전달받은 key가 객체 **고유의 프로퍼티 키일 경우에만 true**를 반환하고, 상속받은 프로퍼티 키일 경우에는 false를 반환한다.
## 프로퍼티 열거
- for ... in 문
- Object.keys/value/entries
### for ... in 문
객체의 모든 프로퍼티를 순회하면서 열거할 때 사용한다.
```javascript
for (변수 선언문 in 객체) {...}
```
순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 프로퍼티 어트리뷰트 **[[Enumerable]]이 true인 프로퍼티를 순회하며 열거한다.**

- [[Enumerable]]: 프로퍼티 열거 가능 여부를 나타내며 불리언값을 가진다.
그리고 for ... in문은 열거할 때 순서를 보장하지 않지만 대부분의 모던 브라우저는 순서를 보장하고 **숫자**(지만 사실은 **문자열**) 프로퍼티에는 정렬을 실시한다.

배열에는 for문 / for ... of / Array.prototype.forEach 메서드를 권장한다.
###  Object.keys/value/entries
객체 고유의 프로퍼티만 열거하기 위해서는 Object.keys/value/entries 메서드를 권장한다.