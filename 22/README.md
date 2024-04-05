# Date

빌트인 객체이면서 생성자 함수.

- UTC(협정 세계시(Coordinated Universal Time)): 국제 표준시
- KST(한국 표준시(Korea Standard Time)): UTC + 9h

## Date 생성자 함수

### new Date()

-   인수 없이 new 연산자와 함께 호출하면 `현재 날짜와 시간을 가지는 Date 객체`를 반환한다.

```javascript
new Date(); // Sat Apr 06 2024 01:11:36 GMT+0900 (한국 표준시)
```

-   new 연산자 없이 호출하면 `Date 객체가 아닌` 날짜와 시간 정보를 나타내는 문자열을 반환한다.

```javascript
Date(); // 'Sat Apr 06 2024 01:11:36 GMT+0900 (한국 표준시)'
```

### new Date(milliseconds)

-   숫자 타입의 밀리초를 인수에 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수에 전달된 `밀리초만큼 경과된 날짜와 시간을 나타내는 Date 객체`를 나타낸다.

```javascript
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)

new Date(90008770); // Fri Jan 02 1970 10:00:08 GMT+0900 (한국 표준시)
```

### new Date(dateString)

-   날짜와 시간을 나타내는 문자열을 인수로 전달하면 `지정된 날짜와 시간을 나타내는 Date 객체를 반환`한다.
    -   인수에 전달한 문자열은 **Date.parse**로 해석이 가능한 형식이어야 한다.

```javascript
new Date("May 26, 2000 05:12:45"); // Fri May 26 2000 05:12:45 GMT+0900 (한국 표준시)

new Date("2000/05/26/05:12:45"); // Fri May 26 2000 05:12:45 GMT+0900 (한국 표준시)
```

### new Date(year, month, day, hour, minute, second, millisecond);

-   연월일시분초, 그리고 밀리초를 의미하는 숫자를 인수로 전달하면 `지정된 날짜와 시간을 나타내는 Date 객체`를 반환한다.
    -   연, 월은 반드시 지정해야한다. 지정하지 않은 경우 1970년 1월 1일 00:00:00을 나타내는 Date 객체를 반환한다.
    -   지정하지 않은 정보는 0 or 1로 초기화된다.

| 인수        | 내용                                     |
| ----------- | ---------------------------------------- |
| year        | 0부터 99는 1900~1999로 처리된다.         |
| month       | 0~11까지의 정수 (0부터 시작이라 0 = 1월) |
| day         | 1~31까지의 정수                          |
| hour        | 0~23까지의 정수                          |
| minute      | 0~59까지의 정수                          |
| second      | 0~59까지의 정수                          |
| millisecond | 0~999까지의 정수                         |

```javascript
new Date(2024, 3); // Mon Apr 01 2024 00:00:00 GMT+0900 (한국 표준시)

new Date(2024, 3, 6, 1, 26, 00, 00); // Sat Apr 06 2024 01:26:00 GMT+0900 (한국 표준시)

// 가독성이 더 좋은 방법
new Date("2024/4/6/1:26:00"); // Sat Apr 06 2024 01:26:00 GMT+0900 (한국 표준시)
```

## Date 메서드

### Date.now

1970년 1월 1일 00:00:00`(UTC)`를 기점으로 **현재까지 경과한 밀리초**를 숫자로 반환한다.

```javascript
const now = Date.now();

console.log(now); // Sat Apr 06 2024 01:30:41 GMT+0900 (한국 표준시)
```

### Date.parse

1970년 1월 1일 00:00:00`(UTC)`를 기점으로 **인수로 전달된 지정 시간까지의 밀리초**를 숫자로 반환한다.

```javascript
// UTC
Date.parse("Jan 2, 1970 00:00:00 UTC"); // 86400000

// KST(local time)
Date.parse("Jan 2, 1970 09:00:00"); // 86400000

// KST(local time)
Date.parse("1970/01/02/09:00:00 UTC"); // 86400000
```

### Date.UTC

1970년 1월 1일 00:00:00`(UTC)`를 기점으로 **인수로 전달된 지정 시간까지의 밀리초**를 숫자로 반환한다.

-   new Date(y, mon, day, hour, min, sec, millisec)와 같은 형식으로 인수를 전달해야 한다.
-   Date.UTC 메서드의 인수는 로컬 타임(KST)이 아닌 UTC로 인식된다.
    -   month: 0~11까지의 인수임을 주의

```javascript
Date.UTC(1970, 1, 1); // 2678400000, 1970/02/01이기 때문에

Date.UTC(1970, 0, 1); // 0

Date.UTC(1970 / 0 / 1); // NaN
```

---

### Date.prototype.getFullYear

Date 객체의 **연도**를 나타내는 정수를 반환한다.

```javascript
new Date("2024/04/06").getFullYear(); // 2024
```

### Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외 **옵션으로 월, 일** 설정 가능하다.

```javascript
const today = new Date();

// 연도 설정
today.setFullYear(2000);
today.getFullYear(); // 2000

// 옵션 - 날짜(월, 일) 설정
today.setFullYear(2000, 4, 26); // 2000년 5월 26일
today.getFullYear(); // 2000
```

---

### Date.prototype.getMonth

Date 객체의 **월**을 나타내는 0~11의 정수를 반환한다.

```javascript
new Date("2024/04/06").getMonth(); // 3
```

### Date.prototype.setMonth

Date 객체에 월을 나타내는 0~11의 정수를 반환한다. 월 이외 **옵션으로 일** 설정 가능하다.

