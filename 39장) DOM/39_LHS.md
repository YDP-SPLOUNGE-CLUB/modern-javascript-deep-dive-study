브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM을 생성한다.

DOM (Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API,
즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

# 39.1 노드
## 39.1.1 HTML 요소와 노드 객체

```text
<시작태그 어트리뷰트이름="어트리뷰트값">콘텐츠</종료태그>
```

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 반환된다.

HTML 요소의 어트리뷰트는 어트리뷰트 노드로,
HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

### 트리 자료구조
트리 자료구조는 노드들의 계층 구조로 이뤄진다.
트리 자료구조는 부모노드와 자식노드로 구성되어 노드 간의 계층적 구조(부자, 형제)를 표현하는
비선형 자료구조를 말한다.

### 비선형 자료구조
>하나의 자료 뒤에 여러 개의 자료가 존재할 수 있는 자료구조다.
>비선형 자료구조에는 트리와 그래프가 있다.
>자료구조의 상대적인 개념인 선형 자료구조는 하나의 자료 뒤에 하나의 자료만 존재하는 자료구조다.
>선형 자료구조에는 배열, 스택, 큐, 링크드리스트, 해시 테이블이 있다.

트리 자료구조는 하나의 최상위 노드에서 시작한다.
최상위 노드는 부모 노드가 없으며, 루트노드라 한다.
루트노드는 0개 이상의 자식 노드를 갖는다.
자식 노드가 없는 노드를 리프노드 라 한다.

**노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.**
노드 객체의 트리로 구조화되어 있기 때문에 DOM을 **DOM트리** 라고 부르기도 한다.

## 39.1.2 노드 객체의 타입

DOM은 노드 객체의 계층적인 구조로 구성된다.
노드 객체는 종류가 있고 상속 구조를 갖는다.
노드 객체는 총 12개의 종류(노드 타입)가 있다.

이 중에서 중요한 노드 타입은 다음과 같이 4가지다.

### 문서 노드 (document node)
문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킨다.
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
- 브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유한다.
- 문서 노드 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다. 즉 요소, 어트리뷰트, 텍스트 노드에 접근하려면 노드를 통해야 한다.

### 요소 노드 (elemnt node)
요소 노드는 HTML 요소를 가리키는 객체다.
- 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 구조화한다.
- 요소노드는 문서의 구조를 표현한다고 할 수 있다.

### 어트리뷰트 노드 (attribute node)
어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다.
- 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다.
- 요소 노드는 부모 노드와 연결되어 잇지만 어트리뷰트 노드는 부모노드와 연결되어 있지 않고 요소노드에만 연결되어 있다.
- 즉 어트리뷰트 노드는 부모노드가 없으므로 요소 노드의 형제 노드는 아니다.
- 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야한다.

### 텍스트 노드 (text node)
텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다.
- 요소 노드가 문서의 구조를 표현한다면 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다.
- 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프노드다.
- 텍스트 노드는 DOM 트리의 최종단이다.
- 접근하려면 먼저 부모 노드인 요소 노드에 접근해야한다.

4가지 노트 타입 외에도 주석을 위한 Comment shem,
DOCTYPE을 위한 DocumentType 노드,
복수의 노드를 생성하여 추가할 때 사용하는 DocumentFragment 노드 등
12개의 노드 타입이 있다.

## 39.1.3 노드 객체의 상속 구조

DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조라고 했다.

DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체다.

**노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.**

모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
문서 노드는 Document, HTMLDocument 인터페이스를 상속받고
어트리뷰트 노드는 Attr 인터페이스를 상속받고
텍스트 노드는 CharacterData 인터페이스를 상속받는다.
요소노드는 Element 인터페이스를 상속받는다.

요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된
HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스를 상속받는다.

프로토타입 체인 관점에서 살펴보면
input 요소를 파싱하여 객체화한 input 객체는 HTMLInputElement, HTMLElement, Element, Node, EventTarget, Object의 prototype에 바인딩되어 있는 프로토타입 객체를 상속받는다.
input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.

```js
const $input = ducment.querySelector('input');

console.log(
Object.getPrototypeOf($input) === HTMLInputElement.prototype,
Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
Object.getPrototypeOf(Element.prototype) === Node.prototype,
Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
Object.getPrototypeOf(EventTarget.prototype) === Object.prototype,
); // true
```

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

노드 객체에는 노드 객체의 종류, 즉 노드 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고 노드 타입에 다라 고유한 기능도 있다.

모든 노드 객체는 이벤트를 발생시킬 수 있다.
- EventTarget 인터페이스가 제공한다.(EventTarget.addEventListener, ...)

모든 노드 객체는 트리 자료구조의 노드로서 공통적으로 트리 탐색 기능이 필요하다.
- 노드 관련 기능은 Node 인터페이스가 제공한다. (Node.parentNode, Node.childNodes, ...)

HTML 요소 종류에 따라 고유한 기능도 있다.
- 필요한 기능을 제공하는 인터페이스(HTMLInputElement, HTMLDivElement, 등)가 HTML 요소의 종류에 따라 각각 다르다.

공통된 기능일수록 프로토타입 체인 상위에
개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여
노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.

**DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다.
DOM API를 통해 HTML 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.**

즉 HTML을 DOM과 연관지어 바라보아야 한다.

# 39.2 요소 노드 취득

## 39.2.1 id 를 이용한 요소 노드 취득

```js
// 주의할 점은 id 가 중복될 경우 첫 번째 요소 노드만 반환한다.
// 존재하지 않을 경우 null을 반환한다.
const $elem = document.getElementById('banana')
$elem.style.color = 'red';
```

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo"></div>
		<script>
			// id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
			console.log(foo === document.getElementById('foo')) // true
			// 암묵적으로 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
			delete foo;
			console.log(foo); // <div id='foo'></div>
		</script>
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo"></div>
		<script>
			let foo = 1;
			// id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
			console.log(foo); // 1
		</script>
	</body>
</html>
```

## 39.2.2 태그 이름을 이용한 요소 노드 취득

```js
// HTMLCollection 객체는 유사 배열이면서 이터러블이다.
const $elems = document.getElementByTagName('li');
[...$elems].forEach(el => {el.style.color = 'red'});
```

```js
// 모든 요소 노드를 취득하려면 *를 전달한다.
const $all = document.getElementsByTagName('*');
```

getElementsByTagName 메서드는
Document.prototype에 정의된 메서드와
Element.prototype에 정의된 메서드가 있다.

Document.prototype.getElmentsByTagName
- DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환
  Element.prototype.getElmentsByTagName
- 특정 요소 노드를 통해 호출하며, 특정 요소노드의 자손중에서 요소노드를 탐색하여 반환

```js
const $lisFromDocument = document.getElementByTagName('li');

const $fruits = document.getElementById('fruits');
const $listFromFruits = fruits.getElementByTagName('li');
console.log($listFromFruits)// HTMLCollection(3) [li, li, li]
```

```js
const $li = document.getElementByTagName('li');
// 요소가 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.
console.log($li); // HTMLCollection []
```

## 39.2.3 class 를 이용한 요소 노드 취득

```js
// HTMLCollection 객체를 반환
const $elems = document.getElementsByClassName('fruit');

[...$elems].forEach(el => el.style.color = 'red');
```

마찬가지로 Document.prototype에 정의된 메서드와
Element.prototype에 정의된 메서드가 있다.

```js
const $lisFromDocument = document.getElementByClassName('li');

const $fruits = document.getElementById('fruits');
const $listFromFruits = fruits.getElementByTagName('li');
console.log($listFromFruits)// HTMLCollection [li.banana]
```

## 39.2.4 CSS 선택자를 이용한 요소 노드 취득

```css
/* all */
* { ... }
/* p tag */
p { ... }
/* id 가 foo */
#foo { ... }
/* class가 foo */
.foo { ... }
/* input 요소 중 type 어트리뷰트가 'text' */
input[type=text] { ... }
/* 후손 선택자 : div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }
/* 자식 선택자 : div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }
/* 인접 형제 선택자 : p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소 선택 */
p + ul { ... }
/* 일반 형제 선택자 : p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }
/* 가상 클래스 선택자 */
a:hover { ... }
/* 가상 요소 선택자 */
p::before { ... }
```

Document.prototype/Element.prototype.querySelector 는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다.
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환한다.
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

```js
// class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환한다.
const $elem = document.querySelector('.banana');
$elem.style.color = 'red'
```

Document.prototype/Element.prototype.querySelectorAll 는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다.
DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 유사 배열이면서 이터러블이다.
- 인수로 전달된 CSS 선택자륾 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환한다.
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

```js
const $elem = document.querySelectorAll('.banana');
$elem.forEach(el => { el.style.color = 'red'; })
```

HTML 문서의 모든 요소 노드를 취득하려면 * 를 전달한다.

```js
const $all = document.querySelectorAll('*')
```

CSS 선택자 문법을 사용하는 querySelector, querySelectorAll 메서드는
getElementById, getElementBy\*\*\* 메서드보다 느린 것으로 알려져 있다.

하지만 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다.

때문에 id 어트리뷰트가 있다면 getElementById 를 사용하고 그 외에는 querySelector, querySelectorAll를 사용하는 것을 권장한다.

## 39.2.5  특정 요소 노드를 취득할 수 있는지 확인

Element.prototoype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

```js
const $apple = document.querySelector('apple');
console.log($apple.matches('#fruits > li.apple')); // true
console.log($apple.matches('#fruits > li.banana')); // false
```

이벤트 위임을할 때 유용하다.

## 39.2.6 HTMLCollection과 NodeList

### HTMLCollection

DOM 컬랙션 객체인 HTMLCollection 과 NodeList 는 DOM API 가 여러 개의 결과값을 반환하기 위한 DOM 컬랙션 객체다.
모두 유사 배열 객체이면서 이터러블이다.

중요한 특징은 노드 객체의 **상태 변화를 실시간으로 반영하는 살아있는 객체**라는 것이다.

HTMLCollection 은 언제나 live 객체로 동작한다.
단 NodeList는 대부분의 경우 노드 객체의 상태변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있다.

```js
const $el = document.getElementsByClassName('red');
console.log($el); // HTMLCollection(3)

for(let i = 0; i < $el; i++) {
	$el[i].className = 'blue'
}
console.log($el); // HTMLCollection(1)
```

li 요소의 class 값이 blue 로 변경되어 모든 요소가 파란색으로 될 것 같지만
두 번째 li 요소만 class 값이 변경되지 않는다.

이유는 실시간으로 for문을 순회하면서 \$el.length 의 값이 달라져서이다.
이러한 경우를 회피하기 위해 역순으로 순회하여 작업한다.

```js
const $el = document.getElementsByClassName('red');
console.log($el); // HTMLCollection(3)

for(let i = $el.length - 1; i >= 0; i--) {
	$el[i].className = 'blue'
}
console.log($el); // HTMLCollection(1)
```

더 간단한 해결책은 HTMLCollection 객체를 사용하지 않는 것이다.
HTMLCollection을 배열로 변환하면 부작용을 방지할 수 있다.

```js
[...$el].forEach(e => {e.className = 'blue'})
```

### NodeList

HTMLCollection 객체의 부작용을 해결하기 위해 getElementsByTagName, getElementsByClassName 메서드 대신 querySelectorAll 메서드를 사용하는 방법도 있다.
querySelectorAll 메서드는 DOM 컬랙션 객체인 NodeList 객체를 반환한다.

NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 non-live 객체다.

```js
const $el = document.querySelectorAll('.red');

// NodeList 객체는 NodeList.prototype.forEach를 상속받아 사용할 수 있다.
$el.forEach(e => {e.className = 'blue'})
```

NodeList.prototype 은 forEach, item, keys, values 메서드를 제공한다.

NodeList 는 정적 상태를 유지하는 non-live 객체로 동작한다.
하지만 **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 과 같이 실시간으로 노드 객체의 상태를 변경하므로 주의가 필요하다.**

```js
const $fruits = document.getElementById('fruits');

// childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
// live 객체기 때문에 순회하면 HTMLCollection 과 동일하게 동작한다.
const { childNodes } = $fruits;
```

때문에 HTMLCollection, NodeList 는 배열로 변환하여 사용하는 것을 권장한다.

# 39.3 노드 탐색

요소 노드를 취득한 다음 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있다.

```html
<ul id="fruits">
	<li class="apple">Apple</li>
	<li class="banana">Banana</li>
	<li class="orange">Orange</li>
</ul>
```

DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. setter 없이 getter만 존재하여 참조 가능한 읽기 전용 접근자 프로퍼티다. 값을 할당하면 에러 없이 무시된다.

preventNode, previousSibling, firstChild, childNodes 프로퍼티는 **Node.prototype 이 제공**하고
프로퍼티 키에 Element가 포함된 previouseElementSibling, nextElementSibling 과 children 프로퍼티는 **Element.prototype 가 제공**한다.

## 39.3.1 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다.
스페이스 키, 탭 키, 엔터 키 등을 입력하면 공백 문자가 추가된다.

따라서 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야한다.

## 39.3.2 자식 노드 탐색

자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

Node.prototype.childNodes
- 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환한다.
- **childNodes 프로퍼티가 반환한 NodeList 에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다.**

Element.prototype.children
- 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다.
- **children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다.**

Node.prototype.firstChild
- 첫번째 자식 노드를 반환한다.
- firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.

Node.prototype.lastChild
- 마지막 자식 노드를 반환한다.
- lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.

Element.prototype.firstElementChild
- 첫 번째 자식 요소 노드를 반환한다.
- 요소 노드만 반환한다.

Element.prototype.lastElementChild
- 마지막 자식 요소 노드를 반환한다.
- 요소 노드만 반환한다.

## 39.3.3 자식 노드 존재 확인

자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드를 사용한다.
hasChildNodes 메서드는 자식 노드가 존재하면 true 없으면 false를 반환한다.

단 hasChildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.

```js
const $el = document.getElementById('fruits');
console.log($el.hasChildNodes()); // true
```

자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면
hasChildNodes 메서드 대신 children.length 또는 Element 인터페이스의 childElementCount 프로퍼티를 사용한다.

```js
<html>
<body>
	<ul id="fruits">
	</ul>
</body>
<script>
	const $el = document.getElementById('fruits');
	console.log($el.hasChildNodes()); // true
	
	console.log(!!$el.children.length); // 0 false
	console.log(!!$el.childElementCount); // 0 false
</script>
</html>
```

## 39.3.4 요소 노드의 텍스트 노드 탐색

요소 노드의 텍스트노드는 요소 노드의 자식 노드다.
따라서 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.

```html
<html>
<body>
	<div id="foo">hello</div>
	<script>
		console.log(document.getElementById('foo').firstChild); // #text
	</script>
</body>
</html>
```

## 39.3.5 부모 노드 탐색

부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다.

```html
<html>
<body>
	<div id="fruits">
		<div class="apple">Apple</div>
		<div class="banana">Banana</div>
		<div class="orange">Orange</div>
	</div>
	<script>
		const $banana = document.querySelector('.banana');
		console.log($banana.parentNode); // ul#fruits
	</script>
</body>
</html>
```

## 39.3.6 형제 노드 탐색

어트리뷰 트노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않는다.
즉, 아래 프로퍼티는 텍스트 노드 또는 요소 노드만 반환한다.

Node.prototype.previousSibling
- 부모노드가 같은 형제 노드중에서 자신의 이전 형제 노드를 탐색하여 반환한다.
- 반환하는 형제 노드는 요소 노드 뿐만 아니라 텍스트 노드일 수 있다.
  Node.prototype.nextSibling
- 부모노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다.
- 반환하는 형제 노드는 요소 노드 뿐만 아니라 텍스트 노드일 수 있다.

Element.prototype.previousElementSibling
- 부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환한다.
- 요소 노드만 반환한다.
  Element.prototype.nextElementSibling
- 부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환한다.
- 요소 노드만 반환한다.

# 39.4 노드 정보 취득

노드 정보 프로퍼티를 사용한다.

Node.prototype.nodeType
- 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 Node에 정의되어 있다.
    - Node.ELEMENT_NODE : 요소 노드 타입을 나타내는 상수 1을 반환
    - Node.TEXT_NODE : 텍스트 노드 타입을 나타내는 상수 3을 반환
    - Node.DOCUMENT_NODE : 문서 노드 타입을 나타내는 상수 9를 반환

Node.prototype.nodeName
- 노드의 이름을 문자열로 반환한다.
    - 요소 노드 : 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환
    - 텍스트 노드 : 문자열 "#text"를 반환
    - 문서 노드 : "#document"를 반환

```html
<html>
<body>
	<div id="foo">hello</div>
	<script>
		// 문서 노드의 정보를 취득
		console.log(document.nodeType); // 9
		console.log(document.nodeName); // #document 
		// 요소 노드의 노드 정보를 취득
		const $foo = document.getElementById('foo');
		console.log($foo.nodeType); // 1
		console.log($foo.nodeName); // DIV
		// 텍스트 노드의 노드 정보를 취득
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeType); // 3
		console.log($textNode.nodeName); // #text
	</script>
