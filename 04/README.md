# 제어문

## 블록문

0 개 이상의 문을 중괄호로 묶은 문. 블록문은 자체 종결성을 가지고 있기 때문에 중괄호가 닫힐 때 세미콜론을 붙이지 않는다.

```javascript
// block statement
{
    var num = 10;
}

// control flow statement
var x = 1;
if (x < 10) {
    x++;
}

// function expression
function sum(a, b) {
    return a + b;
}
```

## 조건문

### if..else 문

-   if문의 조건식은 불리언값으로 평가되어야 한다. 불리언 값에 따라 실행될 코드 블록을 결정한다.

```javascript
if (조건식 1) {
// 조건식 1이 참이면 이 블록이 실행
} else if (조건식 2) {
// 조건식 2가 참이면 이 블록이 실행
} else {
// 앞의 모든 조건식이 거짓이면 이 블록이 실행
}
```

if 뒤에 조건식을 추가하려면 else if(조건식)으로 추가하고 맨 마지막에는 else를 추가하면 된다

### 삼항 조건 연산자

-   대부분의 if..else문은 삼항 조건 연산자로 바꿔 쓸 수 있다.

```javascript
// if..else문
var x = 2;
var result;

if (x % 2) {
    result = "홀수";
} else {
    result = "짝수";
}
// 0은 false로 암묵적 강제 변환된다.
console.log(result); // 짝수

// 삼항 조건 연산자 (2가지 경우)
var x = 2;
var result = x % 2 ? "홀수" : "짝수";
// 0은 false로 암묵적 강제 변환된다.
console.log(result); // 짝수

// 삼항 조건 연산자 (3가지 경우)
var num = 3;
var kind = num ? (num > 0 ? "양수" : "음수") : "영";
console.log(kind); // 양수
```

### switch문

-   switch문의 표현식과 일치하는 case문이 없다면 default문으로 이동하는데, default문은 선택사항이다.
-   case 문에 해당하는 문 끝에 break를 걸어주지 않으면 일치하는 case문을 발견해도 거기서 끝나지 않고 다음 case문으로 연이어 실행되기 때문에 break문을 사용해야한다.

```javascript
var month = 11;
var monthName;

switch(month){
    case 1:monthName = 'January';
        break;
    case 2:monthName = 'February';
        break;
    case 3:monthName = 'March';
        break;
    case 4:monthName = 'April';
        break;
    ...
    case 11:monthName = 'November';
        break;
    case 12:monthName = 'December';
        break;
    default:monthName = 'Invalid month'; // default에는 break를 생략하는 게 일반적이다
}

console.log(monthName); // November
```

## 반복문

조건식의 평가 결과가 참인 경우 코드블록을 실행하고 조건식이 거짓일 때까지 반복한다.

### for문

-   반복 횟수가 명확할 때 주로 사용한다.

```javascript
// for (변수선언문 or 할당문;조건식;증감식) {
//  조건식이 참인 경우 반복 실행될 문;
// }

for (var i = 0; i < 2; i++) {
    console.log(i);
}
// 0
// 1
```

-   for문을 중첩해서도 사용이 가능하다.

### while문

-   반복 횟수가 불명확할 때 주로 사용한다.
-   조건문의 평가 결과가 거짓이 되면 실행하지 않고 종료한다.
-   조건식의 평가 결과가 불리언이 아니면 불리언 값으로 강제 변환한다.

```javascript
var count = 0;

while (count < 3) {
    console.log(count);
    count++;
}
// 0 1 2
```

-   코드 블록 안에 if문으로 탈출 조건을 만들고 break문으로 무한 루프에 빠진 코드 블록을 탈출할 수 있다.

```javascript
var count = 0;

while (true) {
    console.log(count);
    count++;
    if (count === 3) break;
}
// 0 1 2
```

### do...while문

-   코드 블록을 먼저 실행하고 조건식을 평가하기 때문에 블록이 무조건 한번 이상 실행되게 되어있다.

```javascript
var count = 0;

do {
    console.log(count); // 0 1 2
    count++;
} while (count < 3); // 조건이 참일 때까지 do 블록을 실행
```

### break문

-   레이블 문(식별자가 붙은 문), 반복문, switch문의 코드 블록 이외의 경우에서 break를 사용하면 SyntaxError가 발생한다.

```javascript
// 레이블 문을 탈출하려면 break문에 레이블 식별자를 지정한다.

outer: for (var i = 0; i < 3; i++) {
    for (var j = 0; j < 3; j++) {
        if (i + j === 3)
            // break; 하면 내부 블록에서만 벗어나게 된다.
            break outer;
        // outer를 벗어나게 되므로 외부 for문까지 탈출
        console.log(`inner [${i}, ${j}]`);
    }
}
console.log("DONE");
```

```javascript
var string = 'Hello World.';
var search = 'l';
var index;

for(var i = 0; i < string.length; i++) {
    if(string[i] === search){
        index = i;
        break;
    }
}
console.log(index); // 2
// console.log(string.indexOf(search)); 해도 같은 결과
```

### continue문
- 반복문의 코드 블록 실행을 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.
```javascript
var string = 'Hello World.';
var search = 'l';
var count = 0; // l이 몇개인지 세는 변수

for(var i = 0; i < string.length; i++) {
    if(string[i] !== search) continue;
    // l이 아니면 실행을 멈추고 반복문의 증감식으로 이동

    count++; 
    // continue가 실행되면 여기는 실행X
    // l이면 count가 ++되고 나서 반복문의 증감식으로 이동 
}
console.log(count); // 3




// String.prototype.match 메서드도 같은 동작을 한다.
// const regexp = new RegExp(search, 'g'); 
// 여기서 g는 옵션인데 문자열 내의 모든 패턴을 검색하고 없을 시에는 최초 검색 결과만 반환한다.
// console.log(regexp); // 3
```
- if문 내에서 실행할 코드가 한 줄이라면 continue를 안 써도 간결하겠지만 여러 줄이라면 들여쓰기가 깊어지므로 continue를 사용하는 편이 가독성이 좋다.
```javascript
// continue 안 쓰고 들여쓰기가 깊어진 예시


// 'l'이면 카운트를 증가시킨다
 if(string[i] === search) {
    count++;
    // code
    // code
    // code
 }
```