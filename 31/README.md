# DOM

DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API(프로퍼티와 메서드)를 제공하는 트리 자료구조이다.

![R1280x0](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/4aaa49dd-4c71-4ac5-91e0-8ebb110d8c8a)

**HTML (HyperText Markup Language) 요소와 노드 객체** : HTML 요소의 속성은 속성 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

**트리 자료구조** : 노드 간의 계층적 구조를 표현하는 비선형 자료구조

그리고 노드 객체들로 구성된 트리 자료구조를 **DOM**이라고 한다.

### 노드 객체의 타입

```javascript
// 예시

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

아래는 위 문서를 파싱해서 만든 DOM이다.

![R1280x0-3](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/53683edc-7b1c-4b7e-9935-97e7b0e9ed89)

-   **문서 노드**

    -   최상단에 위치한 루트 노드로서 Document 객체를 가리킨다.
    -   전역 객체 window의 document 프로퍼티에 바인딩되어 있어서, window.document나 document로 참조할 수 있다.

-   **요소 노드**

    -   HTML 요소를 가리키는 객체이다. 요소 간의 중첩에 의한 부자 관계를 통해 정보를 구조화한다.

-   **어트리뷰트 노드**

    -   HTML 요소의 속성을 가리키는 객체이다.
    -   요소 노드와 연결되어 있지만 요소 노드의 부모노드 =/= 어트리뷰트 노드는 부모 노드 없음, 따라서 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하려면 우선 요소 노드에 접근해야 한다.

-   **텍스트 노드**
    -   HTML 요소의 텍스트를 가리키는 객체이다.
    -   리프 노드다.(DOM 트리의 최종단이다) 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야한다.
    -   공백 문자(space, enter, tab)는 텍스트 노드를 생성하기 때문에 이후 노드 탐색시 주의해야하고, 가독성 때문에 작성을 권장하지 않는다.

### 노드 객체의 상속 구조

DOM을 구성하는 노드 객체는 ECMAScript에 정의된 `표준 빌트인 객체가 아니라` 브라우저 환경에서 추가적으로 제공하는 `호스트 객체`이다. 하지만 노드 객체도 자바스크립트 객체라서 프로토 타입에 의한 상속 구조를 갖는다.

![image-2](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/49753e9b-ad78-481b-bc5c-af366579aa5d)

모든 노드 객체는 Object, EventTarget, Node를 상속받는다. 그리고 각각 세분화된 인터페이스를 상속받는다.

이를 input 요소 노드 객체의 프로토 타입 체인 관점으로 보자면,

![R1280x0-4](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/042c88f5-ae86-4ae3-aa98-92587d175a3c)

input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토 타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다. (노드 객체의 상속 구조는 개발자 도구 - Elements 우측 Properties에서 확인 가능)

---

노드 객체에는 노드 객체의 종류, 타입에 상관없이 `모든 객체가 공통적으로 갖는 기능`도 있고, `노드 타입에 따라 고유한 기능`도 있다.

요소 노드 객체는 HTML 요소가 갖는 공통적인 기능이 있고, 이러한 공통적인 기능은 HTMLElement 인터페이스가 제공한다.

-   이벤트 관련 기능 (EventTarget.addEventListener...) - EventTarget 인터페이스
-   트리 탐색 기능 (Node.parentNode...) - Node 인터페이스
-   노드 정보 제공 기능 (Node.nodeType...) - Node 인터페이스

특정한 요소 노드만이 필요한 기능을 제공하는 인터페이스는 HTML 요소의 종류에 따라 각각 다르다.
(e.g., input의 value - HTMLInputElement)

프로토타입 체인의 상속 관계를 통해 DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류(노드 타입)에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. DOM API를 통해 HTML 구조나 내용, 스타일을 동적으로 조작할 수 있다.

## 요소 노드 취득

### Document.prototype.getElementById

인수로 id 값을 전달해서 그 값을 가진 하나의 요소 노드를 탐색해서 반환한다.

-   여러 개 있다면 첫번째 요소 노드만 반환한다.
-   없다면 `null`을 반환한다.

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
// id값이 banana인 li 태그의 두번째 값의 스타일 색상을 red로 변경한다.
    const $elem = document.getElementById('banana');
    $elem.style.color = 'red';
    </script>
  </body>
</html>
```