</body>
</html>
```

# 39.5 요소 노드의 텍스트 조작

앞의 노드 탐색, 노드 정보는 모두 읽기 전용 접근자 프로퍼티다.

## 39.5.1 nodeValue

Node.prototype.value 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다.
참조와 할당 모두 가능하다.

nodeValue 객체를 참조하면 노드 객체의 값을 반환한다.
노드 객체의 값은 텍스트 노드의 텍스트이다.

따라서 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null을 반환한다.

```html
<html>
<body>
	<div id="foo">hello</div>
	<script>
		console.log(document.nodeValue); // null
		const $foo = document.getElementById('foo');
		console.log($foo.nodeValue); // null
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeValue); // hello
		$textNode.nodeValue = 'World'
		console.log($textNode.nodeValue); // World
	</script>
</body>
</html>
```

## 39.5.2 textContent

Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다.
요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.

HTML 마크업은 무시된다.

```html
<html>
<body>
	<div id="foo">hello<span>world</span></div>
	<script>
		console.log(document.getElementById('foo').textContent); // hello world
	</script>
</body>
</html>
```

textContent 프로퍼티에 문자열을 할당하면 모든 자식 노드가 제거되고
할당한 문자열이 텍스트로 추가된다.

이때 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급된다.
즉 HTML 파싱이 되지 않는다.

```js
document.getElementById('foo').textContent = 'Hi <span>there!</span>'
```

참고로 textContent와 유사한 동작을 하는 innerText 프로퍼티가 있다.
innerText 프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋다.
- innerText 프로퍼티는 CSS에 순종적이다. 예를 들어 innerText 프로퍼티는 CSS에 비표시(visibility: hidden;) 로 지정된 요소 노드의 텍스트를 반환하지 않는다.
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 보다 느리다.

# 39.6 DOM 조작

DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.
DOM 에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

## 39.6.1 innerHTML

Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다.
요소 노드의 HTML 마크업을 취득하거나 변경한다.
innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.

```html
<html>
<body>
	<div id="foo">hello<span>world</span></div>
	<script>
		console.log(document.getElementById('foo').innerHTML);
		// hello<span>world</span>
	</script>
