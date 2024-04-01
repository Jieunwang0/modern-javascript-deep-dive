# ES6 함수의 추가 기능
## 함수의 구분
ES6 이전의 모든 함수는 callable이면서 constructor이다.
```javascript
var foo = function() {}

foo(); // undefined

new foo(); // foo {}

var obj = {foo: foo};
obj.foo(); // undefined
```
사용 목적에 따른 명확한 구분이 없어서 실수를 유발할 가능성이 있고 성능에 좋지 않기 때문에 ES6에서 함수를 사용 목적에 따라 3가지 종류로 구분했다.

|ES6 함수의 구분|constructor|prototype|super|arguments|
|---|:---:|:---:|:---:|:---:|
|일반 함수|O|O|X|O|
|메서드|X|X|O|O|
|화살표 함수|X|X|X|X|


## ES6 메서드
ES6에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.
- 인스턴스를 생성할 수 없는 **non-constructor**이다.
- 그래서 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
```javascript
const obj = {
    x: 1,
    foo() {
        return this.x;
    },
    bar: function() {
        return this.x;
    }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

// foo()는 ES6 메서드라 non-constructor
new obj.foo(); // TypeError: obj.foo is not a constructor
// bar()는 constructor인 일반 함수
new obj.bar(); // bar {}

obj.foo.hasOwnProperty('prototype'); // false
obj.bar.hasOwnProperty('prototype'); // true
```
- 표준 빌트인 함수가 제공하는 프로토타입의 메서드와 정적 메서드는 모두 **non-constructor**이다.
```javascript
String.prototype.toUpperCase.prototype; // undefined
String.fromCharCode.prototype; // undefined


Number.prototype.toFixed.prototype; // undefined
Number.isFinite.prototype; // undefined

Array.prototype.map.prototype; // undefined
Array.from.prototype; // undefined
```
- 메서드 축약 표현으로 정의된 ES6 매서드는 내부 슬롯 [[HomeObject]]을 갖는다.  **super** 키워드를 사용할 수 있다.
## 화살표 함수
- 인스턴스를 생성할 수 없는 non-constructor이다.
- 중복된 매개변수 이름을 선언할 수 없다.
- 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
    - 화살표 함수 내에서 this, arguments, super, new.target을 참조하려 하면 스코프 체인을 통해 `상위 스코프`의 this, arguments, super, new.target을 참조한다.
- 클래스 필드에 할당한 화살표 함수는 인스턴스 메서드가 되기 때문에 메서드를 정의할 때는 메서드 축약 표현으로 정의해야 한다.

## Rest 파라미터
- 함수에 전달된 인수들의 목록을 배열로 받는다.
- 반드시 `마지막 파라미터`여야 한다.
```javascript
function foo(param1, param2 ...rest){
    console.log(param1); // 1
    console.log(param2); // 2
    console.log(rest); // (3) [3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```
- 함수 객체의 length 프로퍼티에 영향을 주지 않는다.
```javascript
function bar(...rest) {}
console.log(bar.length); // 0

function foo(x, ...rest) {}
console.log(foo.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```
- 화살표 함수로 가변 인자 함수를 구현해야할 때는 반드시 Rest 파라미터를 사용해야 한다.

### arguments
arguments 객체는 배열이 아닌 **유사 배열 객체**이므로 배열 메서드를 사용하려면 배열로 변환 후 사용해야하는 불편함이 있었다.
ES6에서는 Rest 파라미터를 사용해서 가변 인자 함수의 인수 목록을 `배열로 직접 받기 때문에` 이 번거로움을 피할 수 있게 되었다.
```javascript
function sum (...args){
    return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1,2,3,4,5)); // 15
```

하지만 **일반 함수와 ES6 메서드**는 Rest 파라미터와 arguments 객체를 모두 사용할 수 있는 반면, **화살표 함수**는 arguments 객체를 갖지 않기 때문에 반드시 `Rest 파라미터`를 사용해야 한다.

## 매개변수 기본값
매개변수에 인수가 전달되었는지 확인해서 인수가 전달되지 않은 경우 **매개변수에 기본값**을 할당하여 **의도치 않은 결과를 방어할 필요가 있다.**
- 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.
```javascript
function sum(x = 0, y = 0) {
    return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```
- 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.
```javascript
function logName(x = 'Lee') {
   console.log(x);
}

logName(); // Lee
logName(undefined); // Lee

logName('Jay'); // Jay
logName(null); // null
```
- Rest 파라미터에는 기본값을 지정할 수 없다.
- 함수 객체의 length 프로퍼티와 arguments 객체에 영향을 주지 않는다.