HTML 요소에 id 어트리뷰트를 부여하면 id 값과 `동일한 이름의 전역 변수가 암묵적 선언`되고, 해당 노드 객체가 `할당되는 사이드 이펙트`가 일어난다.

-   단, 이미 해당 이름으로 전역 변수가 이미 선언되어 있으면 노드 객체가 **재할당되지 않는다**.

```javascript
<!DOCTYPE html>
<html>
  <body>
   <div id='foo'></div>
    <script>

// id 값과 동일한 이름의 전역 변수가 암묵적으로 선언된다.
    console.log(foo === document.getElementById('foo')); // true

delete foo;

// 암묵적 전역 프로퍼티는 삭제되지만 생성된 전역 변수는 그대로
    console.log(foo); // <div id='foo'></div>

    </script>
  </body>
</html>
```

### Document.prototype.getElementsByTagName

인수로 전달한 `태그 이름을 가진 모든 요소 노드들`을 탐색해서 반환한다.

-   HTML 문서의 모든 요소 노드를 취득하려면 인수로 '\*'를 전달하면 된다.
-   Document.prototype.getElementsByTagName는 document 전체에서 요소 노드를 탐색해서 반환하고, Element.prototype.getElementsByTagName는 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색해서 반환한다.
-   인수로 전달한 태그 이름을 가진 요소가 없는 경우 `빈 HTMLCollection 객체를 반환한다.`

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruit">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>

// li 태그인 모든 요소를 가져와서 스타일 색상을 red로 변경한다.
    const $elems = document.getElementsByTagName('li');
    [...$elems].forEach(elem => {elem.style.color = 'red';});

// HTML 문서의 모든 요소 노드를 탐색해서 반환
    const $all = document.getElementsByTagName('*');

// id가 fruit인 요소 중 tag name이 li인 요소 노드를 모두 탐색해서 반환
    const $fruit = document.getElementById('fruit');
    const $liTagInFruit = $fruit.getElementsByTagName('li');
    console.log($liTagInFruit);

    </script>
  </body>
</html>
```

HTMLCollection 객체는 유사 배열이면서 이터러블이다.

### Document.prototype.getElementsByClassName

인수로 전달한 `클래스 이름을 가진 모든 요소 노드들`을 탐색해서 반환한다.

-   Document.prototype.getElementsByClassName는 document 전체에서 요소 노드를 탐색해서 반환하고, Element.prototype.getElementsByClassName는 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색해서 반환한다.
-   인수로 전달한 클래스 이름을 가진 요소가 없는 경우 `빈 HTMLCollection 객체를 반환한다.`

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruit">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>

// class name이 apple인 요소를 가져와서 스타일 색상을 red로 변경한다.
    const $elems = document.getElementsByClassName('apple');
    [...$elems].forEach(elem => {elem.style.color = 'red';});

// id가 fruit인 요소 중 class name이 apple인 요소 노드를 모두 탐색해서 반환
    const $fruit = document.getElementById('fruit');
    const $liTagInFruit = $fruit.getElementsByClassName('apple');
    console.log($liTagInFruit);

    </script>
  </body>
</html>
```

### CSS 선택자를 이용한 요소 취득

-   CSS 선택자

```javascript
* {...} // 전체 선택자

p {...} // tag name 선택자
#id {...} // id name 선택자
.class {...} // class name 선택자

input[type=text] {...} // attribute 선택자

div p {...} // 후손 선택자: div 요소의 후손 중 p를 모두 선택
div > p {...} // 자식 선택자: div 요소의 자식 중 p를 모두 선택
p + ul {...} // 인접 형제 선택자: p의 형제 요소 중에 p 바로 뒤에 ul이 오는 녀석들을 선택
p ~ ul {...} // 일반 형제 선택자: p의 형제 요소 중에 p 바로 뒤에 ul이 오는 녀석들을 모두 선택


a:hover {...} // 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택
p::before {...} // 가상 요소 선택자: p 요소의 콘텐츠 앞에 위치하는 공간을 선택
```