</body>
</html>
```

innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고
할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.

```js
document.getElementById('foo').innerHTML = 'Hi <span>there!</span>'
```

사용자 입력 받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은
크로스 사이트 스크립팅 공격(XSS) 에 취약하므로 위험하다.
HTML 마크업 내에 자바스크립트 악성 코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있다.

HTML5를 지원하는 브라우저에서는 `<script>` 요소 내의 코드를 실행하지 않는다.

하지만 이처럼 공격이 가능하다.

```js
document.getElementById('foo').innerHTML = `<img src="x" onerror="alert(document.cookie)">`
```

innerHTML 프로퍼티는 구현이 간단하고 직관적이라는 장점이 있지만 XSS에 취약하다.
### HTML 새니티제이션
HTML 새니티제이션은 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능을 말한다. 새니티제이션 함수를 직접 구현할 수도 있겠지만
DOMPurify 라이브러리를 사용하는 것을 권장한다.
DOMPurify는 다음과 같이 HTML 마크업을 살균하여 잠재적 위험을 제거한다.

```js
DOMPurify.sanitize('<img src="x" onerror="alert(document.cookie)">');
// => <img src="x">
```

innerHTML 의 또 다른 단점은 HTML 마크업 문자열을 할당하는 경우 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경한다는 것이다.

때문에 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.

## 39.6.2 insertAdjacentHTML 메서드

기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

insertAdjacentHTML 메서드는 두 번째 인수로 전달한 HTML 마크업 문자열을 파싱하고
그 결과로 생성된 첫 번째 인수로 전달한 위치에 삽입하여 DOM에 반영한다.

하지만 innerHTML 과 마찬가지로 XSS에 취약하다.

첫 번째 인수로 전달할 수 있는 문자열은 'beforebegin', 'afterbegin', 'beforeend', 'afterend' 4가지다.

```text
(beforebegin)<div id='foo'>(afterbegin)text(beforeend)</div>(afterend)
```

```js
const $foo = document.getElementById('foo');
$foo.insertAdjacentHTML('beforebegin', '<p>text</p>')
```

## 39.5.3 노드 생성과 추가

DOM 은 노드를 직접 생성/삽입/삭제/치환 하는 메서드도 제공한다.

### 요소 노드 생성

```js
const $li = document.createElement('li');
```

요소 노드를 생성할 뿐 DOM에 추가하지는 않는다. 추가하는 처리가 별도로 필요하다.
자식 노드도 갖고 있지 않다.

### 텍스트 노드 생성

```js
const textNode = document.createTextNode('Banana');
```

마찬가지로 DOM에 추가하지는 않는다.

### 텍스트 노드를 요소 노드의 자식 노드로 추가

```js
$li.appendChild(textNode);
```

메서드를 호출한 노드의 마지막 자식 노드로 추가한다.
textContent 로 간결하게 작성 가능하다.

```js
$li.appendChild(document.createTextNode('Banana'))
// 위와 동일하게 동작한다.
$li.textContent = 'Banana';
```

단 자식 노드가 있는경우 모든 자식 노드가 제거되므로 주의해야 한다.

### 요소 노드를 DOM에 추가

```js
$fruits.appendChild($li);
```

비로소 DOM에 직접 추가된다.
이때 만들어진 단 하나의 노드를 추가하므로 리플로우와 리페인트가 한번만 실행된다.

## 39.6.4 복수 노드의 생성과 추가

```js
const $fruits = document.getElementById('fruits');
const $container = document.createElement('div');
['apple','banana','orange'].forEach(text => {
	const $li = document.createElement('li');
	const textNode = document.createTextElement(text);
	$li.appendChild(textNode);
	$container.appendChild($li);
})
$fruits.appendChild($container);
```

단 위 예제는 불필요한 컨테이너 요소 `<div>` 가 추가되므로 바람직 하지 않다.
DocumentFragment 노드를 통해 해결할 수 있다.

### DocumentFragment
DocumentFragment 노드는 문서, 요소, 어트리뷰트, 텍스트 노드 같은 노드 객체의 일종으로,
부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다.
서브 DOM을 구성하여 기존 DOM에 추가하는 용도로 사용한다.

DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자식 노드만 DOM에 추가된다.

```js
const $fruits = document.getElementById('fruits');
const $fragment = document.createDocumentFragment();
['apple','banana','orange'].forEach(text => {
	const $li = document.createElement('li');
	const textNode = document.createTextElement(text);
	$li.appendChild(textNode);
	$fragment.appendChild($li);
})
$fragment.appendChild($container);
```

여러 개의 요소 노드를 추가할 경우에는 createDocumentFragment를 사용하자.

## 39.6.5 노드 삽입

### 마지막 노드로 추가

```js
const $li = document.createElement('li');
$li.appendChild(document.createTextNode('Orange'));
document.getElementById('fruits').appendChild($li);
```

### 지정한 위치에 노드 삽입

첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.

```js
const $fruits = document.getElemetById('fruits')
const $li = document.createElement('li');
$li.appendChild(document.createTextNode('Orange'));
$fruits.appendinsertBefore($li, $fruits.lastElementChild);
```

두 번째 인수로 전달한 노드는 반드시 insertBefore 를 호출한 자식 노드이어야 한다.
그렇지 않으면 DOMException 에러가 발생한다.

두 번째 인수가 null 이면 appendChild 와 동일하게 동작한다.

## 39.6.6 노드 이동

DOM 에 이미 존재하는 노드를 appendChild, insertBefore 메서드를 이용하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 즉 노드가 이동한다.

```js
const $fruits = document.getElementById('fruits');
// 이미 존재하는 요소 노드를 취득
const [$apple, $banana, ] = $fruits.children;
$fruits.appendChild($apple);
$fruits.insertBefore($banana, $fruits.lastElementChild);
```

## 39.6.7 노드 복사

Node.prototype.cloneNode(\[deep: true | false\]) 메서드는 노드의 사본을 생성하여 반환한다.
deep 이 true 이면 노드를 깊은 복사하여 사본을 생성하고 false 이면 얕은 복사하여 사본을 생성한다.

얕은 복사로 생성된 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.

```js
const $fruits = document.getElemetById('fruits')
const $apple = $fruits.firstElementChild;

