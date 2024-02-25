# 원시 값과 객체 비교
 원시 타입(primitive type) 값은 변경 불가능한 값(readOnly)이고, 객체 타입(object/reference type) 값은 변경 가능한 값이다.
## 원시 값
- 원시 값이 변경 불가능하다는 말은 원시 값 자체를 변경할 수 없다는 의미지, 변수 값을 변경할 수 없다는 말이 아니다.
- 원시 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장된다.
- 원시 값을 할당한 변수를 다른 변수에 할당하면 ```원본```이 복사되어 전달된다. (깊은 복사)
#### 값에 의한 전달
아래의 예시에선 scoreA와 scoreB 모두 80을 가지고 있지만 같은 메모리 주소의 80을 가지고 있는 게 아니다. 하지만 사본(scoreB)이 원본(scoreA)의 ```실제 값```을 가져와서 새로운 메모리 공간에 할당하고 가리키기 때문에 ```깊은 복사```라고 한다.
```javascript
var scoreA = 80;

var scoreB = scoreA;
console.log(scoreA); // 80
console.log(scoreB); // 80

scoreA = 100;
console.log(scoreA); // 100
console.log(scoreB); // 80
```
재할당 이전의 원시 값을 변경하는 게 아니라 
```새로운 메모리 공간을 확보```하고 ```재할당한 값을 저장```한 후 변수가 ```참조하던 메모리 공간의 주소를 변경```한다. 따라서 값에 의해 전달된 값은 다른 메모리 공간에 저장된 별개의 값이 된다.
```javascript
var a;

a = 10; // 할당

a = 23; // 재할당, 10은 
```


### 문자열과 불변성
문자열은 유사 배열 객체면서 이터러블이라서 배열과 유사하게 각 문자에 접근할 수 있다.
- 이터러블(iterable) : 순회할 수 있는 객체 (for..of을 사용할 수 있는 객체)
```javascript
var str = 'Hello';

console.log(str[1]); // e, 유사 배열이므로 인덱스에 접근할 수 있다.
console.log(str.length); // 5, 원시값인 문자열이 객체처럼 동작한다.
```
하지만 원시 타입의 불변성은 여전하기 때문에 문자열의 일부만 교체할 수는 없고, 새로운 문자열로 바꾸는 것만이 가능하다.
 ```javascript
var str = 'Hello';

str[1] = 'a';
console.log(str); // "Hello", 애러가 발생하지 않고 무시된다.

str = 'Wonka!';
console.log(str); // "Wonka!", 새롭게 할당이 가능하다.
```
## 객체값
- 객체 값을 변수에 할당하면 변수에는 참조 값이 저장된다.
- 객체 값을 할당한 변수를 다른 변수에 할당하면 원본의 ```참조 값```이 저장된다. (얕은 복사)
#### 참조에 의한 전달
할당 연산자(=)를 통해 변수에 객체 값을 가진 변수를 할당할 때는 객체의 참조값이 저장된다. 이는 ```얕은 복사```라고 할 수 있다.
```javascript
var user = {
    name: 'wang'
};

var copy = user;

console.log(user === copy); // true
```
여기서는 원본 user가 참조하는 { name:'wang'} 값의 주소를 사본 copy가 가리키게 되기 때문에 두 개의 식별자(user, copy)가 하나의 객체를 가리키게 된다. 

따라서 ```두 개의 식별자가 동일한 객체를 공유하기 때문에``` 어느 한쪽에서 객체를 변경하면 서로 영향을 받는다.
```javascript
var user = {
    name: 'wang'
};

var copy = user;

console.log(user === copy); // true

copy.name = 'jieun';
user.address = 'Suwon';

console.log(user); // { name: "jieun", address: "Suwon" }
console.log(copy); // { name: "jieun", address: "Suwon" }
```

그리고 아래의 예시처럼 객체 타입은 변경 가능하기 때문에 값을 재할당 없이 직접 변경할 수 있다. 이때 객체를 할당한 변수의 참조값은 ```변경되지 않는다.```
```javascript
var user = {
    name: 'wang'
};

// 프로퍼티 갱신
user.name = 'jieun';

// 프로퍼티 동적 생성
user.age = 25;

console.log(user); // { name: "jieun", age: 25 };
```
#### 얕은 복사와 깊은 복사 (객체의 경우)
```javascript
const o = { x: { y: 1 } };

// 얕은 복사, 스프레드 문법
const copy1 = { ...o };
console.log(copy1 === o); // false
console.log(copy1.x === o.x); // true, 참조값이 같기 때문에 true.

// 깊은 복사, lodash의 cloneDeep
const _ = require('lodash');

const copy2 = _.cloneDeeop(o);
console.log(copy2 === o); // false
console.log(copy2.x === o.x); // false, 원본을 복사해서 새로운 위치에 새로 할당했기 때문에 별개의 값이 된다. 
```