Document.prototype/Element.prototype.**querySelector**

-   인수로 전달한 CSS 선택자를 만족시키는 `하나의 요소 노드`를 반환한다.
    -   만족시키는 노드가 여러 개인 경우 첫번째 요소 노드만 반환한다.
    -   없을 경우 `null을 반환`한다.
    -   인수로 전달한 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>

// banana의 스타일 색상을 yellow로 변경한다.
    const $elem = document.querySelector('.banana');
    $elem.style.color = 'yellow';

    </script>
  </body>
</html>
```

Document.prototype/Element.prototype.**querySelectorAll**

-   인수로 전달한 CSS 선택자를 만족시키는 `모든 요소 노드를 반환한다.`
    -   없을 경우 `빈 NodeList를 반환한다.`
    -   인수로 전달한 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>

// ul 태그 뒤에 li가 오는 모든 요소들을 가져와서 스타일 색상을 blue로 변경한다.
    const $elems = document.querySelectorAll('ul > li');
// querySelectorAll는 forEach 사용 가능 (NodeList 객체라서)
    [...$elems].forEach(elem => {elem.style.color = 'blue';})

    </script>
  </body>
</html>
```

NodeList 객체는 유사 배열이면서 이터러블이다.

---

### HTMLCollection와 NodeList

-   공통점

    -   DOM API가 여러 개의 결과값을 반환하는 DOM 컬렉션 객체이다.
    -   배열은 아니지만 유사 배열 객체이기 때문에 이터러블한 성질을 가진다. (for..of문, 스프레드문법 등 사용 가능)
    -   상태 변경과 상관없이 예상과 다르게 동작할 수 있기 때문에 `Array.from 메서드를 사용하여 배열로 변환 후 사용하는 것을 권장한다.`

-   차이점

|              -              | HTMLCollection                                    | NodeList                                                                                                                         |
| :-------------------------: | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
|            태그             | getElementsByTagName, getElementsByClassName      | querySelectorAll                                                                                                                 |
|          프로퍼티           | HTMLCollection.length                             | NodeList.length                                                                                                                  |
|           메서드            | HTMLCollection.item(), HTMLCollection.namedItem() | NodeList.item(), NodeList.entries(), NodeList.forEach(), NodeList.keys(), NodeList.values()                                      |
|       live 객체 여부        | O, 노드 객체의 상태 변화를 실시간으로 반영        | childNodes 프로퍼티는 live 객체지만 대부분 Non-live 객체이다. 노드 객체의 상태 변화를 실시간으로 반영 X, 과거의 정적 상태를 유지 |
| forEach 함수 사용 가능 여부 | X                                                 | O                                                                                                                                |

#### HTMLCollection

HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거하기 때문에 for문을 사용하여 상태 변경시 주의해야 한다.

-   회피법
    -   for 문을 역방향으로 순회
    -   while문 사용
    -   배열로 변환해서 사용

#### NodeList

NodeList 객체는 대부분 Non-live 객체지만 childNodes 프로퍼티가 반환하는 NodeList는 HTMLCollection과 마찬가지로 살아있는 객체라서 접근에 주의가 필요하다.

-   회피법
    -   배열로 변환해서 사용

---

### Element.prototype.matches

인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 여부를 불리언 값으로 반환한다.

-   이벤트 위임할 때 유용

```javascript
<!DOCTYPE html>
<html>
  <body>
    <ul id='fruit'>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>

    const $apple = document.querySelector('.apple');

    console.log($apple.matchs('#fruit' > 'li apple')); // true

    console.log($apple.matchs('#fruit' > 'li banana')); // false

    </script>
  </body>
</html>
```

## 노드 탐색

```javascript
<ul id="fruit">
    <li class="apple">Apple</li>
    <li class="banana">Banana</li>
    <li class="orange">Orange</li>
</ul>
```

여기서 ul#fruit는 3개의 자식 요소를 갖는데, ul#fruit를 먼저 취득하고 자식 노드를 모두 탐색하거나, 하나만 탐색할 수 있다.

