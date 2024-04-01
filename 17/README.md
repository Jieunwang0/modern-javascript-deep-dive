# 클래스

자바스크립트가 아닌 클래스 기반 객체지향 프로그래밍 언어를 학습하던 사용자들이 프로토타입 기반 프로그래밍을 더욱 빠르게 학습할 수 있도록 ES6에서 추가된 문법적 설탕(Syntactic Sugar)이다.

클래스는 생성자 함수와 유사하게 동작하지만 몇가지 차이가 있다.

-   호이스팅이 발생하지 않는 것처럼 동작한다.
-   클래스는 new 연산자와 함께 호출해야 한다. 없이 호출시 Error가 발생한다.
-   상속을 지원하는 super과 extends 키워드를 제공한다.
-   암묵적으로 strict mode가 지정되어서 실행되고, 해제가 불가하다.
-   constructor, 프로토타입 메서드, 정적 메서드 모두 [[Enumerable]]이 false이다. 즉, 열거되지 않는다.

## 클래스 선언문

클래스 선언문으로 선언한 클래스는 함수 선언문과 같이 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이때 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 constructor이다.
(생성자 함수 + 프로토 타입)은 한 쌍이라는 것을 잊지 말 것.

### 호이스팅

클래스는 클래스 정의 이전에 참조 불가하다.

```javascript
const Person = ""; // 호이스팅이 발생하지 않는다면 ""이 출력되어야 한다.

{
    console.log(Person); // ReferenceError: Cannot access 'Person' before initialization
    class Person {}
}
```

let, const로 선언한 변수처럼 호이스팅되는데, 이는 클래스 선언문 이전에 TDZ에 빠졌기 때문이다.

![temporal dead zone](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/fd6e2411-d741-4f69-b26b-662c7fd85c90)

### 인스턴스 생성

클래스는 반드시 new 연산자와 함께 호출되어야한다.

```javascript
class Person {}

const yesNEW = new Person();
console.log(yesNEW); // Person {}

const noNew = Person();
console.log(noNew); // TypeError: Class constructor Person cannot be invoked without 'new'
```

기명 클래스 표현식의 클래스 이름(MyClass)을 사용해서 인스턴스를 생성하면 참조 에러가 발생하기 때문에 클래스(Person)를 가리키는 식별자를 사용해야 한다.

```javascript
const Person = class MyClass {};

const me = new Person();

// MyClass는 클래스 몸체 내부에서만 유효한 식별자다.
const you = new MyClass(); // ReferenceError: MyClass is not defined

console.log(MyClass); // ReferenceError: MyClass is not defined
```

### 메서드

클래스의 몸체에서 정의할수 있는 3가지 메서드

-   constructor
-   프로토타입 메서드
-   정적 메서드

**constructor**

-   함수 내부에 1개의 constructor만 존재할 수 있다.
-   constructor 내부에 `return문 생략`은 필수이다.

**프로토타입 메서드**

-   클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과 다르게 `기본적으로 프로토타입 메서드가 된다.`
    -   생성자 함수에서는 명시적으로 프로토타입에 메서드를 추가해야 한다.
-   클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

**정적 메서드**

-   메서드에 static 키워드를 붙이면 정적 메서드-클래스 메서드-가 된다.
    -   생성자 함수에서는 명시적으로 생성자 함수에 메서드를 추가해야 한다.
-   인스턴스로 호출하는 게 아니라 클래스로 호출한다.

---

### < 클래스의 프로토타입 메서드와 정적 메서드의 차이 >

| -                                  | 프로토타입 메서드 | 정적 메서드 |
| ---------------------------------- | ----------------- | ----------- |
| 인스턴스의 프로토타입 체인에 존재? | O                 | X           |
| 호출 방법                          | 인스턴스          | 클래스      |
| 인스턴스 프로퍼티 참조 가능?       | O                 | X           |

인스턴스 프로퍼티를 참조하려면 프로토타입 메서드를 사용해야 한다.

**정적 메서드를 활용한 예시**
```javascript
class Square {
    static area(width, height) {
        return width * height;
    }
}

console.log(Square.area(5, 6)); // 30
```
**프로토타입 메서드를 활용한 예시**
```javascript
class Square {
   constructor(width, height){
    this.width = width;
    this.height = height;
   }
     area() {
        return this.width * this.height;
    }
}

const square = new Square(3, 4);
console.log(square.area()); // 12
```
---
### <클래스에서 정의한 메서드의 특징>
- function 키워드를 생략한 메서드 축약 표현을 사용한다.
- 객체 리터럴과 다르게 클래스에는 메서드를 정의할 때 콤마가 필요없다.
- 암묵적으로 strict mode가 실행된다.
- 열거할 수 없다. 
- 내부 메서드 [[Construct]]가 없는 non-constructor이다. new 연산자와 호출이 불가하다.

