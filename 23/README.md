# 정규 표현식 (RegExp)

정규 표현식은 **문자열**을 대상으로 **패턴 매칭 기능**을 제공한다.

## 생성 방법

RegExp 객체는 `리터럴 표기법`과 `생성자`로써 생성할 수 있다.

```javascript
// 셋 모두 같은 정규 표현식을 생성한다

/ab+c/i;
new RegExp(/ab+c/, "i"); // 리터럴
new RegExp("ab+c", "i"); // 생성자
```

### 리터럴

![regexp](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/d632c688-d187-424a-bc8c-203d05296e8d)

```javascript
const target = "Is she alive?";
const regexp = /she/;

regexp.test(target); // true
```

리터럴 표기법은 `표현식을 평가할 때 정규 표현식을 컴파일`한다. 반복문 안에서 사용할 정규 표현식을 리터럴로 생성하면 정규 표현식을 매번 컴파일하지 않기 때문에 만약 변하지 않을 정규 표현식이라면 리터럴 표기법을 사용하는 것을 권장한다.

### RegExp 생성자 함수

생성자 함수를 통해 RegExp 객체를 동적으로 생성할 수 있다.

```javascript
const testContext = "string";
const regexp = new RegExp("pattern", "flags");

regexp.test(testcontext);
```

정규 표현식 객체의 생성자(new RegExp(''))를 사용하면 정규 표현식이 `런타임에 컴파일`된다. 패턴이 변할 가능성이 있는 정규 표현식의 경우 생성자 함수를 사용하는 것을 권장한다.

## 패턴

### 문자열 검색

```javascript
const target = "Is this all your idea? Is this what you want?";
const regexp = /Is/ig;

target.match(regexp); // ['Is', 'is', 'Is', 'is']
```

### 임의의 문자열 검색

```javascript
const target = "Is this all your idea? Is this what you want?";
const regexp = /.../g; // .의 갯수(n)만큼의 n자리 문자열을 매치한다.

target.match(regexp); // ['Is ', 'thi', 's a', 'll ', 'you', 'r i', 'dea', '? I', 's t', 'his', ' wh', 'at ', 'you', ' wa', 'nt?']
```

### 반복 검색

-   {m, n}: 최소 m번 ~ 최대 n번 반복되는 문자열

```javascript
const target = "X OOo oO XxxO O XO o";

// O이 최소 1번, 최대 2번 반복되는 문자열
const regexp = /O{1,2}/g;

target.match(regexp); // ['OO', 'O', 'O', 'O', 'O']
```

-   {m}: m번 반복되는 문자열

```javascript
const target = "X OOo oO XxxO O XO o";

// O이 2번 반복되는 문자열
const regexp = /O{2}/g;

target.match(regexp); // ['OO']
```

-   {m,}: 최소 m번 이상 반복되는 문자열
    -   **+는 {1,}과 같다.** /X+/ 라면 X가 1번 이상 반복된 문자열과 매치된다.

```javascript
const target = "X OOo oO XxxO O XO o";

// x가 최소 1번 이상 반복되는 문자열
const regexp = /X{1,}/g;
// const regexp = /X+/g;

target.match(regexp); // ['X', 'X', 'X']
```

### OR 검색

-   `|은 or`의 의미를 가진다. A|B 는 A or B를 의미한다.

```javascript
const target = "X OOo oO XxxO O XO o";
const regexp = /o|x/g;

target.match(regexp); // ['o', 'o', 'x', 'x', 'o']
```
- 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 같이 사용한다. 

```javascript
const target = "X OOo oO XxxO O XO o";
const regexp = /o+|x+/g;

target.match(regexp); // ['o', 'o', 'x', 'x', 'o']
```
- `[]내의 문자는 or`로 동작한다. 그 뒤에 +를 사용하여 앞선 패턴을 한번 이상 반복하게 한다.

```javascript
const target = "X OOo oO XxxO O XO o";
const regexp = /[ox]+/g;

target.match(regexp); // ['o', 'o', 'xx', 'o']
```
---

- `범위를 지정`해서 알파벳을 검색하는 방법 

```javascript
const target = "ag B2 DS 3357F c";
const regexp = /[A-Z]/g;

target.match(regexp); // ['B', 'D', 'S', 'F']
```

- `대소문자를 구별하지 않고` 알파벳을 검색하는 방법 