const $shallowClone = $apple.cloneNode(); // 얕은 복사
$shallowClone.textContent = 'banana';
$fruits.appendChild($shallowClone);

const $deepClone = $fruits.cloneNode(true); // 깊은 복사
$fruits.appendChild($deepClone);
```

## 39.6.8 노드 교체

자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.
첫 번째 매개변수는 교체할 새로운 노드를 전달하고
두 번째 매개변수는 이미 존재하는 교체될 노드를 인수로 전달한다.
두 번째 매개변수에는 replaceChild 를 호출한 노드의 자식 노드이어야 한다.

```js
const $fruits = document.getElemetById('fruits')
const $newChild = document.createElement('li');
$newChild.textContent = 'Banana';

$fruits.replaceChild($newChild, $fruits.firstElementChild);
```

## 39.6.9 노드 삭제

인수로 전달한 노드를 DOM에서 삭제한다.
인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야 한다.

```js
const $fruits = document.getElemetById('fruits')
$fruits.removeChild($fruits.lastElementChild);
```

# 39.7 어트리뷰트

## 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

HTML 문서가 파싱될 때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다.

모든 어트리뷰트 노드의 참조는 유사배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.

따라서 모든 어트리뷰트 노드는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득할 수 있다.

getter만 존재하는 읽기 전용 접근자이며,
요소노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.

```html
<html>
<body>
	<input id="user" type="text" value="val">
	<script>
		const { attributes } = document.getElementById('user');
		console.log(attributes);
		// NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}
		console.log(attributes.id.value) // user
		console.log(attributes.type.value) // text
		console.log(attributes.value.value) // val
	</script>