그리고 li.banana는 2개의 형제 요소와 부모 요소를 갖는데, li.banana를 먼저 취득하고 형제 노드는 탐색하거나 부모 노드를 탐색할 수 있다.

이렇게 DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

![images-hang_kem_0531-post-4046b3fb-d6fc-4117-a9ec-a00b33fa8eea-image](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/32bec6f0-f95b-435c-8441-d9b63ae88776)

`노드 탐색 프로퍼티는` 모두 접근자 프로퍼티고, setter 없이 getter만 존재해서 참조만 가능한 `readonly property`이다.

### 자식 노드 탐색

|              프로퍼티               | 설명                                                                                 |
| :---------------------------------: | ------------------------------------------------------------------------------------ |
|      Node.prototype.childNodes      | 자식 노드를 모두 탐색해서 NodeList에 담아 반환 텍스트 노드 포함                      |
|     Element.prototype.children      | 자식 노드 중 요소 노드만 모두 탐색해서 HTMLCollection에 담아 반환 텍스트 노드 미포함 |
|      Node.prototype.firstChild      | 첫번째 자식 노드 반환, 텍스트 노드거나 요소 노드                                     |
|      Node.prototype.lastChild       | 마지막 자식 노드 반환, 텍스트 노드거나 요소 노드                                     |
| Element.prototype.firstElementChild | 첫번째 자식 요소 노드 반환                                                           |
| Element.prototype.lastElementChild  | 마지막 자식 요소 노드 반환                                                           |

#### 자식 노드 존재 확인

: **Node.prototype.hasChildNodes**

-   Node.prototype.hasChildNodes 메서드는 텍스트 노드를 포함해서 자식 노드의 존재 여부를 `불리언 값으로 반환한다.`

-   텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 **children.length**나 Element 인터페이스의 **childElementCount** 프로퍼티를 사용한다.

### 요소 노드의 텍스트 노드 탐색

: **Node.prototype.firstChild**

```javascript
<!DOCTYPE html>
<html>
<body>
  <div id="foo">Hello</div>
  <script>
    console.log(document.getElementById('foo').firstChild); // #text
  </script>
</body>
</html>
```

### 부모 노드 탐색

: **Node.prototype.parentNode**

```javascript
<!DOCTYPE html>
<html>
<body>
  <ul id='fruit'>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>

    const $banana = document.querySelector('.banana');
    console.log($banana.parentNode); // ul#fruit

  </script>
</body>
</html>
```

### 형제 노드 탐색

**어트리뷰트 노드는** 요소 노드와 연결되어 있지만 부모가 같은 형제 노드가 아니기 때문에 **반환되지 않는다.** 아래 프로퍼티들은 `요소/텍스트 노드만 반환한다.`

|                 프로퍼티                 | 설명                                                                  |
| :--------------------------------------: | --------------------------------------------------------------------- |
|      Node.prototype.previousSibling      | 부모가 같은 형제 중 자신의 이전 형제 요소/텍스트 노드를 탐색해서 반환 |
|        Node.prototype.nextSibling        | 부모가 같은 형제 중 자신의 다음 형제 요소/텍스트 노드를 탐색해서 반환 |
| Element.prototype.previousElementSibling | 부모가 같은 형제 중 자신의 이전 형제 요소 노드를 탐색해서 반환        |
|   Element.prototype.nextElementSibling   | 부모가 같은 형제 중 자신의 다음 형제 요소 노드를 탐색해서 반환        |

## 노드 정보 취득

|        프로퍼티         | 설명                                                                                                                                                                           |
| :---------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Node.prototype.nodeType | 노드 객체의 종류(노드 타입)를 나타내는 상수를 반환한다. 노드 타입 상수는 Node에 정의되어 있다. (Node.ELEMENT_NODE: 상수 1, Node.TEXT_NODE: 상수 3, Node.DOCUMENT_NODE: 상수 9) |
| Node.prototype.nodeName | 노드의 이름을 문자열로 반환한다. (요소 노드: 대문자 문자열로 태그 이름, 텍스트 노드: 문자열 "#text", 문서 노드: 문자열 "#document")                                            |