```javascript
const target = "ag B2 DS 3357F c";
const regexp = /[A-Za-z]/g;

target.match(regexp); // ['a', 'g', 'B', 'D', 'S', 'F', 'c']
```
---

- `숫자`를 검색하는 방법 

```javascript
const target = "ag 112,339 B2 DS 3357F c";
const regexp = /[0-9]+/g;

target.match(regexp); // ['112', '339', '2', '3357']
```


- 위에서 쉼표 때문에 숫자가 분리되었으므로 `쉼표까지 패턴에 포함시키는 방법`

```javascript
const target = "ag 112,339 B2 DS 3357F c";
const regexp = /[0-9,]+/g;

target.match(regexp); // ['112,339', '2', '3357']
```
---

- \d:  숫자를 의미한다. [0-9]
- \D: \d의 반대로 문자를 의미한다.
```javascript
const target = "AA B 1266V";
let regexp = /[\d,]+/g;

target.match(regexp); // ['1266']

regexp = /[\D,]+/g;

target.match(regexp); // ['AA B ', 'V']
```
- \w: 알파벳 & 숫자 & 언더스코어를 의미한다. [A-Za-z0-9_]
- \W: \w와 반대로 알파벳 & 숫자 & 언더스코어가 아닌 문자를 의미한다.

```javascript
const target = "A_^^A B 1266V _&&5";
let regexp = /[\w,]+/g;

target.match(regexp); // ['A_', 'A', 'B', '1266V', '_', '5']

regexp = /[\W,]+/g;

target.match(regexp); // ['^^', ' ', ' ', ' ', '&&']
```
---
### NOT 검색
- `[...]내의 ^는 not`의 의미를 가진다. [^0-9]는 숫자를 제외한 문자를 의미한다. 
    - \D는 [^0-9]와 같다.
    - \W는 [^A-Za-z0-9_]와 같다. 
```javascript
const target = "AA B AA3 A1 BBB2";
const regexp = /[^0-9]+/g;

target.match(regexp); // ['AA B AA', ' A', ' BBB']
```

### 시작 위치로 검색
- `[...] 밖의 ^는 문자열의 시작`을 의미한다.
```javascript
const target = "https://poiemaweb.com";
const regexp = /^https/i;

target.match(regexp); // ['https']
```
### 마지막 위치로 검색
- `$는 문자열의 마지막`을 의미한다.
```javascript
const target = "https://poiemaweb.com";
const regexp = /com$/i;

regexp.test(target); // true
```
## 플래그

플래그 사용은 옵션이며, 여러개 동시 사용이 가능하다.

`플래그를 사용하지 않은 경우,` 대소문자를 구별해서 대상을 탐색하고, 문자열 내에 패턴 매칭 대상이 여러개여도 `첫번째 매칭한 대상만 발견하고 탐색을 종료한다.`

|flag name|meaning|etc|
|:---:|:---:|---|
|i|ignore case|대소문자 구분없이 탐색|
|g|global|전역 탐색|
|m|multi line|여러 줄에 걸쳐 탐색|
|s|dot all|개행 문자가 .과 일치|
|u|unicode|유니코드, 패턴을 유니코드 코드 포인트의 시퀀스로 간주|
|y|sticky|대상 문자열의 현재 위치에서 탐색을 시작|



## RegExp 메서드

### RegExp.prototype.exec
인수로 받은 문자열을 탐색하고 나온 매칭 결과를 `배열`로 반환하며, 없는 경우 null을 반환한다.

g 플래그가 지정되어도 첫번째 매칭 결과만 반환한다.
```javascript
const target = 'Is this all your idea? Is this what you want?';
const regexp = /is/g;

regexp.exec(target); // ['is', index: 5, input: 'Is this all your idea? Is this what you want?', groups: undefined]
```

### RegExp.prototype.test

인수로 받은 문자열을 탐색하고 나온 매칭 결과를 `불리언값`으로 반환한다.

```javascript
const target = "Is this all your idea? Is this what you want?";
const regexp = /is/;

regexp.test(target); // true
```

### String.prototype.match

대상 문자열을 인수로 전달받은 정규 표현식으로 탐색하고 나온 매칭 결과를 `배열`로 반환한다.

**exec과 달리** g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```javascript
const target = "Is this all your idea? Is this what you want?";
const regexp = /is/g;

target.match(regexp); // ['is', 'is']
```
