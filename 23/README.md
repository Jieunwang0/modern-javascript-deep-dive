# 정규 표현식 (RegExp)

정규 표현식은 **문자열**을 대상으로 **패턴 매칭 기능**을 제공한다.

## 생성 방법

RegExp 객체는 `리터럴 표기법`과 `생성자`로써 생성할 수 있습니다.

-   리터럴 표기법의 매개변수는 두 빗금으로 감싸야 하고 따옴표 X
-   생성자 함수의 매개변수는 빗금으로 감싸지 않고 따옴표 O

```javascript
// 셋 모두 같은 정규 표현식을 생성한다

/ab+c/i;
new RegExp(/ab+c/, "i"); // 리터럴
new RegExp("ab+c", "i"); // 생성자
```

### 리터럴

![regexp](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/d632c688-d187-424a-bc8c-203d05296e8d)

정규 표현식 리터럴은 **패턴**과 **선택적으로 사용할 수 있는 플래그**로 구성된다.

```javascript
const target = 'Is she alive?';
const regexp = /she/i;

regexp.test(target); // true

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하고 매칭 결과를 불리언 값으로 반환한다
```


리터럴 표기법은 `표현식을 평가할 때 정규 표현식을 컴파일`한다. 반복문 안에서 사용할 정규 표현식을 리터럴로 생성하면 정규 표현식을 매번마다 컴파일하지 않기 때문에 만약 정규 표현식이 변하지 않는다면 리터럴 표기법을 사용하는 것을 권장한다.

### RegExp 생성자 함수

```javascript
// 생성자 함수를 통한 긴 문법
regexp = new RegExp("pattern", "flags");

// 생성자 함수를 통한 짧은 문법
new RegExp("pattern", "flags");
```

생성자 함수를 통해 RegExp 객체를 동적으로 생성할 수 있다.

```javascript
const testContext = '';
const regexp = new RegExp("pattern", "flags");

// test 메서드: testContext 문자열에 대해 정규 표현식 regexp의 패턴을 검색하고 매칭 결과를 불리언 값으로 반환.
regexp.test(testcontext);
```

정규 표현식 객체의 생성자(new RegExp(''))를 사용하면 정규 표현식이 `런타임에 컴파일`된다. 패턴이 변할 가능성이 있는 정규 표현식의 경우 생성자 함수를 사용하는 것을 권장한다.

## 패턴과 플래그

### 패턴

### 플래그

플래그 사용은 옵션이며, 여러개를 동시에 사용이 가능하다. 

플래그를 사용하지 않은 경우 대소문자를 구별해서 대상을 탐색하고, 문자열 내에 패턴 매칭 대상이 여러개여도 첫번째 매칭한 대상만 발견하고 탐색을 종료한다.

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
인수로 받은 문자열을 탐색하고 나온 매칭 결과를 배열로 반환하며, 없는 경우 null을 반환한다.

g 플래그가 지정되어도 첫번째 매칭 결과만 반환한다.
```javascript
const target = 'Is this all your idea? Is this what you want?';
const regexp = /is/g;

regexp.exec(target); // ['is', index: 5, input: 'Is this all your idea? Is this what you want?', groups: undefined]
```
### RegExp.prototype.test
인수로 받은 문자열을 탐색하고 나온 매칭 결과를 불리언값으로 반환한다.

```javascript
const target = 'Is this all your idea? Is this what you want?';
const regexp = /is/;

regexp.test(target); // true
```
### String.prototype.match
대상 문자열을 인수로 전달받은 정규표현식으로 탐색하고 나온 매칭 결과를 배열로 반환한다.

exec과 달리 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```javascript
const target = 'Is this all your idea? Is this what you want?';
const regexp = /is/g;

target.match(regexp); // ['is', 'is']
```