</body>
</html>
```

## 39.7.2 HTML 어트리뷰트 조작

Element.prototype.getAttribute/setAttribute 메서드를 사용하여 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있다.

```html
<html>
<body>
	<input id="user" type="text" value="val">
	<script>
		const $input = document.getElementById('user');
		console.log($input.getAttribute());
		$input.setAttribute('value', 'foo');
	</script>
</body>
</html>
```

### 특정 어트리뷰트가 존재하는지 확인

Element.prototype.hasAttribute(attributeName) 메서드를 사용한다.

```js
$input.hasAttribute('value')
```

### 특정 어트리뷰트 삭제

Element.prototype.removeAttribute(attributeName) 메서드를 사용한다.

```js
$input.removeAttribute('value')
```

## 39.7.3 HTML 어트리뷰트 vs DOM 프로퍼티

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(이하 DOM 프로퍼티)가 존재한다.
이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.

DOM 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 즉 참조와 변경이 가능하다.

```html
<html>
<body>
	<input id="user" type="text" value="val">
	<script>
		const $input = document.getElementById('user');
		$input.value = 'foo';
		console.log($input.value) // foo
	</script>
</body>
</html>
```

### HTML 어트리뷰트는 DOM에서 중복 관리되고 있을까 ?
1. 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
2. HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티(DOM 프로퍼티)

그렇지 않다.

HTML 어트리뷰트의 역할
- HTML 요소의 초기 상태를 지정하는 것이다. HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다.

**요소 노드는 2개의 상태, 즉 초기 상태와 최신 상태를 관리해야한다.**
**초기 상태는 어트리뷰트 노드**가 관리하며, 요소 노드의 **최신 상태는 DOM 프로퍼티**가 관리한다

어트리뷰트 노드
- 사용자의 입력에 의해 상태가 변경되어도 변하지 않고 초기 상태를 유지한다.
- 취득하거나 변경하려면 getAttribute/setAttribute를 사용한다.
  DOM 프로퍼티
- 사용자가 입력한 최신 상태를 관리한다.
- 사용자 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.
- DOM 프로퍼티에 값을 할당하는 것은 최신 상태 값을 변경하는 것을 의미한다. 어트리뷰트 값에는 어떠한 영향도 주지 않는다.

단 모든 DOM 프로퍼티가 최신 상태를 유지하는 것은 아니다.
- input 요소는 value 프로퍼티가 관리
- checkbox 요소는 checked 프로퍼티가 관리

id 어트리뷰트에 대응하는 id 프로퍼티는 사용자 입력과 아무런 관계가 없다.
때문에 id 프로퍼티와 같은 사용자 입력에 의한 상태 변화에 의미가 없는 어트리뷰트와 DOM 프로퍼티는 동일한 값으로 연동한다.

### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
대부분의 HTML 어트리뷰트는 이름과 동일한 DOM 프로퍼티와 1:1 로 대응한다.
단 다음과 같이 언제나 1:1 로 대응하는 것은 아니며, HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아니다.

- id 어트리뷰트와 id 프로퍼티는 1:1 대응하며 동일한 값으로 연동한다.
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응한다. 하지만 value 어트리뷰트는 초기상태, value 프로퍼티는 최신 상태를 갖는다.
- class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
- textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
- 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다.

### DOM 프로퍼티 값의 타입

getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.

## 39.7.4 data 어트리뷰트와 dataset 프로퍼티

data 어트리뷰트와 dataset 프로퍼티를 사용하면
HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.

data 어트리뷰트는 data-user-id, data-role 과같이 접두사 다음에 임의의 이름을 붙여 사용한다.

```html
<html>
<body>
	<ul class="users">
		<li id="1" data-user-id="7621" data-role="admin">Lee</li>
		<li id="2" data-user-id="9523" data-role="subscriber">Lee</li>
	</ul>
