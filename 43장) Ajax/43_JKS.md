# 43. Ajax

## 43.1 Ajax 란

> Ajax 란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여
> 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.
> Ajax 는 브라우저에서 제공하는 Web API XMLHttpRequest 객체를 기반을 동작한다.

> 1999년 마이크로소프트가 개발한 XMLHttpRequest 는 그다지 큰 주목을 받지 못하다가 2005년 구글이 발표한 구글 맵스를 통한
> 웹 어플리케이션 개발 프로그래밍 언어로서 자바스크립트의 가능성을 확인하는 계기를 마련했다.

> 이전의 웹 페이지 html 태그로 시작해서 html 태그로 끝나는 완전한 HTMl 을 서버로부터 전송받아 웹 페이지 전체를 처음부터 다시 렌더링 하는 방식으로 동작했다.

1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함한 완전한 HTML을 서버로부터 매번 다시 전송받기 떄문에 불필요한 데이터 통신이 발생한다.
2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 화면 전환이 일어나면 순간적으로 깜빡이는 현상이 발생한다.
3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 떄문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹 된다.

> Ajax 의 동작은 이전의 전통적인 패러다임을 획기적으로 전환헀다. 서버로부터 웹 페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹 페이지를 변경할 필요가 없는
> 부분은 다시 렌더링하지 않고 변경 할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해진 것이다.

> 이를 통해 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 퍼포먼스와 부드러운 화면 전환이 가능하다.

> Ajax는 전통적인 방식과 비교하였을 때 다음과 같은 장점이 있다.

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## 43.2 JSON

> JSON 은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로
> 대부분의 프로그래밍 언어에서 사용할 수 있다.

### 43.2.1 JSON 표기 방식

> JSON 의 키는 반드시 큰따음표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다.
> 하지만 문자열은 반드시 큰따음표로 묶어야 한다.

### 43.2.2 JSON.stringify

> JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 한다.

### 43.2.3 JSON.parse

> JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면
> JSON 포맷의 문자열을 객체화해야 하는데 이를 역질렬화라 한다.

## 43.3 XMLHttpRequest

> 브라우저는 주소창이나 HTML 의 form 태그 또는 a 태그 를 통해서 HTTP 요청 전송 기능을 기본 제공한다.
> 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API 인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신
> 을 위한 다양한 프로퍼티와 메서드를 제공한다.

### 43.3.1 XMLHttpRequest 객체 생성

> XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다.
> XMLHttpRequest 객체는 브랑줘에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.


```javascript
const xml = new XMLHttpRequest();
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

> XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공한다. 대표적인 프로퍼티와 메서드는 다음과 같다.

1. XMLHttpRequest 객체의 프로토타입 프로퍼티

- readyState
  - HTTP 요청의 현재 상태를 나타내는 정수, 다음과 같은 XMLHttpRequest 의 정적 프로퍼티를 값으로 가진다.
  - UNSENT: 0
  - OPENED: 1
  - HEADERS_RECEIVED: 2
  - LOADING: 3
  - DONE: 4

- status
  - HTTP 요청에 대한 응답 상태를 나타내는 정수

- statusText
  - HTTP 요청에 대한 응답 메세지를 나타내는 문자열

- responseType
  - HTTP 응답 타입

- response
  - HTTP 요청에 대한 응답 몸체, responseType 에 따라 타입이 다르다.

- responseText
  - 서버가 전송한 HTTP 요청에 대한 응답 문자열

2. XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

- onreadystatechange
  - readyState 프로퍼티 값이 변경된 경우

- onloadstart
  - HTTP 요청에 대한 응답을 받기 시작했을 경우

- onprogress
  - HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생

- onabort
  - abort 메서드에 의해 HTTP 요청이 발생했을 경우

- onerror
  - HTTP 요청 에러가 발생했을 경우

- onload
  - HTTP 요청에 성공적으로 완료한 경우

- ontimeout
  - HTTP 요청 시간이 초과했을 경우

- onloadend
  - HTTP 요청이 완료된 경우, 성공 또는 실패 경우 발생

3. XMLHttpRequest 객체의 메서드

- open
  - HTTP 요청 초기화

- send
  - HTTP 요청 전송

- abort
  - 이미 전송한 HTTP 요청 중단.

- setRequestHeader
  - 특정 HTTP 요청 헤더의 값을 설정
  - 특정 HTTP 요청 헤더의 값을 문자열로 반환

4. XMLHttpRequest 객체의 정적 프로파티
   
- UNSENT
  - 0
  - open 메서드 호출 이전

- OPEND
  - 1
  - open 메서드 호출 이후

- HEADERS_RECEIVED
  - 2
  - send 메서드 호출 이후

- LOADING
  - 3
  - 서버 응답 중

- DONE
  - 4
  - 서버 응답 완료

### 43.3.3 HTTP 요청 전송

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```javascript
const xhr = new XMLHttpRequest();

xhr.open('GET', '/users');

xhr.setRequestHeader('content-type', 'apllication/json');

xhr.send();
```

XMLHttpRequest.prototype.open

> open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다.

xhr.open(method, url[, async]);

> HTTP 요청 메서드는 클라이언트가 서버에게 요청한 종류의 목적을 알리는 방법이다.
> 주로 5가지 요청 메서드를 구현한다 (GET, POST, PUT, PATCH, DELETE 등) 을 사용하여 CRUD를 구현한다.

XMLHttpRequest.prototype.send

> send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다.

1. GET 요청의 경우 데이터를 URL 의 일부분인 쿼리 문자열로 서버에 전송한다.
2. POST 요청 메서드의 경우 데이터를 요청 몸체에 담아 전송한다.

> send 메서드에는 요청 몸체에 담아 전송할 데이터로 인수를 전달할 수 있다. 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화 한 다음
> 전송해야 한다.

> HTTP 요청 메서드가 GET 인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null 로 설정된다.

XMLHttpRequest.prototype.setRequestHeader

> setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. 반드시 open 메서드를 호출한 이후에 호출해야 한다.

> Content-type 은 요청 몸체에 담아 전송할 데이터 MIME 타입의 정보를 표현한다.

text
    - text/plain, text/html, text/css, text/javascript

application
    - application/json, application/x-www-form-urlencode

multipart
    - multipart/formed-data

### 43.3.4 HTTP 응답 처리

> 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.

> HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다.

> send 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환한다.
> 하지만 언제 응답이 클라이언트에게 도달할지는 알 수 없다. 따라서 readytstatechange 이벤트를 통해
> HTTP 요청의 현재 상태를 확인해야 한다.

> onreadystatechange 이벤트 핸들러 프로파티에 할당한 이벤트 핸들러는 HTTP 요청의 현재 상태를 나타내는 xhr.readyState 가 XMLHttpRequest.DONE 인지 확인하여
> 서버의 응답이 완료되었는지 확인한다.

> 서버의 응답이 완료되면 HTTP 요청에 대한 응답 상태를 나타내는 xhr.status 가 200인지 확인하여 정상 처리와 에러 처리를 구분한다.

---

- [ ] Ajax 란