## 요소 노드의 텍스트 조작

-   nodeValue
-   textContent

### nodeValue

-   setter와 getter 모두 있는 접근자 프로퍼티
-   텍스트 노드의 텍스트를 반환한다.
-   텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드를 nodeValue로 참조하려 하면 `null을 반환한다.`
-   요소 노드를 취득하고, fistChild로 탐색할 **텍스트 노드를 탐색한 후** nodeValue로 값을 취득/변경한다.

### textContent

-   setter와 getter 모두 있는 접근자 프로퍼티
-   요소 노드의 콘텐츠 영역(<>와 </>사이)내의 `HTML 마크업을 무시하고 남은 텍스트를 모두 반환한다.`
-   요소 노드를 취득하고 textContent로 요소 내의 텍스트를 모두 취득/변경한다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <div id="foo">Hello<span>World</span></div>
  <script>
    console.log(document.nodeValue); // null

    const $foo = document.getElementById('foo');
    console.log($foo.nodeValue); // null

// nodeValue
    const $textNode1 = $foo.firstChild;
    console.log($textNode1.nodeValue); // Hello
    console.log($foo.lastChild.firstChild.nodeValue); // World

// textContent
    const $textNode2 = $foo.textContent;
    console.log($textNode2); // Hello World
  </script>
</body>
</html>
```

-   textContent와 유사한 동작을 하는 innerText 프로퍼티가 있는데, 다음을 이유로 사용을 지양해야한다.
    -   CSS에 순종적이다. CSS에 의해 비표시로 지정된 요소 노드의 텍스트를 반환하지 않는다.
    -   CSS를 고려해야 하므로 textContent보다 느리다.

## DOM 조작

: 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것 **=> 리플로우와 리페인트 발생!**

### innerHTML

-   setter와 getter 모두 있는 접근자 프로퍼티
-   요소 노드의 콘텐츠 영역(<>와 </>사이)내의 `HTML 마크업을 포함한 모든 텍스트를 문자열로 반환한다.`
-   \*단점
    -   \*XSS: Cross-Site Scripting Attacks에 취약
        -   사용자로부터 입력받은 데이터를 그대로 innerHTML에 할당하는 것은 HTML 마크업 내에 악성 코드가 그대로 포함되어 있다면 파싱 과정에서 실행될 위험이 있기 때문에 위험하다.
    -   \*요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하려 DOM을 변경한다.
    -   \*요소를 삽입하려 할 때 삽입할 위치를 지정할 수 없다.
-   innerHTML의 사용은 지양하는 것이 좋고 일반 텍스트를 삽입할 때는 Node.textContent를 사용하는 것을 권장한다.

#### XSS: Cross-Site Scripting Attacks에 취약

-   크로스 사이트 스크립팅 공격:
    -   악의적인 스크립트를 웹 페이지에 삽입하는 공격. 주로 사용자의 개인 정보를 탈취하거나 세션을 위조하여 세션 하이재킹을 수행하는데에 사용된다.
-   XSS공격의 유형

    -   DOM-based XSS: DOM을 조작하여 공격자가 제어하는 데이터를 페이지에 삽입하거나 조작하여 스크립트를 실행시키는 공격
    -   Stored XSS: 공격자가 악의적인 코드를 웹 애플리케이션의 데이터베이스나 파일에 저장하고, 이를 다른 사용자가 웹 페이지를 방문할 때 실행하도록 함.
    -   Reflected XSS: 공격자가 특정 링크를 통해 악의적인 코드를 희생자에게 전달하고, 희생자가 해당 링크를 클릭할 때만 스크립트가 실행되도록 함.

#### innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소의 모든 자식 노드가 제거되고 재생성

```javascript
  <ul id='fruit'>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
    </ul>
    <script>

    const $fruit= document.getElementById('fruit');

    $fruit.innerHTML += '<li class="orange">Orange</li>'

  </script>
