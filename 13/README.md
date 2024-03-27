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