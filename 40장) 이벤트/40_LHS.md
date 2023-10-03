# 40.1 이벤트 드리븐 프로그래밍
이벤트가 발생했을 때 호출될 함수를 **이벤트 핸들러**(event handler)라 하고
이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위힘하는 것을 **이벤트 핸들러 등록**이라 한다.

브라우저에서 사용자 버튼 클릭을 감지하여 클릭 이벤트를 발생시킬 수 있다.
특정 버튼 요소에서 클릭 이벤트가 발생하면 특정함수(이벤트 핸들러)를 호출하도록
브라우저에게 위임(이벤트 핸들러 등록)할 수 있다.

즉 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임할 수 있다.

```js
const $button = document.querySeletor('button');
$button.onclick = () { alert('button click') }
```

이와 같이 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을
**이벤트 드리븐 프로그래밍**(event-drivin programing)이라 한다.

# 40.2 이벤트 타입

이벤트 타입은 이벤트의 종류를 나타내는 문자열이다.
이벤트 타입은 200여가지가 있다.

다음은 사용 빈도가 높은 이벤트다.

## 40.2.1 마우스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                 |
| ----------- | ---------------------------------------------------------------- |
| click       | 마우스 버튼을 클릭했을때                                         |
| dblclick    | 마우스 버튼을 더블 클릭했을 때                                   |
| mousedown   | 마우스 버튼을 눌렀을 때                                          |
| mouseup     | 누르고 있던 마우스 버튼을 놓았을 때                              |
| mousemove   | 마우스 커서를 움직였을 때                                        |
| mouseenter  | 마우스 커서를 HTML 요소 안으로 이동했을때 (버블링 되지 않는다.)  |
| mouseover   | 마우스 커서를 HTML 요소 안으로 이동했을 때 (버블링 된다.)        |
| mouseleave  | 마우스 커서를 HTML 요소 밖으로 이동했을 때 (버블링 되지 않는다.) |
| mouseout    | 마우스 커서를 HTML 요소 밖으로 이동했을 때 (버블링 된다.)        |

## 40.2.2 키보드 이벤트

| 이벤트 타입 | 이벤트 발생 시점                            |
| ----------- | ------------------------------------------- |
| keydown     | 모든 키를 눌렀을 때 발생한다.               |
| keypress    | 문자 키를 눌렀을 때 연속적으로 발생한다.    |
| keyup       | 누르고 있던 키를 놓았을 때 한번만 발생한다. |

keydown
- control, option, shift, tab, delete, enter, 방향키와 문자, 숫자, 특수 문자 키를 눌렀을때 발생한다.
- 단 문자, 숫자, 특수문자, enter 키를 눌렀을 때는 연속적으로 발생하지만 그 외의 키를 눌렸을 때는 한번만 발생한다.
  keypress
- control, option, shift, tab, delete, 방향 키 등을 눌렀을 때는 발생하지 않고, 문자, 숫자, 특수문자, enter키를 눌렀을 때만 발생한다. 폐지(deprecated)되었으므로 사용하지 않을 것을 권장
  keyup
- keydown 이벤트와 마찬가지로 control, option, shift, tab, delete, enter, 방향키와 문자, 숫자, 특수 문자 키를 놓았을 때 발생한다.

## 40.2.3 포커스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                    |
| ----------- | --------------------------------------------------- |
| focus       | HTML 요소가 포커스를 받았을 때 (버블링되지 않는다.) |
| blur        | HTML 요소가 포커스를 잃었을 때 (버블링되지 않는다.) |
| focusin     | HTML 요소가 포커스를 받았을 때 (버블링 된다.)       |
| focusout    | HTML 요소가 포커스를 잃었을 때 (버블링 된다.)       |

focusin, focusout 이벤트 핸들러를 이벤트 핸들러 프로퍼티 방식으로 등록하면 크롬, 사파리에서 정상 동작하지 않는다.
focusin, focusout 이벤트 핸들러는 addEventListener 메서드 방식을 사용해 등록해야 한다.