</body>
</html>
```

data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.
DOMStringMap 객체를 반환한다.

DOMStringMap 객체는 data 어트리뷰트와 data 접두사 다음에 붙인 임의의 이름을 가진 프로티를 가지고 있다. 이를 통해 값을 취득하거나 변경할 수 있다.

```html
<html>
<body>
	<ul class="users">
		<li id="1" data-user-id="7621" data-role="admin">Lee</li>
		<li id="2" data-user-id="9523" data-role="subscriber">Lee</li>
	</ul>
	<script>
		const users = [...document.querySelector('.users').children];
		const user = users.find(user => user.dataset.userId === '7621');
		console.log(user.dataset.role); // "admin"
		user.dataset.role = 'subscriber';
		console.log(user.dataset);
		// DOMStringMap {userId: "7621", role: "subscriber"}
	</script>
</body>
</html>
```

존재 하지 않는 이름을 dataset 에 할당하면 data 어트리뷰트가 추가된다.
추가한 카멜케이스의 프로퍼티는 접두사 다음에 케밥케이스로 자동 변경되어 추가된다.

# 39.8 스타일

## 39.8.1 인라인 스타일 조작

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서
인라인 스타일을 취득하거나 추가 또는 변경한다.

```js
const $div = document.querySelector('div');
$div.style.color = 'blue';
```

style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다.
CSSStyleDeclaration 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며 값을 할당하면 해당 CSS 가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.

CSS 프로퍼티는 케밥 케이스를 따른다.
CSSStyleDeclaration 객체는 카멜 케이스를 따른다.

## 39.8.2 클래스 조작

class 어트리뷰트에 대응하는 DOM 프로퍼티는 class 가 아니라 className 과 classList다.
자바스크립트의 class는 예약어이기 때문이다.

### className

Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다.
값을 취득하거나 변경할 수 있다.

className은 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편하다.

```html
<html>
<body>
	<div class="box red">Hello World</div>
	<script>
		const $box = document.querySelector('.box');
		console.log($box.className); // 'box red'
		$box.className = $box.className.replace('red', 'blue')
	</script>
