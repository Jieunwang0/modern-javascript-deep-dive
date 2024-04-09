# Set, Map

## Set

Set 객체는 중복되지 않는 값들의 유일한 집합. 배열과는 다음과 같은 차이가 있다.

|구분|배열|Set 객체|
|---|:---:|:---:|
|동일한 값을 중복해서 포함할 수 있다.|O|X|
|요소 순서에 의미가 있다.|O|X|
|인덱스로 요소에 접근할 수 있다.|O|X|

### Set 객체의 생성

Set 생성자 함수는 `이터러블`을 인수로 받아서 Set 객체를 생성한다. 이터러블의 중복된 값은 요소로 저장되지 않는다.
인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```javascript
const set = new Set("hello");
console.log(set); // Set(4) {'h', 'e', 'l', 'o'}
```

중복을 허용하지 않는 Set 객체의 특성을 이용해서 배열에서 중복된 요소를 제거할 수 있다.

```javascript
const arr = (array) => [...new Set(array)];

console.log(arr([1, 1, 2, 3])); // [1, 2, 3]
```

### Set.prototype.size - 요소 갯수 확인

Set 객체의 요소 갯수를 확인할 때 사용하는 size 프로퍼티는 getter 함수만 존재한다. 고로 size  프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.

```javascript
const set = new Set([1, 1, 2, 3]);
console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size')); // {set: undefined, enumerable: false, configurable: true, get: ƒ}, get만 있고 set은 undefined임을 알 수 있다.

console.log(set.size); // 3
```

### Set.prototype.add - 요소 추가

Set 객체에 요소를 추가할 때 사용하는 add 메서드는 새로운 요소가 추가된 Set 객체를 반환하기 때문에 연속 호출이 가능하다. 그리고 자바스크립트의 모든 값을 요소로 저장할 수 있으며, 중복 요소를 허용하지 않는다.

```javascript
const set = new Set([1, 1, 2, 3]);
console.log(set); //  {1, 2, 3}

set.add(6).add(4).add(4);
console.log(set); // {1, 2, 3, 6, 4}
```

Set 객체는 NaN과 NaN을, 0과 -0을 같다고 평가해서 중복 추가를 허용하지 않는다.

```javascript
const set = new Set();
console.log(set); //  Set(0) {size: 0}

set.add(NaN).add(NaN);
console.log(set); //  {NaN}

set.add(0).add(-0);
console.log(set); // {NaN, 0}
```

### Set.prototype.has - 요소 존재 여부 확인

Set 객체에 특정 요소가 존재하는지 확인하기 위해 사용하는 has 메서드는 불리언 값을 반환한다.

```javascript
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(7)); // false
```

### Set.prototype.delete - 요소 삭제

Set 객체의 특정 요소를 삭제하기 위해 사용하는 delete 메서드는 삭제 성공 여부로 불리언 값을 반환한다. 순서에 의미가 없기 때문에 인수로 인덱스가 아니라 삭제하려는 요소값을 전달해야 한다.

```javascript
const set = new Set([1, 2, 3]);

console.log(set.delete(2)); // true
console.log(set.delete(7)); // false
```

삭제 성공 여부를 알리는 불리언 값을 반환하기 때문에 연속 호출할 수 없다.

```javascript
const set = new Set([1, 2, 3]);

set.delete(2).delete(7); // TypeError: set.delete(...).delete is not a function
```

### Set.prototype.clear - 요소 일괄 삭제

Set 객체의 모든 요소를 일괄 삭제하기 위해 사용하는 clear 메서드는 언제나 undefined를 반환한다.

```javascript
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); //  Set(0) {}
```

### Set.prototype.forEach - 요소 순회

Set 객체의 요소를 순회하기 위해 사용하는 forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 콜백 내부에서 this로 사용될 객체를 인수로 받는다.

```javascript
set.forEach(v, v1, set);
```

-   v: 현재 순회 중인 요소값
-   v1: 현재 순회 중인 요소값
-   set: 현재 순회 중인 Set 객체 자체

Set 객체는 요소의 순서에 의미를 갖지 않지만 순회하는 순서는 요소가 추가된 순서를 따른다.

```javascript
const set = new Set([1, 2, 3]);

set.forEach((v, v1, set) => console.log(v, v1, set));
/**
 * 1 1 Set(3) {1, 2, 3}
 * 2 2 Set(3) {1, 2, 3}
 * 3 3 Set(3) {1, 2, 3}
 */
```