## 40.2.4 폼 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                                                                                                                                                     |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| submit      | 1. form 요소 내의 input(text, checkout, radio), select 입력 필드(textarea제외)에서 엔터 키를 눌렀을 때<br> 2. form 요소 내의 submit 버튼(\<button>, \<input type="submit">)을 클릭했을 때 (submit 이벤트는 form 요소에서 발생한다.) |
| reset       | form 요소 내의 reset 버튼을 클릭했을 때 (최근에는 사용 안함)                                                                                                                                                                         |

## 40.2.5 값 변경 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                                                                                                                                                                                                                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| input            | input(text, checkbox, radio), select, textarea 요소의 값이 입력되었을 때                                                                                                                                                                                                                                                 |
| change           | input(text, checkbox, radio), select, textarea 요소의 값이 변경되었을 때 <br>change 이벤트는 input 이벤트와는 달리 HTML 요소가 포커스를 잃었을 때 사용자 입력이 종료되었다고 인식하여 발생한다. 즉 사용자가 입력을 하고 있을 때는 input 이벤트가 발생하고 사용자 입력이 종료되어 값이 변경되면 change 이벤트가 발생한다. |
| readystatechange | HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값('loading'. 'interactive', 'complete')이 변경될 때                                                                                                                                                                                                |

## 40.2.6 DOM 뮤테이션 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                            |
| ---------------- | ----------------------------------------------------------- |
| DOMContentLoaded | HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때 |

## 40.2.7 뷰 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                     |
| ----------- | ---------------------------------------------------------------------------------------------------- |
| resize     | 브라우저 윈도우(window)의 크기를 리사이즈할 때 연속적으로 발생한다. 오직 window 객체에서만 발생한다. |
| scroll      | 웹페이지(document)또는 HTML 요소를 스크롤할 때 연속적으로 발생한다.                                  |

## 40.2.8 리소스 이벤트

| 이벤트  타입 | 이벤트 발생 시점                                                                                                      |
| ------------ | --------------------------------------------------------------------------------------------------------------------- |
| load         | DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료되었을 때(주로 window 객체에서 발생) |
| unload       | 리소스가 언로드될 때 (주로 새로운 웹페이지를 요청한 경우)                                                             |
| abort        | 리소스 로딩이 중단되었을 때                                                                                           |
| error        | 리소스 로딩이 실패했을 때                                                                                             |


# 40.3 이벤트 핸들러 등록

이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라 한다.

총 3가지 방법이 존재한다.

## 40.3.1 이벤트 핸들러 어트리뷰트 방식
HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 이벤트 핸들러 어트리뷰트가 있다.
on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.

이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문(state)을 할당하면 이벤트 핸들러가 등록된다.

```html
<!DOCTYPE html>
<html>
<body>
	<button onclick="sayHi('Lee')">Click me!</button>
	<script>
		function sayHi(name) {
			console.log(`Hi : ${name}.`);
		}
	</script>
</body>
</html>
```

이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당했다.
이때 **이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트의 핸들러의 함수 몸체를 의미한다.**
이벤트 핸들러에 인수를 전달하기 위해서다.

따라서 이벤트 핸들러 어트리뷰트 값으로 다음과 같이 여러 개의 문을 할당할 수 있다.

```html
<button onclick="console.log('Hi! ') console.log('Lee');">Click me!</button>
```

이벤트 핸들러 어트리뷰트 방식은 오래된 코드에서 간혹 이방식을 사용한 것이 있기 때문에 알아둘 필요는 있지만 더는 사용하지 않는 것이 좋다.

HTML과 자바스크립트는 관심사가 다르므로 혼재하는 것보다 분리하는 것이 좋다.

하지만 모던 자바스크립트에서는 이벤트 핸들러 어트리뷰트 방식을 사용하는 경우가 있다.

CBD(Component Based Development) 방식의 Angular/React/Svelte/Vue.js 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리한다.

CBD 에서는 HTML, CSS 자바스크립트를 관심사가 다른 개별적인 요소가 아닌 뷰를 구성하기 위한 구성 요소로 보기 때문에 관심사가 다르다고 생각하지 않는다.