```

이 경우 +=라 작성했기 때문에 $fruit.innerHTML에 fruit.orange만 추가되겠다고 생각할 수 있지만, #fruit 요소의 모든 자식 노드가 완전히 제거되고 새롭게 요소 노드 fruit.apple과 fruit.banana를 생성하여 #fruit 요소의 자식 노드로 추가한다.

```javascript
$fruit.innerHTML += '<li class="orange">Orange</li>';

// 위를 축약 표현으로 작성하면 아래가 된다.

$fruit.innerHTML = $fruit.innerHTML + '<li class="orange">Orange</li>';
```

#### 요소가 삽입할 위치를 지정 불가

```javascript
  <ul id='fruit'>
      <li class="apple">Apple</li>
// innerHTML로 <li class="orange">Orange</li>를 여기에 추가할 수 없다.
      <li class="banana">Banana</li>
    </ul>
    <script>
```

### insertAdjacentHTML

insertAdjacentHTML 메서드를 사용하면 기존 요소를 제거하지 않으면서 동시에 위치를 지정해 새로운 요소로 삽입할 수 있다.

<img width="430" alt="image (8)" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/b26843e5-93d7-40b6-a702-985113b8f443">

```javascript
<!DOCTYPE html>
<html>
<body>

  <!-- beforebegin -->

 <div id="foo">
  <!-- afterbegin -->

  text
  <!-- beforeend -->

 </div>
  <!-- afterend -->

  </body>
  <script>

    const $foo = document.getElementById('foo');

    $foo.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>');
    $foo.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>');
    $foo.insertAdjacentHTML('beforeend', '<p>beforeend</p>');
    $foo.insertAdjacentHTML('afterend', '<p>afterend</p>');

</script>
</html>
```

하지만 insertAdjacentHTML 메서드 또한 HTML 마크업 문자열을 그대로 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다는 보안상의 이슈가 해결되진 않지만 자식 요소의 파싱 과정이 생략되기 때문에 성능상 훨씬 좋다.

### 노드 생성과 추가

#### 요소 노드 생성:

```
Document.prototype.createElement(tagname)
```

createElement를 통해 생성하면 요소 노드를 생성할 뿐 기존 DOM에 추가하지 않기 때문에 홀로 붕 떠서 존재하는 상태다.
이후 기존 DOM에 추가하는 처리가 별도로 필요하고, 아무런 자식 노드가 없다.

```javascript
// <div></div> 노드 생성

let $li = document.createElement("li");
```

<img width="640" alt="createElement" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/5bfa920e-b8c2-471e-86d9-80346b188796">

#### 텍스트 노드 생성:

```
Document.prototype.createTextNode(text)
```

원래 텍스트 노드는 요소의 자식인데, createTextNode로 생성한 텍스트 노드는 홀로 붕떠서 존재하는 상태이다. 이후 요소 노드에 추가하는 처리가 필요하다.

```javascript
// <div> 사이에 들어갈 텍스트 노드 생성

let text = document.createTextNode("Banana");
```

<img width="640" alt="createTextNode" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/9c311e2f-415a-463b-b0ec-d08adfb8ea63">

#### 텍스트 노드를 요소 노드의 자식 노드로 추가:

```
Node.prototype.appendChild(textNode)
```

appendChild 메서드를 `호출한 노드의 마지막 자식 노드로 추가한다.` 위치 지정 불가하고 무조건 **마지막**에 추가된다.

```javascript
let $li = document.createElement("li");
let text = document.createTextNode("Banana");

// <div>노드에 텍스트 노드를 자식으로 추가

