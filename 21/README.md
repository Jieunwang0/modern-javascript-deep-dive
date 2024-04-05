# Math
생성자 함수가 아니기 때문에 정적 프로퍼티와 메서드만 제공한다.

## Math 프로퍼티

### Math.PI
원주율 파이(π) 값을 반환한다.
```javascript
Math.PI; // 3.141592653589793
```
## Math 메서드

### Math.abs
Math.abs 메서드는 `인수로 전달된 숫자의 절대값`을 반환한다. 
- 절대값은 반드시 0 아니면 양수여야 한다.

```javascript
Math.abs(-1); // 1
Math.abs('-1'); // 1

Math.abs(''); // 0
Math.abs([]); // 0
Math.abs(false); // 0
Math.abs(null); // 0

Math.abs(undefined); // NaN
Math.abs({}); // NaN
Math.abs('hi'); // NaN
Math.abs(); // NaN
```

---

### Math.round
Math.round 메서드는 인수로 전달된 숫자의 소수점 이하를 `반올림한 정수`를 반환한다.

```javascript
Math.round(1.4); // 1
Math.round(1.8); // 2

Math.round(-1.4); // -1
Math.round(-1.8); // -2

Math.round(1); // 1
Math.round(); // NaN
```
### Math.ceil
Math.ceil 메서드는 인수로 전달된 숫자의 소수점 이하를 `올림한 정수`를 반환한다.

```javascript
Math.ceil(1.4); // 2
Math.ceil(1.8); // 2

Math.ceil(-1.4); // -1
Math.ceil(-1.8); // -1

Math.ceil(1); // 1
Math.ceil(); // NaN
```

### Math.floor
Math.floor 메서드는 인수로 전달된 숫자의 소수점 이하를 `내림한 정수`를 반환한다. 
```javascript
Math.floor(1.9); // 1
Math.floor(9.1); // 9

Math.floor(-1.9); // -2
Math.floor(-9.1); // -10

Math.floor(); // NaN
```
---

### Math.sqrt
Math.sqrt 메서드는 `인수로 전달된 숫자의 제곱근`을 반환한다.
```javascript
Math.sqrt(16); // 4
Math.sqrt(-16); // NaN
Math.sqrt(2); // 1.4142135623730951
Math.sqrt(1); // 1
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```
### Math.random
Math.random 메서드는 `임의의 난수`를 반환한다. 
- 반환하는 난수는 0에서 1 미만의 실수다.

```javascript
// Math.random을 사용해서 1~10 범위의 랜덤 정수를 얻는 법

const random = Math.floor((Math.random()* 10) + 1);

console.log(random);
```
<img width="418" alt="random" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/754e28d2-5427-4775-87a9-1673160d2d95">

### Math.pow

```javascript
Math.pow(base, exponent)

base ** exponent;
```
Math.pow 메서드는 `밑(base)을 지수(exponent)로 거듭제곱한 결과`를 반환한다.

대신 ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다. 
```javascript
Math.pow(2, 8); // 256
Math.pow(2, -4); // 0.0625

// 지수 연산자
2 ** 8; // 256
Math.pow(2, 8); // 256

2 ** 2 ** 2; // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

---

### Math.max
Math.max 메서드는 `전달받은 인수 중에서 가장 큰 수`를 반환한다.
- 인수가 전달되지 않은 경우 -Infinity를 반환.

```javascript
Math.max(1); // 1
Math.max(1145, 78, 13, 32); // 1145
Math.max(); // -Infinity
```

### Math.min
Math.min 메서드는 `전달받은 인수 중에서 가장 작은 수`를 반환한다. 
- 인수가 전달되지 않은 경우 Infinity를 반환.
```javascript
Math.min(1); // 1
Math.min(1145, 78, 13, 32); // 13
Math.min(); // Infinity
```

#### 배열의 요소 중 최대값/최소값
배열을 인수로 받아서 배열의 요소 중 최대값/최소값을 구하려면 **Function.prototype.apply 메서드**나 **스프레드 문법**을 사용해야 한다.
```javascript
// Math.max
Math.max.apply(null, [1, 1145, 78, 32]); // 1145
Math.max(...[1, 1145, 78, 32]); // 1145

// Math.min
Math.min.apply(null, [1145, 78, 13, 32]); // 13
Math.min(...[1145, 78, 13, 32]); // 13
```

---