Set 객체는 이터러블이기 때문에 for...of문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```javascript
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in set); // true

// for...of 문 사용 가능
for (const item of set) {
    console.log(item); // 1 // 2 // 3
}

// 스프레드 문법 사용 가능
console.log([...set]); // [1 2 3]

// 배열 디스트럭처링 할당 가능
const [x, ...rest] = set;
console.log(x, rest); // 1 [2, 3]
```

### 집합 연산

**교집합** 
![intersection](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/c7714211-b542-451e-a1f2-e22aa7b0a70e)


```javascript
Set.prototype.intersection = function (set) {
    const result = new Set();
    for(const value of set) {
        if(this.has(value)) result.add(value);
    }
    return result;
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.intersection(set2)); // Set(2) {2, 4}
console.log(set2.intersection(set1)); // Set(2) {2, 4}
```
또는
```javascript
Set.prototype.intersection = function (set) {
    return new Set([...this].filter(v => set.has(v)));
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.intersection(set2)); // Set(2) {2, 4}
console.log(set2.intersection(set1)); // Set(2) {2, 4}
```

**합집합**

![union](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/6a15ca0e-15a3-4688-9956-baa70a649b1f)

```javascript
Set.prototype.union = function (set) {
    const result = new Set(this);
    for(const value of set) {
        result.add(value);
    }
    return result;
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.union(set2)); // Set(4) {1, 2, 3, 4}
console.log(set2.union(set1)); // Set(4) {2, 4, 1, 3}
```
또는
```javascript
Set.prototype.union = function (set) {
    return new Set([...this, ...set]);
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.union(set2)); // Set(4) {1, 2, 3, 4}
console.log(set2.union(set1)); // Set(4) {2, 4, 1, 3}

```
**차집합**

![difference](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/6184023b-c116-42f6-8c68-f007c532bed1)

```javascript
Set.prototype.difference = function (set) {
    const result = new Set();
    for(const value of set) {
        if(this.has(value)) result.delete(value);
    }
    return result;
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.difference(set2)); // Set(2) {1, 3}
console.log(set2.difference(set1)); // Set(0) {}
```
또는
```javascript
Set.prototype.difference = function (set) {
    return new Set([...this].filter((v) => !set.has(v)));
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.difference(set2)); // Set(2) {1, 3}
console.log(set2.difference(set1)); // Set(0) {}
```

**부분 집합과 상위 집합**
![superset](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/e89b9bd5-daa6-4641-98e9-2fd6eee9ecf3)


```javascript
Set.prototype.isSuperset = function (subset) {
    for(const value of subset) {
        if(!this.has(value)) return false;
    }
    return true;
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.isSuperset(set2)); // true
console.log(set2.isSuperset(set1)); // false
```
또는
```javascript
Set.prototype.isSuperset = function (subset) {
    const supersetArr = [...this];
    return [...subset].every(v => supersetArr.includes(v));
}

const set1 = new Set([1, 2, 3, 4]);
const set2 = new Set([2, 4]);

console.log(set1.isSuperset(set2)); // true
console.log(set2.isSuperset(set1)); // false
```

## Map
Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. 객체와는 다음과 같은 차이가 있다.

|구분|객체|Map 객체|
|---|---|---|
|키로 이용할 수 있는 값|문자열 or 심벌값|객체를 포함한 모든 값|
|이터러블|X|O|
|요소 개수 확인|Object.keys(obj).length|map.size|

### Map 객체의 생성
Map 생성자 함수는 `이터러블`을 인수로 받아서 Map 객체를 생성한다. 이터러블은 `키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.` 인수를 전달하지 않으면 빈 Map 객체가 생성된다.

만일 중복된 키인 경우 값이 덮어씌워지기 때문에 중복된 키를 갖는 요소가 존재할 수 없다.

```javascript
const map = new Map([['key1', 'value1'], ['key1', 'value2'], ['key2', 'value2']]);

console.log(map); // Map(2) {'key1' => 'value2', 'key2' => 'value2'}
```

### Map.prototype.size - 요소 갯수 확인

Map 객체의 요소 갯수를 확인할 때 사용하는 size 프로퍼티는 getter 함수만 존재한다. 고로 size  프로퍼티에 숫자를 할당하여 Map 객체의 요소 개수를 변경할 수 없다.

```javascript
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size')); // {set: undefined, enumerable: false, configurable: true, get: ƒ}, get만 있고 set은 undefined임을 알 수 있다.

console.log(map.size); // 2
```

