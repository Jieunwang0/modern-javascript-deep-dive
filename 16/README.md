# 클로저
클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
```javascript
const x = 10;

function outer(){
    const x = 1;
    function inner() {
        console.log(x); 
    }
    inner();
}

outer(); // 1
```
전역에서 선언한 outer 안의 중첩 함수 inner에서 outer에서 선언한 지역 변수값을 참조할 수 있다.

그런데 outer와 inner 모두 전역에서 정의한다면
```javascript
const x = 10;

function outer(){
    const x = 1;
    inner();
}

function inner() {
    console.log(x);
}

outer(); // 10
```
inner는 outer의 변수를 참조할 수 없고, 상위 스코프인 전역에서 참조한다.

## 렉시컬 환경
자바스크립트 엔진은 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 

### 함수 내부 슬롯 [[Environment]]
함수가 정의되는 환경(위치)과 호출되는 환경(위치)이 다를 수 있지만 렉시컬 스코프가 가능하려면 함수가 정의된 위치(상위 스코프의 참조)를 기억하고 있어야 한다.

이는 함수 정의가 평가되는 시점에 **내부 슬롯 [[Environment]]에 함수의 현재 실행 중인 실행 컨택스트의 렉시컬 환경의 참조를 저장한다는 것을 의미한다.** 

정확히는 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조에 저장할 참조값"은 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨택스트의 렉시컬 환경의 참조(= 상위 스코프)가 할당된다. 이를 `정적 스코프-렉시컬 스코프`-라고 한다. 


## 클로저와 렉시컬 환경
```javascript
const x = 3;
function outer() {
    const x = 5;
    const inner = function(){ // 함수 표현식
        console.log(x); 
    }
    return inner;
}

const innerFunc = outer();
innerFunc(); // 5
```
1. outer를 호출하면 outer의 렉시컬 환경이 생성되고 outer의 내부 슬롯 [[Environment]]에 전역 렉시컬 환경을 할당한다.
2. 함수 표현식이기 때문에 inner를 런타임에 평가한다. inner의 내부 슬롯 [[Environment]]에 outer의 렉시컬 환경을 상위 스코프로서 저장한다.
3. outer의 실행이 종료되면 inner를 반환하고 outer 함수가 실행 컨택스트에서 제거된다.
4. outer 안에서 선언된 지역 변수 x의 생명 주기가 종료되었지만, outer의 렉시컬 환경은 inner의 [[Environment]]에 의해 참조되고 있으므로 사라지지 않는다.
5. 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 내부 슬롯 [[Environment]]에 저장되어 있는 참조값이 할당되기 때문에 innerFunc가 호출되자 outer의 지역변수 x가 다시 동작한 것이다.


외부 함수보다 중첩 함수가 더 오래 유지되는 경우 외부 함수의 생존 여부와 상관없이 중첩 함수는 상위 스코프의 식별자를 참조하거나 값을 변경할 수 있다. 이 중첩 함수를 `클로저`라고 한다. 
 
### 클로저가 아닌 **2가지 경우**가 있다.
- `상위 스코프의 어떤 식별자도 참조하지 않는 경우` 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다. 이 경우의 중첩 함수는 클로저가 아니다. 

- 상위 스코프의 식별자를 참조하고 있지만 외부 함수의 외부로 중첩 함수가 반환되지 않는 경우,  즉 `중첩 함수가 외부 함수보다 생명 주기가 짧을 경우` 일반적으로 클로저라고 하지 않는다.

### 클로저의 활용
- 상태가 **의도치 않게 변경되지 않도록** 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다. 
- **부수 효과를 최대한 억제**하여 오류를 피하고 안정성을 높이기 위해 클로저를 사용한다.

#### 클로저를 활용한 카운트 상태 변경 함수
```javascript
const increase = (function () { 
	let num = 0;
	// 클로저 반환
	return function () { 
		return ++num; // 카운트 상태를 1만큼 증가 
	};
}());

console.log(increase()); // 1 
console.log(increase()); // 2 
console.log(increase()); // 3

```
즉시 실행 함수가 반환한 클로저는 한번만 실행되므로 increase가 호출될 때마다 매번 num 변수가 초기화되지 않는다. 또한 num은 외부에서 접근 불가한 private 변수이므로 의도치 않은 변경을 걱정하지 않아도 된다. 