</body>
</html>
```

### classList

Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

```html
<html>
<body>
	<div class="box red">Hello World</div>
	<script>
		const $box = document.querySelector('.box');
		console.log($box.classList); // 노드의 상태 변화를 실시간으로 반영 하는 live 객체
		// DOMTokenList(2) [length:2, value: "box blue", 0: "box", 1: "blue"]
		$box.classList.replace('red', 'blue')
	</script>
</body>
</html>
```

DOMTokenList 객체는 유사배열 객체이면서 이터러블이다. 또한 실시간으로 반영하는 live 객체다.

DOMTokenList 객체는 다음과 같은 유용한 메서드를 제공한다.

**add(...className)**
- 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
```js
$box.classList.add('foo')
$box.classList.add('foo', 'bar')
```

**remove(...className)**
- 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 어트리뷰트에서 삭제한다.
- 일치하는 클래스가 없으면 에러없이 무시된다.
```js
$box.classList.remove('foo')
$box.classList.remove('foo', 'bar')
$box.classList.remove('x')
```

**item(index)**
- 인수로 전달한 index 에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
```js
$box.classList.item(0) // foo
$box.classList.item(1) // bar
```

**contains(classList)**
- 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.
```js
$box.classList.contains('box'); // true
$box.classList.contains('blue'); // false
```

**replace(oldClassName, newClassName)**
- 첫번째 인수로 전달한 문자열을 두번째 인수로 전달한 문자열로 변경한다.
```js
$box.classList.replace('red', 'blue')
```

**toggle(className\[, force\])**
- 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
```js
$box.classList.toggle('foo') // class='box foo'
$box.classList.toggle('foo') // class='box'
```
- 두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.
- true 이면 강제로 첫 번째 인수로 전달받은 문자열을 추가하고 false면 제거한다.
```js
$box.classList.toggle('foo', true)
$box.classList.toggle('foo', false)
```

이 밖에도 forEach, entries, keys, values, supports 메서드를 제공한다.

## 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

style 프로퍼티는 인라인 스타일만 반환한다.
클래스를 적용한 스타일이나 암묵적으로 적용된 스타일은 style 프로퍼티를 통해 참조할 수 없다.

HTML 요소에 적용되어 있는 모든 CSS를 참조하려면 getComputedStyle 메서드를 사용해야 한다.

window.getComputedStyle(element,\[, pseudo\]) 메서드는
첫 번째 인수로 전달한 요소 노드에 적용되어 있는 스타일을 CSSStyleDeclaration 객체에 담아 반환한다.

```html
<html>
<head>
	<style>
		body { color: 'red' }
		.box { 
			width: 100px; height: 50px;
			background-color: cornsilk; border: 1px solid balck;
		}
	</style>
