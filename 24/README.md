# String
빌트인 객체이면서 생성자 함수.


## 생성자 함수

#### String 생성자 함수에 인수로 `아무것도 전달하지 않았을 경우`

-  [[StringData]] 내부 슬롯에 문자열을 할당할 String 래퍼 객체인 [[PrimitiveValue]] 프로퍼티에 빈 문자열이 할당된다.

<img width="300" alt="스크린샷 2024-04-08 오전 1 25 23" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/1cea02ba-5fb5-4284-abc2-c69ae58e4fe0">


---

#### String 생성자 함수에 인수로 `문자열을 전달하면서 new 연산자를 함께 호출`했을 경우

 -   [[PrimitiveValue]] 프로퍼티에 인수로 전달된 문자열이 할당된다.

<img width="300" alt="스크린샷 2024-04-08 오전 1 26 19" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/7856f467-e6e3-455d-a79a-63c6d6477cbe">


---

#### String 생성자 함수에 인수로 `문자열이 아닌 값을 전달했을 경우`

-  [[PrimitiveValue]] 프로퍼티에 문자열로 강제 변환한 값을 할당한다.

<img width="300" alt="스크린샷 2024-04-08 오전 1 27 03" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/51d72be0-12d0-48a6-87b3-fa799066254b">
<img width="290" alt="스크린샷 2024-04-08 오전 1 27 25" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/60168a2e-81c1-44fa-97fe-f5df025ce1a7">

---

#### String 생성자 함수에 `new 연산자를 호출하지 않았을 경우`

- String 인스턴스가 아닌 문자열을 반환한다. (명시적 타입 변환)

<img width="300" alt="notstringinstance" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/cf6f9b63-dba8-4bb5-b293-6fdbe77cdeeb">



## length 프로퍼티
- length 프로퍼티는 문자열의 문자 갯수를 반환한다. 
- String 래퍼 객체는 `유사 배열 객체`이다.
```javascript
'Hello'.length; // 5
```
## String 메서드
배열에는 원본 배열을 직접 변경하는 메서드 ( mutator method )와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드 ( accessor method )가 있다.

그런데 String은 immutable한 **원시값**이기 때문에, String 객체에는 직접 변경하는 메서드가 없고 String 래퍼 객체도 **항상 새로운 문자열을 반환한다.**

### String.prototype.indexOf

- 대상 문자열에서 인수로 전달받은 **문자열**의 `첫번째 인덱스를 반환한다.`
- 검색에 실패하면 -1을 반환한다.

```javascript
const str = 'Hello World';

str.indexOf('o'); // 4

str.indexOf('z'); // -1

str.indexOf('or'); // 7
```
- 2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다. 

```javascript
str.indexOf('l', 7); // 9
```
- 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용하다.
- 이때 String.prototype.indexOf 대신 String.prototype.includes를 사용하면 **가독성**이 더 좋다. 
```javascript
// indexOf
if (str.indexOf('Hello')) {
    // str에 'Hello'가 존재한다면 처리할 내용
};

// includes
if (str.includes('Hello')) {
     // str에 'Hello'가 존재한다면 처리할 내용
};
```

### String.prototype.includes

- 대상 문자열에서 인수로 전달받은 **문자열이 존재하는지**의 여부를 `불리언값으로 반환한다.`

```javascript
const str = 'Hello World';

str.includes('o'); // true
str.includes('z'); // false
str.includes(' '); // true
str.includes(); // false
```

- 2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다. 

```javascript
str.includes('l', 7); // true
```

### String.prototype.search

- 대상 문자열에서 인수로 전달받은 **정규 표현식과 매치되는** 문자열의 `첫번째 인덱스를 반환한다.`
- 검색에 실패하면 -1을 반환한다.

```javascript
const str = 'Hello World';

str.search(/o/); // 4
str.search(/x/); // -1
```

### String.prototype.startsWith

- 대상 문자열에서 인수로 **전달받은 문자열로 시작하는지** 여부를 `불리언값으로 반환한다.`

```javascript
const str = 'Hello World';

str.startsWith('o'); // false
str.startsWith('z'); // false
str.startsWith('Hell'); // true

```

- 2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다. 

```javascript
// index 5부터 검사를 시작했을 때 시작점이 공백인지 여부를 체크
str.startsWith(' ', 5); // true
```

### String.prototype.endsWith

- 대상 문자열에서 인수로 **전달받은 문자열로 끝나는지** 여부를 `불리언값으로 반환한다.`

```javascript
const str = 'Hello World';

str.endsWith('z'); // false
str.endsWith('ld'); // true
```

- 2번째 인수로 검색할 문자열의 길이를 지정할 수 있다. 

