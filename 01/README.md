# 변수

---

```text
10 + 20
```

위 식에서 + 는 연산자, 10과 20을 피연산자라고 한다.
컴퓨터는 CPU로 연산을 하고 메모리로 데이터를 기억하는데, 위 예제에서는 피연산자인 10과 20이 임의의 메모리 주소에 저장되고, CPU를 통해 이 값을 읽어와서 연산을 수행한 후 결과값이 임의의 메모리 주소에 저장된다. 이때 메모리에 저장되는 모든 값은 2진수로 저장된다.

그런데 결과값을 사용하기 위해 메모리 조작을 통해서 값에 접근하면 치명적인 오류를 발생시킬 수 있기 때문에 자바스크립트는 메모리 조작을 허용하지 않는다.
저장된 결과값인 30을 사용하려면 값의 위치를 가리키는 이름을 붙여서 접근하면 된다. (= 변수)

```text
// 변수는 하나의 값을 저장하기 위한 수단이다
var userId = 1;
var userName = James;

// 객체나 배열같은 자료구조를 활용해서 여러개의 값을 하나의 그룹화해서 하나의 값으로 사용할 수 있다.
var user = {id: 2, name: Carol};
var users = [
    {
        id: 3,
        name: Nick,
    },
    {
        id: 4,
        name: Victoria,
    }
]
```
- 변수 이름(= 식별자라고도 함)은 값 자체를 기억하는 게 아니라 값이 있는 메모리 주소를 기억해서 값에 접근할 수 있게 한다. 
- 변수의 이름은 품질 향상과 개발자의 가독성을 높이기 위해 변수에 저장된 값의 의미를 잘 표현하는 이름으로 지어야 한다.

변수를 선언할 때 var, let, const 를 사용한다. ES6부터 let과 const를 도입하기 시작했다. var 키워드로 선언하면 변수에 값을 할당하지 않았어도 자바스크립트 엔진에 의해 undefined가 암묵적으로 할당되어 초기화된다. (let, const는 아님)
```text
var emptyValue; // undefined
// 초기화 : 변수가 선언된 이후 최초로 값을 할당하는 것
```
변수 선언시 1) 선언단계를 거쳐 2) 초기화단계로 수행된다. 
- 선언단계: 변수의 이름을 등록해서 자바스크립트 엔진에게 변수의 존재를 알린다
- 초기화단계: 값을 저장하기 위해 메모리공간을 확보하고 암묵적으로 undefined를 할당해서 초기화한다.  

var을 사용해서 변수 선언을 했다면 이 2단계가 동시에 진행된다.
```text
console.log(x); // undefined;
var x; // 변수 선언문
```
위에서부터 순차적으로 소스코드가 실행될 거라고 예상하고 콘솔에 참조 에러가 뜰 거라고 생각할 수 있지만, var로 선언했다면 실행 이전(런타임 이전)에 먼저 변수 선언이 실행되기 때문에 콘솔에 undefined가 찍힌다. 
```text
console.log(x); // throws a ReferenceError
const x = 'hello';
``````
스코프 안의 어디에서 변수 선언을 했든 최상위에서 선언된 것과 동등한 자바스크립트의 특징을 변수 호이스팅이라고 한다. 
let/const면 호이스팅되지만 undefined를 반환하지 않기 때문에 ReferenceError를 던질 것이다.

그럼 아래 예시에서의 콘솔값을 이렇게 예상할 수 있다.
```text
console.log(exampleValue); // undefined;

var exampleValue; // 1) 변수 선언
exampleValue = '가나다'; // 2) 값 할당

console.log(exampleValue); // '가나다'
```
위의 예시에서 변수 선언하고 값을 할당하는 두 줄을 
```text
var exampleValue = '가나다';
```
이처럼 한 줄로 줄여쓰기도 가능하다. 
## var / let / const 정리

||var|let|const|
|---|---|---|---|
|초기값 없이 선언 가능 여부|O|O|X|
|호이스팅 (hoisting)|O, undefined로 초기화|O, 값은 초기화되지 않음 (TDZ 발생)|O, 값은 초기화되지 않음 (TDZ 발생)|
|재선언 (re-declaration)|O|X|X|
|재할당 (update)|O|O|X|
|유효범위 (scope)|함수 내부에서 선언되었으면 함수 내에서만, 함수 외부에서 선언되었으면 전역적으로 사용|변수가 선언된 블록 내에서만 유효|변수가 선언된 블록 내에서만 유효|

어휘적 바인딩이 실행되기 전까지 액세스할 수 없는 현상을 Temporal Dead Zone(TDZ)라고 한다. TDZ는 초기화되지 않은 바인딩에 접근하려는 경우, 예기치 않은 결과를 내는 대신에 개발자에게 에러 피드백을 제공하기 때문에 유용하게 사용된다.

- 재할당의 경우 
```text 
let a = 100;
a = 150;
```
이때 a의 새로운 값이 기존의 값이 든 메모리 주소에 그대로 덮어씌워지는 게 아니라, 새로운 메모리 주소에 값 150이 들어가고 100과 같이 식별자와 연결되지 않은 불필요한 값들은 가비지 콜렉터에 의해 메모리에서 자동으로 해제된다. (해제시점은 모름)

## 식별자 네이밍 컨벤션
```text
// camelCase
var namingRule;

// snake_case
var naming_rule;

// PascalCase
var NamingRule;

// typeHungarianCase
var strNamingRule; // type + identifier
var $elem = document.getElementId('Id') // DOM node
```