다음은 외부 상태 변경이나 가변 데이터를 피하고, 불변성을 지향하는 함수형 프로그래밍에서 클로저를 활용하면서 감소 기능을 추가한 예시이다.

#### 함수형 프로그래밍에서 클로저를 활용한 카운트 상태 변경 함수
```javascript
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저 반환함 
function makeCounter(aux) { 
	let counter = 0;

	// 클로저 반환
	return function () { 
		// 인수로 전달받은 보조 함수에 상태 변경을 위임
		counter = aux(counter); 
		return counter; 
	};
}

// (aux에 인수로 넘길) 보조 함수
function increase(n) { 
	return ++n; 
} 

// (aux에 인수로 넘길) 보조 함수
function decrease(n) { 
	return --n; 
}

// makeCounter 함수는 보조 함수를 인수로 전달받아 함수 반환
const increaser = makeCounter(increase);
console.log(increaser()); // 1 
console.log(increaser()); // 2 

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1 
console.log(decreaser()); // -2
```
그런데 이렇게 되면 increaser과 decreaser에 할당된 함수는 각자만의 독립된 렉시컬 환경을 가지게 되므로 카운터의 증감 상태가 연동되지 않는다. 

증감이 가능하게 하려면 **렉시컬 환경을 공유하도록** makeCounter 함수를 두번 호출하지 않게 해야한다.
```javascript
// 함수를 반환하는 고차 함수
// 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저 반환함 
const counter = (function () {
let counter = 0;

	// 함수를 인수로 전달받는 클로저 반환
	return function (aux) { 
		// 인수로 전달받은 보조 함수에 상태 변경을 위임
		counter = aux(counter); 
		return counter; 
	};
}())
	

// (aux에 인수로 넘길) 보조 함수
function increase(n) { 
	return ++n; 
} 

// (aux에 인수로 넘길) 보조 함수
function decrease(n) { 
	return --n; 
}

// 자유 변수 counter가 공유되어 카운트 상태가 공유되었다.
console.log(counter(increase)); // 1 
console.log(counter(increase)); // 2

console.log(counter(decrease)); // 1 
console.log(counter(decrease)); // 0 
```

## 캡슐화와 정보 은닉

객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 캡슐화라고 하고, 캡슐화를 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하는 것을 **정보 은닉**이라고 한다.

- 공개할 필요가 없는 구현의 일부를 외부에 감추어서 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호한다.
- 객체 간의 상호 의존성을 낮추는 효과가 있다.



그리고 대부분의 객체지향 프로그래밍 언어는 **접근 제한자**를 선언해서 공개범위를 한정할 수 있다.
- *public*: public으로 선언된 프로퍼티와 메서드는 클래스 외부에서 참조 가능
- *private*: private으로 선언된 경우는 클래스 외부에서 참조 불가능

자바스크립트는 접근 제한자를 제공하지 않으며 객체의 모든 프로퍼티와 메서드는 **기본적으로 public**하다. 또한 **정보 은닉을 완전하게 지원하지 않는다**.

## 자주 발생하는 실수
`let이나 const 키워드를 사용하는 반복문`에서 코드 블록을 **반복 실행할 때마다 새로운 렉시컬 환경을 생성**하여 반복할 당시의 상태를 스냅샷을 찍는 것처럼 저장하는데 이는 반복문의 코드 블록 내부에서 함수를 정의할 때 의미가 있다. 

반복문의 코드 블록 내부에 함수 정의가 없는 반복문이 생성하는 새로운 렉시컬 환경은 반복 직후 아무도 참조하지 않기 때문에 가비지 컬렉션의 대상이 된다.
```javascript
const funcs = []; 

for (let i = 0; i < 3; i++) { 
	funcs[i] = function () { return i; }; 
} 

for (let i = 0; i < funcs.length; i++) { 
	console.1og(funcs[i]()); // 0 1 2 
}
```
`고차 함수`를 사용하면 변수와 반복문의 사용을 억제할 수 있기 때문에 오류를 줄이고 가독성을 좋게 만든다. 
```javascript
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다
// 배열의 요소로 추가된 함수들은 모두 클로저
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [f, f, f] 

// 배열의 요소로 추가된 함수들을 순차적으로 호출한다
funcs.forEach(f => console.log(f())); // 0 1 2
```