$li.appendChild(text);
```

![appendChild](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/d182ac98-e79c-4d49-9b87-c66ccb337f65)

단, $li 요소에 자식 노드가 있는 경우 text 프로퍼티에 문자열을 할당하면 요소 노드의 `모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가되므로 주의`해야한다.

#### 요소 노드를 DOM에 추가

```javascript
$fruit.appendChild("$li");
```

<img width="640" alt="appendChildEle" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/7e0f885a-d936-43f8-860e-fee821cb703a">

이때 DOM이 한번 변경되기 때문에 리플로우와 리페인트가 한번 실행된다.

DOM이 여러 번 변경되는 건 지양해야 한다.

**Document.prototype.createDocumentFragment**로 비어있는 DocumentFragment 노드를 생성해서 기존 DOM에 `한 번만 변경을 시도`하는 것이 효율적이다.

1. DocumentFragment 노드 생성
2. 추가할 요소 노드 생성
3. DocumentFragment에 자식 노드로 추가
4. 기존 DOM에 추가

<img width="640" alt="다운로드 (8)" src="https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/abb103ce-6258-49fa-bc26-da6d03b9813b">

### 노드 삽입

-   마지막 노드로 추가 : **Node.prototype.appendChild**

```javascript
element.appendChild(child);
```

-   지정된 위치로 노드 삽입: **Node.prototype.insertBefore**

```javascript
parentNode.insertBefore(newNode, childNode);
```

### 노드 이동

DOM에 이미 존재하는 노드를 appendChild나 insertBefore 메서드로 다시 추가하려고 하면 현재 위치의 노드를 제거하고 새로운 위치에 노드를 추가한다. ( = 노드가 이동한다)

### 노드 복사

Node.prototype.cloneNode([deep: true | false])는 사본을 생성하여 반환한다.

-   매개변수 deep에 `true => 깊은 복사`, 모든 자손 노드를 포함한 사본
-   매개변수 deep에 `false => 얕은 복사`, 노드 자신만을 복사한 사본, 자손이 없으니 텍스트 노드도 없다.

```javascript
element.cloneNode([deep: true | false])
```

### 노드 교체

Node.prototype.replaceNode(newChild, oldChild)는 oldChild를 newChild로 교체한다.

이때 oldChild는 DOM에서 제거된다.

```javascript
parentNode.replaceChild(newChild, oldChild);
```

### 노드 삭제

Node.prototype.removeChild(child)는 인수로 전달한 child 노드를 DOM에서 삭제한다.

인수로 전달한 노드는 메서드를 호출한 노드의 자식 노드여야 한다.

```javascript
parentNode.removeChild(child);
```

## 어트리뷰트

### Attribute Node & Attribute Property

HTML가 파싱되면 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다. 이때 **하나의 어트리뷰트당 하나의 어트리뷰트 노드**가 생성이 된다.

```javascript
<input id='user' type='text' value='book'>

// 여기서 input은 id, type, value 총 3개의 attributes 노드가 생성된다.
```

모든 어트리뷰트 노드의 참조는 유사 배열 객체이자, 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.

Element.prototype.`attributes` 프로퍼티로 요소의 모든 어트리뷰트 노드를 취득할 수 있다.

-   getter만 존재하는 읽기 전용 접근자 프로퍼티
-   요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환

```javascript
<input id='user' type='text' value='book'>
<script>

const { attributes } = document.getElementById('user');

console.log(attributes); // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

console.log(attributes.id.value); // user
console.log(attributes.type.value); // text
console.log(attributes.value.value); // book

</script>
```

### HTML 어트리뷰트 조작

Element.prototype.`getAttribute` / `setAttribute`를 사용하면 attributes 프로퍼티를 통하지 않아도 요소에서 메서드를 통해 HTML 어트리뷰트 값을 취득/변경할 수 있어서 편리하다.

```javascript
// HTML 어트리뷰트 값을 참조하려면
element.getAttribute(attributeName);

// HTML 어트리뷰트 값을 변경하려면
element.setAttribute(attributeName, attributevalue);

// 특정 HTML 어트리뷰트가 존재하는지 확인하려면 (boolean 반환)
element.hasAttribute(attributeName);