</head>
<body>
	<div class="box">Box</div>
	<script>
		const $box = document.querySelector('.box');
		// CSSStyleDeclaration 객체를 취득
		const computedStyle = window.getComputedStyle($box);
		console.log(computedStyle) // CSSStyleDeclaration
		// 임베디 스타일
		console.log(computedStyle.window); // 100px;
		console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
		console.log(computedStyle.border); // 1px solid rgb(0,0,0)
		// 상속 스타일(body- .box)
		console.log(computedStyle.color); // rgb(255, 0, 0)
		// 기본 스타일
		console.log(computedStyle.display); // balck
	</script>
</body>
</html>
```

두 번째 인수로 :after, :before 같은 의사 요소를 지정하는 문자열을 전달할 수 있다.
의사 요소가 아닌 일반 요소의 경우 두 번째 인수는 생략한다.

```html
<html>
<head>
	<style>
		.box {
			content: "Hello"
		}
	</style>
</head>
<body>
	<div class="box">Box</div>
	<script>
		const $box = document.querySelector('.box');
		const computedStyle = window.getComputedStyle($box, ':before');
		console.log(computedStyle.content); // "Hello"
	</script>
</body>
</html>
```

# 39.9 DOM 표준

HTML과 DOM 표준은
W3C(world Wide Web Consortium) 과
WHATWG(Web Hypertext Application Technology Working Group) 이라는 두 단체가 나름대로 협력하면서 공통된 표준을 만들어왔다.

어느 순간 서로 다른 결과물을 내놓았고 2018년 4월 부터 구글, 애플, 마이크로소프트, 모질라로 구성된 4개의 주류 브라우저 벤더사가 주도하는 WHATWG 이 단일 표준을 내놓기로 두 단체가 합의했다.
현재 DOM은 4개의 레벨(버전)이 있다.

- DOM Level 1
- DOM Level 2
- DOM Level 3
- DOM Level 4

# 알고 가기
---
- [ ] DOM 에 대해 설명해주세요.
