# Number

-   Number 생성자 함수에 인수로 `아무것도 전달하지 않았을 경우`

    -   [[PrimitiveValue]] 프로퍼티에 0이 할당된다.

      <img width="300" alt="스크린샷 2024-04-04 오후 3 15 51" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/68127ce6-ba47-455d-90e6-d9365cdaa64a">

---

-   Number 생성자 함수에 인수로 `숫자를 전달하면서 new 연산자를 함께 호출`했을 경우

    -   [[PrimitiveValue]] 프로퍼티에 인수로 전달된 숫자가 할당된다.

      <img width="300" alt="스크린샷 2024-04-04 오후 3 19 21" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/23b2c03a-5ed1-481a-b165-3c3a5b9e4a17">

---

-   Number 생성자 함수에 인수로 `숫자가 아닌 값을 전달했을 경우`

    -   숫자로 변환할 수 있다면 숫자로 변환한 값을 [[PrimitiveValue]] 프로퍼티에 할당하고, 숫자로 변환할 수 없는 값이라면 NaN을 [[PrimitiveValue]] 프로퍼티에 할당한다.

      <img width="300" alt="스크린샷 2024-04-04 오후 3 20 17" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/0c0fb19d-e7c4-444b-8542-d6b4cc78c70b">

## Number 프로퍼티

### Number.EPSILON

정수는 2진법으로 오차없이 저장 가능하지만 부동 소수점을 표현하기 위해 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환하면 무한 소수가 되어 계산에 미세한 오차가 발생한다.

```
0.1 + 0.2; // 0.30000000000000004

0.1 + 0.2 === 0.3; // false
```

Number.EPSILON은 부동소수점으로 인해 나타나는 오차를 해결하기 위해 사용한다. 1과 1보다 더 큰 숫자 중에서 가장 작은 숫자와의 차이와 같고, 약 2.220446049250313e-16이다.

```javascript
function isEqual(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3); // true
```

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수값. 이보다 더 큰 숫자는 Infinity이다.

```javascript
Number.MAX_VALUE; // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

### Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수값. 이보다 더 작은 숫자는 0이다.

```javascript
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0; // true
```

### Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값.

```javascript
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값.

```javascript
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

### Number.POSITIVE_INFINITY

양의 무한대를 나타내는 숫자값 Infinity와 같다.

```javascript
Number.POSITIVE_INFINITY; // Infinity
```

### Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 숫자값 -Infinity와 같다.

```javascript
Number.NEGATIVE_INFINITY; // -Infinity
```

### Number.NaN

숫자가 아님(Not-a-Number)을 나타내는 숫자값이다. window.NaN과 같다.

```javascript
Number.NaN; // NaN
```

## Number 메서드

### Number.isFinite

Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수인지 검사해서 `boolean 값으로 반환`한다.

-   인수를 암묵적 타입 변환하지 않는다.

```javascript
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

// 인수가 NaN이면 언제나 false
Number.isFinite(NaN); // false
```

빌트인 메서드 isFinite는 검사 전에 인수를 숫자로 암묵적 타입 변환하지만, Number.isFinite는 암묵적 타입 변환을 하지 않는다는 차이점이 있다.

```javascript
isFinite(null); // true, null은 0으로 암묵적 타입 변환을 한다.

Number.isFinite(null); // false
```

### Number.isIntegar

Number.isIntegar 정적 메서드는 인수로 전달된 숫자가 정수인지 검사해서 `boolean 값으로 반환`한다.

-   인수를 암묵적 타입 변환하지 않는다.

```javascript
Number.isIntegar(0); // true
Number.isIntegar(10); // true
Number.isIntegar(-10); // true

// 소수임
Number.isIntegar(0.1); // false

// 암묵적 타입 변환X
Number.isIntegar("777"); // false
Number.isIntegar(false); // false

// 무한수는 정수가 아님
Number.isIntegar(Infinity); // false
```

### Number.isNaN

Number.isNaN 정적 메서드는 인수로 전달된 숫자가 NaN인지 검사해서 `boolean 값으로 반환`한다.

-   인수를 암묵적 타입 변환하지 않는다.

```javascript
Number.isNaN(NaN); // true
```

빌트인 메서드 isNaN은 암묵적 타입 변환이 수행되지만 Number.isNaN은 암묵적 타입 변환이 이루어지지 않기 때문에 숫자가 아닌 인수라면 false가 반환된다.

```javascript
isNaN(undefined); // true, undefined는 NaN으로 암묵적 타입변환

Number.isNaN(undefined); // false
```

### Number.isSafeInteger

Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 결과를 `boolean 값으로 반환`한다.

-   안전한 정수의 범위는 -(2 ** 53 - 1)에서 2 ** 53 - 1까지의 모든 정수를 의미한다.

-   인수를 암묵적 타입 변환하지 않는다.

```javascript
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true

Number.isSafeInteger(10000000000000001); // false
Number.isSafeInteger(0.1); // false
Number.isSafeInteger("111"); // false
Number.isSafeInteger(false); // false
Number.isSafeInteger(Infinity); // false
```

### Number.prototype.toExponential

toExponential 메서드는 숫자를 **\*지수 표기법**으로 변환해서 문자열로 반환한다.

**\*지수 표기법**: 아주 크거나 작은 숫자를 표기할 때 주로 사용하며, e(Expotential) 앞의 숫자에 10의 n승을 곱하는 형식으로 나타내는 형식이다. 인수에 소수점 이하로 표현할 자릿수를 전달할 수 있다.

```javascript
(44.1233).toExponential(); // '4.41233e+1'
(44.1233).toExponential(0.0002); // '4e+1'
(44.1233).toExponential(4); // '4.4123e+1'
```

그룹 연산자 (괄호) 없이 숫자리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 숫자 뒤의 .의 의미가 모호하기 때문에 에러가 발생한다.

```javascript
44.toExponential(); // SyntaxError: Invalid or unexpected token

(44).toExponential(); // '4.4e+1'
```

44 뒤의 . 뒤에 숫자가 이어지므로 부동 소수점 숫자의 소수 구분 기호로 해석되고, 두번째 .은 프로퍼티 접근 연산자로 해석되기 때문에 연산이 정상적으로 수행된다. 하지만 가독성이 떨어지기 때문에 `그룹 연산자`를 사용하거나 `숫자 뒤에 공백`을 주는 방식을 사용할 수 있다.

```javascript
(44.1233).toExponential(); // '4.41233e+1'
(44.1233).toExponential(); // '4.41233e+1'
```

### Number.prototype.toFixed

toFixed 메서드는 숫자를 반올림하여 문자열로 반환한다.

-   소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.
-   인수를 생략할 경우 기본값 0.

```javascript
(0.7235662).toFixed(); // 1
(0.7235662).toFixed(1); // 0.7
(0.7235662).toFixed(3); // 0.724
(0.7235662).toFixed(5); // 0.72357
```

### Number.prototype.toPrecision

toPrecision 메서드는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 반환한다.
인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 반환한다.

-   전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다.
-   인수를 생략하면 기본값 0.

```javascript
(1234.5678).toPrecision(); // '1234.5678'
(1234.5678).toPrecision(1); // '1e+3'
(1234.5678).toPrecision(3); // '1.23e+3'
(1234.5678910111213).toPrecision(15); // '1234.56789101112'
```

### Number.prototype.toString
toString 메서드는 숫자를 문자열로 변환하여 반환한다. 
- 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 10진법.

```javascript
(16).toString(); // '16'
(16).toString(2); // '10000'
(16).toString(4); // '100'
(16).toString(8); //'20'
```
