# this
this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**이다. 

this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다. 

## 함수 호출과 this 바인딩

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

|함수 호출 방식|this 바인딩|
|:---|:---|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가 생성할 인스턴스|
|Function.prototype.apply/call/bind 메서드에 의한 간접 호출|메서드에 첫번째 인수로 전달한 객체|





### 일반 함수 호출
- 일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다. (중첩 함수, 콜백 함수 포함)

```javascript
var x = 10;

const obj = {
    x: 1;
     foo() {
        console.log("foo's this: ", this); // {x: 1, foo: ƒ}
        console.log("foo's x: ", this.x); // 1

        function bar(){
            console.log("bar's this: ", this); // Window
             console.log("bar's x: ", this.x); // 10
        }
        bar();
     }
     };
    obj.foo();
```

- 메서드 내부 일반함수와 메서드의 this 바인딩을 일치시키는 방법
    - this 바인딩을 다른 변수에 할당해서 사용하기
    - 화살표 함수를 사용하면 함수 내부의 this는 상위 스코프의 this를 가리킨다.

### 메서드 호출
메서드명 앞에 찍은 마침표 앞에 기술한 객체가 바인딩된다.

```javascript
const user = {
    name: 'Wang',
    getName() {
        return this.name;
    }
};

console.log(user.getName()); // Wang
```
 
메서드 내부의 this는 자신를 소유한 객체가 아니라 자신을 호출한 객체에 바인딩된다.

![method](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/a3d74fde-8350-4262-bd84-b1238bd61e86)
```javascript
function Person(name){
    this.name;
}
// 프로토타입에 getName 메서드 추가
Person.prototype.getName = function (){
    return this.name;
};

const me = new Person('Lee');
console.log(me.getName()); // Lee

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName()); // Kim
```

### 생성자 함수 호출
this에 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.
```javascript
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return this.radius * 2;
    };
}

const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

const circle2 = Circle(15);
console.log(circle2); // undefined 
// 일반함수로 호출된 Circle에 반환문이 없으므로 암묵적 undefined 반환

console.log(radius); // 15
// 일반 함수 내부의 this는 전역 객체를 가리킨다.
```
circle2의 예시와 같이 new 연산자와 함께 호출하지 않으면 일반 함수로 동작한다. 

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply/call
    - 함수를 호출하면 첫번째 인수로 전달한 객체를 호출한 함수의 this에 바인딩한다.
    - 유사 배열 객체에 배열 메서드를 사용하는 경우에 주로 사용된다. 

- bind
    - apply/call과 달리 함수를 호출하지 않는다.
    - 첫번째 인수로 전달한 값을 this 바인딩이 교체된 함수를 새로 생성해서 반환한다.
    - 메서드의 this와 중첩 함수의 this가 불일치하는 문제를 해결하기 위해 주로 사용된다.