```JSX
// Angular
<button (click)="click($event)">Save</button>
// React
<button onClick={click($event)}>Save</button>
// Svelte
<button on:click={click($event)}>Save</button>
// Vue.js
<button v-on:click="click($event)">Save</button>
```

## 40.3.2 이벤트 핸들러 프로퍼티 방식

window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있다.

```js
const $button = document.querySeletor('button');
$button.onclick = function () {
	console.log('button click')
}
```

이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 이벤트 타깃과
이벤트의 종류를 나타내는 문자열인 이벤트타입
그리고 이벤트 핸들러를 지정할 필요가 있다.

```text
$button.onclick = function () {}
------- 이벤트 타깃 (event target)
		------- on + 이벤트 타입
					------------ 이벤트 핸들러
```

이벤트 핸들러는 대부분 이벤트를 발생시킬 이벤트 타깃에 바인딩한다.
하지만 반드시 이벤트 타깃에 이벤트 핸들러를 바인딩해야 하는 것은 아니다.
이벤트 핸들러는 이벤트 타깃 또는 전파된 이벤트를 캐치할 DOM 노드 객체에 바인딩한다.

이벤트 핸들러 어트리뷰트 방식도 결국 DOM 노드 객체의 이벤트 핸들러 프로퍼티로 변환되므로 결과적으로 이벤트 핸들르 퍼르퍼티 방식과 동일하다고 할 수 있다.

하지만 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있다.
## 40.3.3 addEventListener 메서드 방식

```text
EventTarget.addEventListener('eventType', functionName, \[, useCapture\])
----------- 이벤트 타깃
							------------- 이벤트 타입
										  ------------ 이벤트 핸들러
														  --------------
														  capture 사용여부
														  true: capturing
														  false: bubbling(기본값)
```

첫 번째 매개 변수
- 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달한다.
- on 접두사를 붙이지 않는다.
  두 번째 매개 변수
- 이벤트 핸들러를 전달한다.
  마지막 매개 변수
- 이벤트를 캐치할 이벤트 전파 단계(캡처링 또는 버블링)

```js
const $button = document.querySeletor('button')
$button.addeventListener('click', function () {
	console.log('button click');
})
```

동일한 HTML 요소에서 발생한 동일한 이벤트에 대해
이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식을 모두 사용하여 이벤트 핸들러를 등록하면
2개의 이벤트 핸들러 모두 호출된다.

```js
const $button = document.querySeletor('button')
$button.onclick = function () {
	console.log('button click 1')
}
$button.addeventListener('click', function () {
	console.log('button click 2');
})
```

addEventListener 는 하나 이상의 이벤트 핸들러를 등록할 수 있다.
이때 이벤트 핸들러는 등록된 순서대로 호출된다.

```js
const $button = document.querySeletor('button')
$button.addeventListener('click', function () {
	console.log('button click 1');
})
$button.addeventListener('click', function () {
	console.log('button click 2');
})
```

메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록된다.

```js
const $button = document.querySeletor('button')
const handlerClick = () => console.log('click')
$button.addeventListener('click', handleClick)
$button.addeventListener('click', handleClick)
```

# 40.4 이벤트 핸들러 제거

EventTarget.prototoype.removeEventListener 메서드를 이용한다.
전달할 인수는 addEventListener 메서드와 동일하다.
단, addEventListener 메서드에 전달한 인수와 removeListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.

```js
const $button = document.querySeletor('button')
const handlerClick = () => console.log('click')
$button.addEventListener('click', handleClick)
$button.removeEvnetListener('click', handleClick, true); // 실패
$button.removeEvnetListener('click', handleClick) // 성공
```

무명함수를 이벤트 핸들러로 등록한 경우 제거할 수 없다.

```js
$button.addEventListener('click', () => console.log('click'))
```

단 기명 이벤트 핸들러 내부에서 제거하는 것은 가능하다.
이때 이벤트 핸들러는 단 한번만 호출된다.
여러번 클릭해도 단한번만 이벤트 핸들러가 호출된다.

