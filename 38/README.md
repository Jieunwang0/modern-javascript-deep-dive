# 에러 처리
## try..catch..finally
```javascript
try {
// 에러가 발생할 가능성이 있는 코드
} catch (err) {
// try문에서 발생한 Error 객체가 err에 담겨서 전달된다.
} finally {
// 에러 발생 여부와 상관없이 무조건 1번 실행
// 불필요하다면 생략 가능
}
```
이렇게 에러 처리를 하면 프로그램이 강제 종료되지 않는다.

## Error 객체
```javascript
const error = new Error('message')
```
Error 생성자 함수가 생성한 에러 객체는 message와 stack 프로퍼티를 가진다.
- message 프로퍼티: Error 생성자 함수에 인수로 전달한 에러 메시지
- stack 프로퍼티: 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 게 아니다. (하단 throw 참고)

아래 생성자 함수들이 생성한 에러 객체는 모두 Error.prototype을 상속받는다.
|생성자 함수|인스턴스|
|---|---|
|Error|일반적 에러 객체|
|SyntaxError|자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체|
|ReferenceError|참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체|
|TypeError|피연산자/인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체|
|RangeError|숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체|
|URIError|encodeURI나 decodeURI에 부적절한 인수를 전달했을 때 발생하는 에러 객체|
|EvalError|eval 함수에서 발생하는 에러 객체|

## throw

```javascript
throw 표현식
```
에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야한다.

```javascript
const repeat = (n, f) => {
    if(typeof f !== 'function') throw new TypeError("f must be a function")

    for(var i = 0; i < n; i++) {
        f(i)
    }
};

try {
    repeat(2, 3);
} catch(err) {
    console.error(err);
}

// TypeError: f must be a function
```

## 에러의 전파
에러는 콜 스택의 아래 방향(실행 중인 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

![image (10)](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/59201acf-8945-4d16-93da-fe26705569c6)

- throw된 에러를 catch하지않으면 호출자 방향으로 전파된다. 
- 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 X
    - 이벤트 루프에 의해 마이크로 큐/태스크 큐에서 콜 스택으로 푸시&실행되는데 이때 콜 스택의 가장 하부에 존재하게 된다.
    - 따라서 에러를 전파할 호출자가 존재하지 않는다.