```javascript
const today = new Date();

// 월 설정
today.setMonth(0);
today.getMonth(); // 0

// 옵션 - 날짜(일) 설정
today.setMonth(10, 2); // 11월 2일
today.getMonth(); // 10
```

---

### Date.prototype.getDate

Date 객체의 **일**을 나타내는 1~31의 정수를 반환한다.

```javascript
new Date("2024/04/06").getDate(); // 6
```

### Date.prototype.setDate

Date 객체에 일을 나타내는 1~31의 정수를 설정한다.

```javascript
const today = new Date();

// 날짜(일) 설정
today.setDate(1);
today.getDate(); // 1
```

---

### Date.prototype.getDay

Date 객체의 **요일**을 나타내는 0~6의 정수를 반환한다.

|  요일  | 반환값 |
| :----: | :----: |
| 일요일 |   0    |
| 월요일 |   1    |
| 화요일 |   2    |
| 수요일 |   3    |
| 목요일 |   4    |
| 금요일 |   5    |
| 토요일 |   6    |

```javascript
new Date("2024/04/06").getDay(); // 6
```

---

### Date.prototype.getHours

Date 객체의 **시간(시)**을 나타내는 0~23의 정수를 반환한다.

```javascript
new Date("2024/04/06/02:00:00").getHours(); // 2
```

### Date.prototype.setHours

Date 객체에 시간(시)을 나타내는 0~23의 정수를 설정한다.

```javascript
const today = new Date();

// 시간(시) 설정
today.setHours(9);
today.getHours(); // 9
```

---

### Date.prototype.getMinutes

Date 객체의 **분**을 나타내는 0~59의 정수를 반환한다.

```javascript
new Date("2024/04/06/02:43:00").getMinutes(); // 43
```

### Date.prototype.setMinutes

Date 객체에 분을 나타내는 0~59의 정수를 설정한다.

```javascript
const today = new Date();

// 시간(분) 설정
today.setMinutes(32);
today.getMinutes(); // 32
```

---

### Date.prototype.getSeconds

Date 객체에 **초**를 나타내는 0~59의 정수를 설정한다.

```javascript
new Date("2024/04/06/02:43:01").getSeconds(); // 1
```

### Date.prototype.setSeconds

```javascript
const today = new Date();

// 시간(초) 설정
today.setSeconds(7);
today.getSeconds(); // 7
```

---

### Date.prototype.getMilliseconds

Date 객체의 **밀리초**를 나타내는 0~999의 정수를 반환한다.

```javascript
new Date("2024/04/06/02:43:01:235").getMilliseconds(); // 235
```

### Date.prototype.setMilliseconds

Date 객체에 밀리초를 나타내는 0~999의 정수를 설정한다.

```javascript
const today = new Date();

// 시간(밀리초) 설정
today.setMilliseconds(777);
today.getMilliseconds(); // 777
```

---

### Date.prototype.getTime

1970년 1월 1일 00:00:00`(UTC)`를 기점으로 **Date 객체의 시간까지 경과된 밀리초를 반환**한다.

```javascript
new Date("2024/04/06/02:43:01:235").getTime(); // 1712338981235
```

### Date.prototype.setTime

Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정한다.

```javascript
const today = new Date();

// 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 시간(밀리초) 설정
today.setTime(86400000);
today.getTime(); // 86400000
```

---

### Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 locale 시간과의 차이를 분단위로 반환한다.

-   UST = KST - 9h

```javascript
const today = new Date();

// UTC와 today의 지정 locale 시간 차는 -9 이다.
today.getTimezoneOffset() / 60; // -9
```

---

### Date.prototype.toDateString

Date 객체의 **날짜**를 표현한 **문자열**을 반환한다.

```javascript
const today = new Date("2024/4/6/02:20");

today.toDateString(); // 'Sat Apr 06 2024'
```

### Date.prototype.toTimeString

Date 객체의 **시간**을 표현한 **문자열**을 반환한다.

```javascript
const today = new Date("2024/4/6/02:20");

today.toTimeString(); // '02:20:32 GMT+0900 (한국 표준시)'
```

### Date.prototype.toISOString

`ISO 8601 형식`으로 Date 객체의 **날짜와 시간을 표현한 문자열**을 반환한다.

```javascript
const today = new Date("2024/4/6/02:20");

today.toISOString(); // '2024-04-05T17:20:00.000Z'
```

### Date.prototype.toLocaleString

인수로 전달한 `Locale 기준`으로 Date 객체의 **날짜와 시간**을 표현한 문자열을 반환한다.

```javascript
const today = new Date("2024/4/6/02:20");

today.toString(); // 'Sat Apr 06 2024 02:20:00 GMT+0900 (한국 표준시)'

today.toLocaleString("ko-KR"); // '2024. 4. 6. 오전 2:20:00'
today.toLocaleString("en-US"); // '4/6/2024, 2:20:00 AM'
today.toLocaleString("ja-JP"); // '2024/4/6 2:20:00'
```

인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용한다.

```javascript
today.toLocaleString(); // '2024. 4. 6. 오전 2:20:00'
```

### Date.prototype.toLocaleTimeString

인수로 전달한 `Locale 기준`으로 Date 객체의 **시간**을 표현한 문자열을 반환한다.

```javascript
const today = new Date("2024/4/6/02:20");

today.toString(); // 'Sat Apr 06 2024 02:20:00 GMT+0900 (한국 표준시)'

today.toLocaleTimeString("ko-KR"); // '오전 2:20:00'
today.toLocaleTimeString("en-US"); // '2:20:00 AM'
today.toLocaleTimeString("ja-JP"); // '2:20:00'
```

인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용한다.

```javascript
today.toLocaleTimeString();  // '오전 2:20:00'
```

---