```js
$button.addEventListener('click', function foo() {
	console.log('button click')
	$button.removeEventListener('click', foo)
});
```

기명함수를 이벤트 핸들러로 등록할 수 없다면, 호출된 함수
함수 자신을 가리키는 arguments.callee를 사용할 수도 있다.

```js
$button.addEventListener('click', function () {
	console.log('button click')
	$button.removeEventListener('click', arguments.callee)
});
```

arguments.callee는 코드 최적화를 방해하므로 strict mode에서 사용이 금지된다.
따라서 지양하는 편이 좋다.

이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거할 수 없다.
이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null을 할당한다.

```js
const $button = document.querySeletor('button')
const handlerClick = () => console.log('click')

$button.onclick = handleClick;
$button.removeEnvetListener('click', handlerClick) // 제거할 수 없다.
$button.onclick = null;
```

# 40.5 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.
**생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달한다.**

```js
const $msg = document.querySeletor('.message');
function showCoords(e) {
	$msg.textContent = `x : ${e.clientX} y: ${e.clientY}`
}
document.onclick = showCoords
```

이벤트 핸들러 어트리뷰트 방식으로 이벤트 핸들러는 다음과 같이 이벤트 객체를 전달 받을 수 있다.

```html
<body onclick="showCoords(event)">
	<script>
		const $msg = document.querySeletor('.message');
		function showCoords(e) {
			$msg.textContent = `x : ${e.clientX} y: ${e.clientY}`
		}
	</script>
</body>
```

이벤트 어트리뷰트 방식의 경우 event가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못한다.
이벤트 핸들러 어트리뷰트 값은 암묵적으로 생성되는 이벤트 핸들러의 함수 몸체를 의미하기 때문이다.
다음과같이 암묵적으로  생성한다.

```js
function onclick(evnet) {
	showCoords(evnet);
}
```

## 40.5.1 이벤트 객체의 상속 구조

![[Pasted image 20231002123407.png]]

이벤트 객체는 위와 같은 상속 구조를 갖는다.
Event, UIEvent, MouseEvent 등 모두는 생성자 함수다.

생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다.

```js
let e = new Event('foo');
e = new FocusEvent('focus');
e = new MouseEvent('click');
e = new KeyboardEvent('keyup')
e = new InputEvent('change');
```

이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고
일부는 자바스크립트 코드에 의해 인위적으로 생성된 것이다.

MouseEvent 타입의 이벤트 객체는 사용자가 마우스를 클릭하거나 이동했을때 생성되는 이벤트 객체이며,
CustomEvent 타입의 이벤트 객체는 자바스크립트 코드에 의해 인위적으로 생성한 이벤트 객체다.

Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체를 나타낸다.
Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent, KeyboardEvent, WheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있다.

즉 이벤트 객체의 프로퍼티는 발생한 이벤트의 타입에 따라 달라진다.

```js
const $input = document.querySeletor('input[type=text]')
const $checkbox = document.querySeletor('input[type=checkbox]')
const $button = document.querySeletor('button')

// load 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
window.onload = console.log;
// change 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
$checkbox.onchnage = console.log;
// focus 이벤트가 발생하면 FocusEvent 타입의 이벤트 객체가 생성된다.
$input.onfocus = console.log;
// input 이벤트가 발생하면 InputEvent 타입의 이벤트 객체가 생성된다.
$input.oninput = console.log;
// keyup 이벤트가 발생하면 KeyboardEvent 타입의 이벤트 객체가 생성된다.
$input.onkeyup = console.log;
// click 이벤트가 발생하면 MouseEvent 타입의 이벤트 객체가 생성된다.
$button.onclick = console.log;
```

## 40.5.2 이벤트 객체의 공통 프로퍼티

Event 인터페이스, 즉 Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는
UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속된다.

즉 Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다.