---

## 상속에 의한 클래스 확장

### extends
```javascript
// 수퍼(부모/베이스) 클래스
class Base {}

// 서브(자식/파생) 클래스
class Derived extends Base {}
```
### 동적 상속 가능
클래스 말고도 생성자 함수를 상속받아 클래스를 확장할 수 있다.
단, extends 키워드 앞에는 반드시 클래스가 와야한다.
```javascript
// 생성자 함수
function Base(a) {
    this.a = a;
}

class Derived extends Base{}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```
extends 키워드 뒤에는 클래스뿐만 아니라 **[[Construct]] 메서드를 가지며 함수 객체로 평가될 수 있는 모든 표현식이 올 수 있다.** 
```javascript
function Base1(){}

class Base2 {}

let condition = true;

// 조건을 걸어서 불린값에 따라 상속받을 대상을 결정하는 서브 클래스
class Derived extends (condition ? Base1 : Base2 ) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### super
- super을 호출하면 수퍼클래스의 constructor을 호출한다.
- super을 참조하면 수퍼클래스의 (프로토타입/정적) 메서드를 호출할 수 있다.

**super 호출**

- 수퍼 클래스의 constructor을 호출되어 서브 클래스의 constructor에 전달되기 때문에 초과된 인수만 작성하면 된다.
```javascript
// super class
class Base {
    constructor(a, b){
        this.a = a;
        this.b = b;
    }
}
// sub class
class Derived extends Base {
    constructor(a, b, c){
        super(a, b);
        this.c = c;
    }
}

const derived = new Derived(1, 2, 3);
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```
단, 주의사항이 몇가지 있다.
- 반드시 서브클래스의 constructor에만 호출한다. 
- 서브클래스의 constructor을 생략하지 않는 경우에는 반드시 super을 호출해야 한다.
- super을 호출하기 전에 this를 참조할 수 없다.
 
**super 참조**

- `서브 클래스의 프로토타입 메서드 내에서` super.sayHi는 `수퍼클래스의 프로토타입 메서드` sayHi를 가리킨다.
- 자신을 바인딩하고 있는 객체를 가리키기 위한 [[HomeObject]]를 가지는 `메서드 축약 표현`으로 정의된 함수만이 super 참조를 할 수 있다.
- **객체 리터럴**에도 super 참조가 가능하지만 메서드 축약 표현으로 정의된 함수여야 한다.
```javascript
// super class
class Base {
    constructor(name) {
        this.name = name;
    }
    sayHi(){
        return `Hey! ${this.name}!`
    }
}

// sub class
class Derived extends Base {
    sayHi(){
        return `${super.sayHi()} Where are you going?`;
    }
}

const derived = new Derived('Lecter');
console.log(derived.sayHi()); // Hey! Lecter! Where are you going?
```

- `서브 클래스의 정적 메서드 내에서` super.sayHi는 `수퍼클래스의 정적 메서드` sayHi를 가리킨다.
```javascript
// super class
class Base {
    static sayHi(){
        return 'Hi!';
    }
}

// sub class
class Derived extends Base {
    static sayHi() {
        return `${super.sayHi()} How are you?`
    }
}

console.log(Derived.sayHi()); // Hi! How are you?
```
### 표준 빌트인 생성자 함수 확장
extends 키워드 뒤에 [[Construct]] 메서드를 가지며 함수 객체로 평가될 수 있는 모든 표현식이 올 수 있다, 라고 앞에서 이야기 했는데 표준 빌트인 객체도 해당되므로 extends 키워드로 확장이 가능하다. 
```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
    uniq(){
        return this.filter((v, i, self) => self.indexOf(v) === i);
    }

    average(){
        return this.reduce((pre, cur) => pre + cur, 0) / this.length;
    }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75

// Array.prototype의 메서드 중 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다.
console.log(myArray.filter(v => v % 2) instanceof Array); // true
// 그래서 이렇게 메서드 체이닝이 가능하다.
console.log(myArray.filter(v => v % 2).uniq().average()); // 2
```