```javascript
// 문자열 str의 처음부터 다섯 자리('Hello')까지 검사했을 때 마지막이 lo인지 체크
str.endsWith('lo', 5); // true
```
### String.prototype.charAt
- 대상 문자열에 인수로 전달받은 인덱스를 검색해서 **인덱스에 해당하는** `문자를 반환한다.`
- 문자열의 길이를 넘어선 인덱스를 전달한 경우 빈 문자열을 반환한다.
```javascript
const str = 'Hello World';

for (let i = 0; i < str.length; i++) {
    console.log(str.charAt(i)); // H e l l o   W o r l d
}

str.charAt(20); // ''
```
### String.prototype.substring
- `인덱스 a부터 인덱스 b 이전까지의 문자를 반환`한다. **(a 이상 b 미만)**
- b는 생략이 가능한데, 이럴 경우에는 인덱스 a부터 문자열 마지막까지 반환한다. 

```javascript
String.substring(a, b);
```
- a > b인 경우 두 인수는 교환된다.
- 인수 < 0 이거나 NaN인 경우 0으로 취급
- 인수 > str.length의 경우 인수 = str.length로 취급된다.

```javascript
const str = 'Hello World';

str.substring(2, 6); // 'llo '

str.substring(8, 5); // ' Wo'

str.substring(-2); // 'Hello World'

str.substring(20); // ''

str.substring(100); // ''
```
String.prototype.indexOf를 같이 사용하면 특정 문자열을 기준으로 검색할 수 있다.
```javascript
// 시작부터 공백 전까지의 문자열을 검색
str.substring(0, str.indexOf(' ')); // 'Hello'
```

### String.prototype.slice
- substring 메서드와 동일하게 동작한다. 여기에 음수인 인수를 전달할 수 있다는 차이가 있다. 
- 음수를 전달하면 대상 문자열의 **마지막에서부터 index자리까지의 문자열**을 잘라내서 반환한다.
```javascript
const str = 'Hello World';

str.slice(2); // 'llo World'
str.slice(-7); // 'o World'

str.slice(0, 2); // 'He'

str.slice(-5, -2); // 'Wor'
str.slice(-2, -1); // 'l'
```
### String.prototype.toUpperCase
- 대상 문자열을 모두 `대문자로 변경`한 문자열을 반환한다.
```javascript
const str = 'Hello World';

str.toUpperCase(); //'HELLO WORLD'
```
### String.prototype.toLowerCase
- 대상 문자열을 모두 `소문자로 변경`한 문자열을 반환한다.

```javascript
const str = 'Hello World';

str.toLowerCase(); // 'hello world'
```
### String.prototype.trim
- 대상 문자열의 `앞뒤에 있는 공백을 제거`한 문자열을 반환한다.
```javascript
let str = '         foo         ';

console.log(str.length); // 21

str = str.trim(); // 'foo'
console.log(str.length); // 3
```

- trimStart()로 앞부분의 공백만 제거가 가능하다.(다른 이름으론 trimLeft())
```javascript
let str = '         foo         ';
console.log(str.length); // 21

str = str.trimStart(); // 'foo         '
console.log(str.length); // 12
```

- trimEnd()로 뒷부분의 공백만 제거가 가능하다.(다른 이름으론 trimRight())
```javascript
let str = '         foo         ';
console.log(str.length); // 21

str = str.trimEnd(); // '         foo'
console.log(str.length); // 12
```

### String.prototype.repeat
- 대상 문자열을 인수로 전달받은 `정수만큼 반복해서 연결한 문자열을 반환한다.`
- 인수를 생략하면 기본값 0
- 인수가 0이면 빈 문자열을 반환한다.
- 인수가 음수면 RangeError를 발생시킨다.
```javascript
const str = 'abc';

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(3); // abcabcabc

str.repeat(2.2); // abcabc, (2.2 -> 2)소수점자리는 버린다.
str.repeat(2.6); // abcabc, (2.6 -> 2)소수점자리는 버린다.
str.repeat(-1); // RangeError: Invalid count value: -1
```
### String.prototype.replace
- 첫번째 인수로 전달받은 **문자열/정규 표현식**을 검색해서 두번째 인수로 전달한 문자열로 `치환한 문자열을 반환한다.`
- 검색된 문자열이 여러개일 경우 첫번째로 검색된 문자열만 치환한다.
```javascript
// 인수가 문자열
const str = 'Hello World';

str.replace(' ', '@'); // 'Hello@World'
str.replace('l', 'L'); // 'HeLlo World'

// 인수가 정규 표현식
str.replace('/hello/ig', 'SpongeBob'); // 'SpongeBob World'
```
- 특수한 **교체 패턴**을 사용할 수 있다. ($& => 검색된 문자열을 의미한다. 여기서 교체 패턴에 대한 설명은 패스)

```javascript
str.replace('world', '<strong>$&</strong>');
```

### String.prototype.split
- 첫번째 인수로 전달한 **문자열/정규 표현식**을 검색해서 문자열을 구분하고, `분리된 문자열로 이루어진 배열을 반환한다.`
- 인수가 빈 문자열이면 각 문자를 모두 분리한다.
- 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```javascript
const str = 'How are you?';

str.split(); // ['How are you?']
str.split(''); // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', '?']

str.split(' '); // ['How', 'are', 'you?']
str.split(/\s/); // ['How', 'are', 'you?']
// \s는 공백 문자를 의미한다. 
```
- 두번째 인수로 **배열의 길이**를 지정할 수 있다.
```javascript
str.split(' ', 1); // ['How']
```

