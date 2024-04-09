# 브라우저의 렌더링 과정

-   파싱: (구문 분석 Syntax Analysis). 텍스트 문서의 문자열을 토큰으로 분해하고 토큰의 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 파스 트리를 생성하는 과정
-   렌더링: HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 화면에 시각적으로 출력하는 것

**브라우저의 렌더링 과정**

1. 브라우저가 렌더링에 필요한 리소스를 요청하고 서버로부터 응답받는다.
2. 브라우저의 렌더링 엔진은 서버로부터 응답받은 HTML과 CSS를 파싱해서 DOM, CSSOM을 생성하고 이들을 결합해서 렌더 트리를 생성한다.
3. 브라우저의 자바스크립트 엔진은 서버로부터 응답받은 자바스크립트를 파싱해서 AST를 생성하고 바이트코드로 변환하여 실행한다. 이때 DOM API를 통해 DOM, CSSOM를 조작할 수 있다. 변경되면 다시 렌더 트리로 결합된다.
4. 렌더 트리 기반으로 HTML 요소의 레이아웃을 계산하고 화면에 HTML을 그린다.

## HTTP 1.1과 HTTP 2.0

HTTP은 웹에서 브라우저와 서버가 통신하기 위한 프로토콜이다.

![0_6kn_AwymfFxgjvdp](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/552cc91a-781b-4e6e-bee0-f1eb385e49a2)

**HTTP 1.1**은 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 다중 요청/응답이 불가하다는 단점이 있다.

**HTTP 2.0**은 다중 요청/응답이 가능하다. 여러 리소스의 동시 전송이 가능해서 HTTP 1.1에 비해 약 50% 정도 빠르다고 알려져 있다.

## HTML 파싱과 DOM 생성

![htmlparsing](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/cf23676e-56d6-4f8d-a698-b29e2e6dc619)

1. 서버는 브라우저가 요청한 HTML 파일을 읽고 메모리에 저장한 다음 메모리에 저장된 바이트(2진수)를 인터넷을 경유해서 응답한다.
2. 브라우저는 서버가 응답한 HTML 문서를 바이트 형태로 응답받게 되고, 이 문서의 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식을 통해 문자열로 변환된다.
3. 문자열로 변환된 HTML문서를 읽고 토큰으로 분해한다.
4. 각 토큰들을 객체로 변환하여 노드를 생성한다. 각 노드들은 이후 DOM의 구성하는 기본 요소가 된다.
5. HTML 요소간의 부자 관계를 반영하여 모든 노드들을 트리 자료 구조로 구성한다. 이렇게 구성된 것을 DOM(Document Object Model)이라고 한다. 

- 한 줄로 말하자면 (바이트 -> 문자 -> 토큰 -> 노드 -> DOM)
- 코드 한 줄씩 순차적으로 실행된다.

## CSS 파싱과 CSSOM 생성
![cssparsing](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/94469da2-7227-4ef1-a57e-8b0d73df155c)

브라우저 렌더링 엔진이 HTML을 한 줄씩 순차적 파싱하며 DOM을 만들 때, CSS를 로드하는 style 태그나 link 태그를 만나면 DOM 생성을 일시중단하고 해당 태그 내의 CSS를 HTML처럼 파싱하여 CSSOM을 생성한다. CSS 파싱을 완료하면 DOM 생성을 재개한다.

- 한 줄로 말하자면 (바이트 -> 문자 -> 토큰 -> 노드 -> CSSOM)

- CSSOM은 CSS의 상속을 반영하여 생성된다.
## 렌더 트리 생성
![rendertree](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/f74ae862-83f7-4736-ad87-52e671a9339e)

- **렌더 트리**: 렌더링을 위한 트리 구조의 자료구조이며 화면에 렌더링되는 노드들로만 구성된다. 화면에 렌더링되지 않는 노드와 CSS에 의해 비표시되는 노드들은 포함되지 않는다. 

![paint](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/eba5655d-ecc8-4e1a-89be-dd9bbce0f68e)

브라우저의 렌더링 과정은 반복해서 실행될 수 있다. 

    - 리페인팅, 리플로우 참조



## 자바스크립트 파싱과 실행

![image (7)](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/bbd03165-3a42-4d2d-bf44-5d60a2b6b09d)

브라우저의 자바스크립트 엔진은 자바스크립트 코드를 파싱하여 저수준언어로 변환하고 실행하는 역할을 한다. 

자바스크립트 코드를 해석하여 AST(추상적 구문 트리 Abstract Syntax Tree)를 생성한다. 이를 기반으로 인터프리터가 실행 할 수 있는 중간 코드인 바이트 코드를 생성 및 실행한다.

**토크나이징(Tokenizing)** : 자바스크립트 소스코드를 어휘분석하여 토큰으로 분해한다. 

**파싱** : 토큰들의 집합을 구문분석하여 AST를 생성한다. 

**바이트코드 생성** : AST는 바이트코드로 변환되고 인터프리터에 의해 실행된다. 

### 리플로우/리페인트
자바스크립트 코드에 DOM API가 사용된 경우 변경된 DOM / CSSOM은 다시 렌더 트리로 결합되고 변경된 렌더 트리 기반으로 레이아웃과 페인트 과정을 거처 화면에 다시 렌더링한다. 이를 리플로우, 리페인트라고 한다.

- **리플로우**는 레이아웃을 다시 계산하는 것을 말하는데 색상이 아닌 즉, 노드의 추가나 삭제, 크기나 위치를 다시 계산하는 것을 말한다.
    - 페이지 초기 최초 렌더링
    - 윈도우 리사이징 (Viewport 크기 변경 시)
    - 자바스크립트에 의한 노드 추가 또는 삭제
- **리페인트**는 레이아웃에 영향이 없는 경우 리플로우 없이 실행될 수 있다.
    - 레이아웃에 영향을 주지 않는 스타일 속성 변경시

![images_ansalstmd_post_7e7a8c6d-5c55-4599-9e85-8490d58f7be7_image](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/ab1fadeb-bd66-495a-bb12-a9c8b3fcf570)

### 자바스크립트 파싱에 의한 HTML 파싱 중단
script 태그가 HTML 중간에 있음으로 인한 HTML 파싱 블로킹  
- DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있다. 
- HTML 렌더링을 방해받지 않아 페이지 로딩 시간이 단축된다.

### 해결책 async/defer 어트리뷰트 (IE10)
```javascript
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```
src를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용가능하다. 

**async 어트리뷰트**

`자바스크립트 파일의 로드가 끝난 직후` 파싱과 실행이 진행되며, 이떄 HTML 파싱이 중단된다.

async 어트리뷰트는 IE10 이상에서 지원된다. 
![asyncattribute](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/adb9767b-adc8-43dd-ae93-0f78028e9863)



**defer 어트리뷰트**

자바스크립트 파일의 로드가 끝났어도 HTML 파싱이 끝나고 `DOM 생성이 완료된 직후`에 자바스크립트 파싱과 실행이 진행되며, 이떄 HTML 파싱이 중단된다.

defer 어트리뷰트는 IE10 이상에서 지원한다.
![deferattribute](https://github.com/Jieunwang0/modern-javascript-deep-dive/assets/134492810/29d20d07-8eef-43aa-8e0f-1693e889b45c)