| 공통 프로퍼티    | 설명                                                                                                                                                                                                                                      | 타입          |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| type             | 이벤트 타입                                                                                                                                                                                                                               | string        |
| target           | 이벤트를 발생시킨 DOM 요소                                                                                                                                                                                                                | DOM 요소 노드 |
| currentTarget    | 이벤트 핸들러가 바인딩된 DOM 요소                                                                                                                                                                                                         | DOM 요소 노드 |
| eventPhase       | 이벤트 전파 단계<br>0: 이벤트 없음. 1: 캡처링 단계, 2: 타깃 단계, 3: 버블링 단계                                                                                                                                                          | number        |
| bubbles          | 이벤트를 버블링으로 전파하는지 여부, 다음 이벤트는 bubbles:false로 버블링하지 않는다.<br>포커스 이벤트 focus/blur<br>리소스 이벤트 load/unload/abort/error<br>마우스 이벤트 mouseenter/mouseleave                                         | boolean       |
| cancelable       | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부. 다음 이벤트는 cancelable:flase로 취소할 수 없다.<br>포커스 이벤트 focus/blur<br>리소스 이벤트 load/unload/abort/error<br>마우스 이벤트 mouseenter/mouseleave | boolean       |
| defaultPrevented | preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부                                                                                                                                                                                 | boolean       |
| isTrusted        | 사용자의 행위에 의해 발생한 이벤트인지 여부. 예를 click 메서드 또는 dispatchEvent 메서드를 통해 인위적으로 발생시킨 이벤트인 경우 IsTrusted는 false다.                                                                                    | boolean       |
| timeStamp        | 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초)                                                                                                                                                                                | number        |

```js
$checkbox.onchange = e => {
	console.log(Object.getPrototypeOf(e) === Event.prototype); // true
	// e.target은 change 이벤트를 발생시킨 DOM 요소 $checkboxfmf rkflzlrh
	// e.target.checked는 체크 박스 요소의 현재 체크 상태를 나타낸다.
	console.log(e.target === e.currentTarget); // true
}
```

일반적으로 target 프로퍼티와 currentTarget 프로퍼티는 동일한 DOM 요소를 가리키지만
이벤트 위임에 따라 서로 다른 DOM 요소를 가리킬 수 있다.

## 40.5.3 마우스 정보 취득

click, dblclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 다음과 같은 고유 프로퍼티를 갖는다.

- 마우스 포인터의 좌표 정보를 나타내는 프로퍼티
    - scrrenX/scrrenY, clientX/clientY, pageX/pageY, offsetX/offsetY
- 버튼 정보를 나타내는 프로퍼티
    - altKey, ctrlKey, shiftKey, button

## 40.5.5 키보드 정보 취득

keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는
altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유 프로퍼티를 갖는다.

참고로 input 요소의 입력 필드에 한글을 입력하고 엔터 키를 누르면 keyup 이벤트 핸들러가 두 번 호출되는 현상이발생한다. 이 같은 문제를 회피하려면 keyup 이벤트 대신keydown 이벤트를 캐치한다.

# 40.6 이벤트 전파

DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다.
이를 이벤트 전파(event propagation)라고 한다.

```html
<!DOCTYPE html>
<html>
<body>
	<ul id="fruits">
		<li id="apple">Apple</li>
		<li id="banana">Banana</li>
		<li id="orange">Orange</li>
	</ul>
</body>
</html>
```

ul 요소의 두 번째 자식인 li 요소를 클릭하면 클릭 이벤트가 발생한다.
이때 **생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃(event target)을 중심으로 DOM 트리를 통해 전파된다.**

이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 다음과 같이 3단계로 구분할 수 있다.

![[Pasted image 20231002132033.png]]

- 캡처링 단계 (capturing phase) : 이벤트가 상위 요소에서 하위 요소 방향으로 전파
- 타깃 단계 (target phase) : 이벤트가 이벤트 타깃에 도달
- 버블링 단계 (bubbling phase) : 이벤트가 하위 요소에서 상위 요소 방향으로 전파

ul 요소에 이벤트 핸들러를 바인딩하고 ul 요소의 하위 요소인 li 요소를 클릭하여 이벤트를 발생시켜 보자.
이벤트 타깃(event.target)은 li 요소이고 커런트 타깃(currentTaget)은 ul 요소다.