// 특정 HTML 어트리뷰트를 삭제하려면
element.removeAttribute(attributeName);
```

### HTML 어트리뷰트 vs. DOM 프로퍼티

-   HTML 어트리뷰트:

    -   HTML 요소의 초기 상태를 지정한다. 변하지 않는다.

-   DOM 프로퍼티:

    -   요소의 최신 상태를 관리한다.

-   **어트리뷰트 노드 :**

    -   HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다. `초기 상태를 취득/변경`하려면 getAttribute/setAttribute 메서드를 사용한다.

-   **DOM 프로퍼티 :**
    -   사용자가 입력한 `최신 상태는` HTML 어트리뷰트에 대응하는 `DOM 프로퍼티`가 관리한다. 언제나 최신 상태를 유지한다.
    -   DOM 프로퍼티에 값을 할당해도 HTML 요소에 지정한 어트리뷰트 값에는 **영향X**

### data 어트리뷰트 & dataset 프로퍼티

-   data 어트리뷰트

    -   사용자가 HTML 요소에 추가 정보를 저장하거나 접근/조작하고 싶을 때 사용
    -   data-role, data-user-id와 같이 data-접두사에 네이밍해서 사용

-   dataset 프로퍼티
    -   HTMLElement.dataset 프로퍼티로 모든 data 어트리뷰트 정보를 제공하는 `DOMStringMap 객체`값을 취득할 수 있다.
    -   DOMStringMap 객체: data 어트리뷰트의 **data- 뒤의 네이밍을 카멜 케이스(fooBar)로 변환한 프로퍼티**를 가진다.
    -   존재하지 않는 이름을 키로 dataset 프로퍼티에 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <ul class='users'>
      <li id='1' data-user-id='7621'>Park</li>
      <li id='2' data-user-id='9524'>Kim</li>
    </ul>
    <script>

    const users = [...document.querySelector('.users').children];

    const user = users.find(user => user.dataset.userId === '7621');

    user.dataset.role = "admin";
    console.log(user.dataset);
    /*  DOMStringMap {userId: "7621", role: "admin"}
    -> <li id='1' data-user-id="7621" data-role="admin">Park</li>
    */
  </script>
  </body>
</html>
```

## 스타일

### in-line 스타일 조작

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티

```javascript
<!DOCTYPE html>
<html>
<body>
<div style="color: red">red apple</div>
    <script>
      const $div = document.querySelector('div');
// red -> green 으로 스타일 변경
      $div.style.color = "green";
// 길이 100px로 스타일 추가
      $div.style.width = "100px";
// 백그라운드컬러 스타일 추가
      $div.style.backgroundColor = "black";
  </script>
  </body>
</html>
```

### 클래스 조작

class 어트리뷰트 조작 대응하는 DOM 프로퍼티

#### className

-   요소의 className 프로퍼티를 `참조`하면 class 어트리뷰트 값을 문자열로 반환한다.
-   요소의 className 프로퍼티에 문자열을 `할당`하면 해당 문자열로 변경된다.
-   className 프로퍼티는 **문자열을 반환**하므로 공백으로 구분된 여러개의 클래스를 반환하는 경우 다루기가 불편하다.

#### classList

Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
DOMTokenList는 아래 메서드들을 제공한다.

-   **add:**
    인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
    ```javascript
    add(...className);
    ```
-   **remove:**
    인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 해당 class 어트리뷰트 값에서 삭제한다. 없으면 시도를 무시한다.
    ```javascript
    remove(...className);
    ```
-   **item:**
    index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
    ```javascript
    item(index);
    ```
-   **contains:**
    className과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인하고 `불리언 값을 반환한다.`
    ```javascript
    contains(className);
    ```
-   **replace:**
    class 어트리뷰트에서 oldClassName을 newClassName으로 대체한다.
    ```javascript
    replace(oldClassName, newClassName);
    ```
-   **toggle:**
    class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
    -   force: true => class 어트리뷰트에 강제로 className을 추가
    -   force: false => class 어트리뷰트에서 강제로 className을 제거
    ```javascript
    toggle(className, [force: true | false])
    ```

## DOM 표준

HTML과 DOM 표준은 W3C와 WHATWG에서 협력하면서 공통된 표준을 만들어왔다.
|레벨|표준 문서 URL|
|:---:|---|
|DOM Level 1|https://www.w3.org/TR/REC-DOM-Level-1|
|DOM Level 2|https://www.w3.org/TR/DOM-Level-2-Core/|
|DOM Level 3|https://www.w3.org/TR/DOM-Level-3-Core/|
|DOM Level 4|https://dom.spec.whatwg.org/|