### Map.prototype.set - 요소 추가
Map 객체에 요소를 추가할 때 사용하는 set 메서드는 새로운 요소가 추가된 Map 객체를 반환하기 때문에 연속 호출이 가능하다. 그리고 중복 요소를 허용하지 않는다.

```javascript
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1').set('key1', 'value2').set('key2', 'value2');
console.log(map); // Map(2) {'key1' => 'value2', 'key2' => 'value2'}
```

Map 객체는 NaN과 NaN을, 0과 -0을 같다고 평가해서 중복 추가를 허용하지 않는다.

```javascript
const map = new Map();
console.log(map);  // Map(0) {}

map.set(NaN, 'value1').set(NaN, 'value2')
console.log(map);// Map(1) {NaN => 'value2'}


map.set(0, 'value1').set(0, 'value2')
console.log(map); // Map(2) {NaN => 'value2', 0 => 'value2'}
```

문자열이나 심벌값만 키러ㅗ 사용할 수 있는 일반 객체와는 달리 Map 객체는 모든 값을 키로 가질 수 있다.

```javascript
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');
console.log(map); // Map(2) {{…} => 'developer', {…} => 'designer'}
```

### Map.prototype.get - 요소 취득
Map 객체에서 특정 요소를 취득하기 위해 사용하는 get 메서드는 인수로 키를 전달하면 Map 객체에서 그 키를 가진 값을 반환한다. 존재하지 않으면 undefined를 반환한다.

```javascript
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### Map.prototype.has - 요소 존재 여부 확인

Map 객체에 특정 요소가 존재하는지 확인하기 위해 사용하는 has 메서드는 불리언 값을 반환한다.

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

### Map.prototype.delete - 요소 삭제

Map 객체의 특정 요소를 삭제하기 위해 사용하는 delete 메서드는 삭제 성공 여부로 불리언 값을 반환한다. 삭제하려는 키를 인수로 전달해야 하고, 존재하지 않는 키로 요소를 삭제하려 하면 에러없이 무시된다.

```javascript
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(map.delete('key1')); // true
console.log(map.delete('key5')); // false
```

삭제 성공 여부를 알리는 불리언 값을 반환하기 때문에 연속 호출할 수 없다.

```javascript
const map = new Map([['key1', 'value1'], ['key2', 'value2'], ['key3', 'value3']]);

map.delete('key1').delete('key3'); // TypeError: map.delete(...).delete is not a function
```

### Map.prototype.clear - 요소 일괄 삭제

Map 객체의 모든 요소를 일괄 삭제하기 위해 사용하는 clear 메서드는 언제나 undefined를 반환한다.

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

map.clear();
console.log(map); //  Map(0) {}
```

### Map.prototype.forEach - 요소 순회

Map 객체의 요소를 순회하기 위해 사용하는 forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 콜백 내부에서 this로 사용될 객체를 인수로 받는다.

```javascript
map.forEach(v, v1, map);
```

-   v: 현재 순회 중인 요소값
-   v1: 현재 순회 중인 요소키
-   map: 현재 순회 중인 Map 객체 자체

Map 객체는 요소의 순서에 의미를 갖지 않지만 순회하는 순서는 요소가 추가된 순서를 따른다.

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

map.forEach((v, v1, map) => console.log(v, v1, map));
/**
 * developer {name: 'Lee'} Map(2) {{…} => 'developer', {…} => 'designer'}
 * 
 * designer {name: 'Kim'} Map(2) {{…} => 'developer', {…} => 'designer'}
 */
```

Map 객체는 이터러블이기 때문에 for...of문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);
// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in map); // true

// for...of 문 사용 가능
for (const entry of map) {
    console.log(entry); // [{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]

// 스프레드 문법 사용 가능
console.log([...map]); // [{name: 'Lee'}, "developer"], [{name: 'Kim'}, "designer"]

// 배열 디스트럭처링 할당 가능
const [x, y] = map;
console.log(x, y);  // [{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]
```
---

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다. 

|Map 메서드|설명|
|---|---|
|Map.prototype.keys|Map 객체에서 `요소키`를 **값**으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환|
|Map.prototype.values|Map 객체에서 `요소값`을 **값**으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환|
|Map.prototype.entries|Map 객체에서 `요소키와 요소값`을 **값**으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환|


```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

for (const key of map.keys()) {
    console.log(key); // {name: 'Lee'} {name: 'Kim'}

for (const key of map.values()) {
    console.log(key); // developer designer

for (const key of map.entries()) {
    console.log(key); // [{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]
```