```js
const $fruits = document.getElementById('fruits');

// #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
$fruits.addEventListener('click', e =>> {
	console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
	console.log(`이벤트 단계: ${e.taget}`); // [object HTMLLIElement]
	console.log(`이벤트 단계: ${e.currentTaget}`); // [object HTMLUListElement]
})
```

캡처링 단계
- 클릭 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파된다.
  타깃 단계
- 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달한다.
  버블링 단계
- 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파된다.

이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.
하지만 addEventListener 메서드 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계분만 아니라 캡처링 단계의 이벤트도 선별적으로 캐치할 수 있다.

캡처링 단계의 이벤트를 캐치하려면 addEventListener 메서드의 3번째 인수로 true를 전달해야 한다.
생략하거나 false를 전달하면 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.

**이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다.**

즉 DOM 트리를 통해 전파되는 이벤트는 이벤트 패스(이벤트가 통과하는 DOM 트리상의 경로, Event.prototoype.composedPath 메서드로 확인할 수 있다.)에 위치한 모든 DOM 요소에서 캐치할 수 있다.

대부분의 이벤트는 캡처링과 버블링을 통해 전파된다.

하지만 다음 이벤트는 버블링을 통해 전파되지 않는다.
이 이벤트들은 버블링을 통해 이벤트를 전파하는지 여부를 나타내는 이벤트 객체의 공통 프로퍼티 event.bubbles의 값이 모두 false다.

- 포커스 이벤트 : focus/ blur
- 리소스 이벤트 : load/unload/abort/error
- 마우스 이벤트 : mouseenter/mouseleave

반드시 이벤트를 상위 요소에서 캐치해야 한다면 아래 이벤트를 통해 대체한다.
- 포커스 이벤트 : focusin/focusout
- 마우스 이벤트 : mouseover/mouseout

# 40.7 이벤트 위임

이벤트 위임(event delegation)은 여러 개의 하위 DOM 요소에 각각 이벤트를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.

이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다.
이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.

일반적으로 이벤트 객체의 target 프로퍼티와 currentTarget 프로퍼티는 동일한 DOM 요소를 가리키지만
이벤트 위임을 통해 상위 DOM 요소에 이벤트를 바인딩한 경우 target 프로퍼티는 실제로 이벤트를 발생 시킨 DOM 요소를 가리킨다 때문에 target 프로퍼티와 currentTarget 프로퍼티의 값이 동일하지 않을 수 있다.

# 40.8 DOM 요소의 기본 동작 조작
## 40.8.1 DOM 요소의 기본 동작 중단

DOM 요소는 저마다 기본 동작이있다.
- a 요소 클릭 시 href 어트리뷰트에 지정된 링크로 이동
- checkbox 또는 radio 요소를 클릭하면 체크 또는 헤제

이벤트 객체의 preventDefault 메서드는 이러한 DOM 요소의 기본 동작을 중단시킨다.

```js
document.querySeletor('a').onclcick = e => {
	e.preventDefault();
}
```

## 40.8.2 이벤트 전파 방지

stopPropagation 메서드는 이벤트 전파를 중지시킨다.

```html
<!DOCTYPE html>
<html>
<body>
	<div class="container">
		<button class="btn1">Btn 1</button>
		<button class="btn2">Btn 2</button>
		<button class="btn3">Btn 3</button>
	</div>
	<script>
		document.querySelector('.container').onclick = ({target}) => {
			if(!target.matches('.container > button')) return;
			target.style.color = 'red';
		}
		document.querySelector('.btn2').onclick = e => {
			e.stopPropagation();
			e.target.style.color = 'blue';
		}
	</script>
</body>
</html>
```

container 요소에 이벤트를 위임했다.
하위 DOM 요소에서 발생한 클릭 이벤트를 상위 DOM 요소인 container 요소가 캐치하여 이벤트를 처리한다.

하지만 btn2 요소는 자체적으로 이벤트를 처리한다.
자신이 발생시킨 이벤트가 전파되는 것을 중단하여 자신에게 바인딩된 이벤트 핸들러만 실행되도록한다.

이처럼 stopPropagation 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파를 중단시킨다.

# 40.9 이벤트 핸들러 내부의 this

## 40.9.1 이벤트 핸들러 어트리뷰트 방식

```html
<!DOCTYPE html>
<html>
<body>
	<button onclick="handleClick()">click me</button>
	<script>
		function handleClick() {
			console.log(window); // this
		}
	</script>
</body>
</html>
```

단 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.

```html
<!DOCTYPE html>
<html>
<body>
	<button onclick="handleClick(this)">click me</button>
	<script>
		function handleClick(button) {
			console.log(button); // 이벤트를 바인딩한 button 요소
			console.log(window); // this
		}
	</script>
</body>
</html>
```

handleClick 함수에 전달한 this는 암묵적으로 생성된 이벤트 핸들러 내부의 this다.
즉 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다. 이는 이벤트 핸들러 프로퍼티 방식과 동일하다.

## 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
즉 이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.

```html
<!DOCTYPE html>
<html>
<body>
	<button class="btn1">0</button>
	<button class="btn2">0</button>
	<script>
		const $button1 = document.querySelector('.btn1');
		const $button2 = document.querySelector('.btn2');
		
		$button1.onclick = function (e) {
			console.log(this); // $button1
			console.log(e.currentTarget); // $button1
			console.log(this === e.currentTarget); // true
			
			++this.textContent;
		}
		$button2.addEventListener('click', function (e) {
			console.log(this); // $button2
			console.log(e.currentTarget); // $button2
			console.log(this === e.currentTarget); // true
			
			++this.textContent;
		})
	</script>
</body>
</html>
```

화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다.
화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.

```html
<!DOCTYPE html>
<html>
<body>
	<button class="btn1">0</button>
	<button class="btn2">0</button>
	<script>
		const $button1 = document.querySelector('.btn1');
		const $button2 = document.querySelector('.btn2');
		
		$button1.onclick = e => {
			console.log(this); // window
			console.log(e.currentTarget); // $button1
			console.log(this === e.currentTarget); //false
			
			// window.textContent에 NaN(undefined + 1)을 할당한다.
			++this.textContent;
		}
		$button2.addEventListener('click', e => {
			console.log(this); // window홧
			console.log(e.currentTarget); // $button2
			console.log(this === e.currentTarget); // false
			
			// window.textContent에 NaN(undefined + 1)을 할당한다.
			++this.textContent;
		})
	</script>
</body>
</html>
```

클래스에서 이벤트 핸들러를 바인딩하는 경우 this에 주의해야 한다.

```html
<!DOCTYPE html>
<html>
<body>
	<button class="btn1">0</button>
	<script>
		class App {
			constructor() {
				this.$button = document.querySelector('.btn');
				this.count = 0;
				this.$button.onclick = this.increase;
			}
			increase() {
				// 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
				// 따라서 this.$button은 this.$button.$button가 같다.
				this.$button.textContent = ++this.count;
				// TypeError: Cannot set property 'textContent' of undefined
			}
		}
		new App();
	</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<body>
	<button class="btn1">0</button>
	<script>
		class App {
			constructor() {
				this.$button = document.querySelector('.btn');
				this.count = 0;
				// increase 메서드 내부의 this가 인스턴스를 가리키도록 한다.
				this.$button.onclick = this.increase.bind(this);
			}
			increase() {
				this.$button.textContent = ++this.count;
			}
		}
		new App();
	</script>
</body>
</html>
```

화살표 함수로 정의하면 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.

```html
<!DOCTYPE html>
<html>
<body>
	<button class="btn1">0</button>
	<script>
		class App {
			constructor() {
				this.$button = document.querySelector('.btn');
				this.count = 0;
				this.$button.onclick = this.increase;
			}
			// 클래스 필드 정의
			// increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킨다.
			increase = () => this.$button.textContent = ++this.count;
		}
		new App();
	</script>
</body>
</html>
```

# 40.10 이벤트 핸들러에 인수 전달

이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식에서 인수를 전달하는 방법

```js
const $input = document.querySelector('input[type=text]');
const $msg = document.querySelector('.message');
const checkUserNameLength = min => {
	/* ... */
}
// 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
$input.onblur = () => {
	checkUserNameLength(5);
}
```

```js
const $input = document.querySelector('input[type=text]');
const $msg = document.querySelector('.message');

// 이벤트 핸들러를 반환하는 함수
const checkUserNameLength = min => e => {
	/* ... */
}
$input.onblur = checkUserNameLength(5);
```

# 40.11 커스텀 이벤트

## 40.11.1 커스텀 이벤트 생성

이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트의 종류에 따라 이벤트 타입이 결정된다.
하지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.

이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 한다.

```js
// KeybaordEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent('keyup');
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent('foo');
console.log(customEvent.type); // foo
```

생성된 커스텀 이벤트 객체는 버블링 되지 않으며 preventDefault 메서드로 취소할 수도 없다.
즉 커스텀 이벤트 객체는 bubbles 와 cancelable 프로퍼티의 값이 false로 기본 설정된다.

```js
const customEvent = new MouseEvent('click');
console.log(customEvent.type); // click
console.log(customEvent.bubbles); // false
console.log(customEvent.cancelable); // false
```

커스텀 이벤트 객체의 bubbles 또는 cancelable 을 true로 설정하려면 두번째 인수로 전달한다.

```js
const customEvent = new MouseEvent('click',{
	bubbles: true,
	cancelable: true
});
console.log(customEvent.type); // click
console.log(customEvent.bubbles); // true
console.log(customEvent.cancelable); // true
```

마우스 이벤트 객체 고유의 프로퍼티, 버튼 정보를 나타내는 프로퍼티 등
이벤트 객체 고유의 프로퍼티 값을 지정하려면 다음과 같이 전달한다.

```js
const mouseEvent = newMouseEvent('click', {
	bubles: true,
	cancelable: true,
	clientX: 50,
	clientY: 100
})
```

이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false 다.
커스텀 이벤트가 아닌 사용자 행위에 의해 발생한 이벤트에 생성된 이벤트 객체의 isTrusted 프로퍼티의 값은 언제나 true이다.

```js
const customEvent = new InputEvent('foo');
console.log(customEvent.isTrusted); // false
```

## 40.11.2 커스텀 이벤트 디스패치

생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있다.
dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.

```js
const $button = document.querySelector('.btn');
$button.addEventListener('click', e => {
	console.log(e);
})

const customEvent = new MouseEvent('click');
$button.dispatchEvent(customEvent);
```

일반적으로 이벤트 핸들러는 비동기 처리방식으로 동작하지만
dispatchEvent 메서드는 이벤트 핸들러를 동기처리 방식으로 호출한다.

dispatchEvent를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같다.

따라서 dispatchEvent 메서드로 이벤트를 디스패치하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 한다.

기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 이벤트 객체를 생성하는 경우 일반적으로 CustomEvent 이벤트 생성자 함수를 사용한다.

```js
const customEvent = new CustomEvent('foo');
console.log(customEvent.type); // foo
```

이때 CustomEvent 이벤트 생성자 함수에는 두 번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담은 detail 프로퍼티를 포함하는 객체를 전달할 수 있다.

```js
const customEvent = new CustomEvent('foo', {
	detail: { message: 'Hello' } // 이벤트와 함께 전달하고 싶은 정보
});

const $button = document.querySelector('.btn');
$button.addEventListener('foo', e => {
	console.log(e.detail.message);
})

$button.dispatchEvent(customEvent);
```

기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우
반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 한다.

이벤트 핸들러 어트리뷰트/프로퍼티 방식 을 사용할 수 없는 이유는
'on + 이벤트 타입' 으로 이루어진 요소가 노드에 존재하지 않기 때문이다.
'foo' 이라는 커스텀 이벤트 타입은 'onfoo' 라는 핸들러 어트리뷰트/프로퍼티가 존재하지 않기 때문

# 알고가기
---
- [ ] 이벤트 캡처링, 버블링 에 대해 설명해주세요.
