# 30. Date

> 빌트인 객체인 Date 는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수이다.

> 현재 날짜와 시간은 자바스크립트 코드가 실행되는 시스템 시계에 의해 결정된다.

## 30.1 Date 생성자 함수

> Date 는 생성자 함수다.
> 내부적으로 날짜와 시간을 나타내는 정수값을 가진다. 
> 1970년 1월 1일 0시를 나타내는 Date 객체는 내부적으로 정수값 0 을 가지며
> 하루가 지난 1월 2일 0시를 나타내는 Date 객체는 내부적으로 정수값 86400000 을 갖는다.

### 30.1.1 new Date()

> 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

> Date 생성자 함수를 new 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

### 30.1.2 new Date(milliseconds)

> 인수로 숫자 타입 밀리초를 인수로 전달하면 1970년 1월 1일 0시 를 기점으로 인수로 저달된 만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

### 30.1.3 new Date(dateString)

> 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

```javascript
new Date('May 26, 2020 10:00:00');
// The May 26 2020 10:00:00 GMT+0900 *대한민국 표준시
```

### 30.1.4 new Date(year month[, day, hour, minute, second, millisecond])

> 생성자 함수에 연 월 일 시 분 초 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체가 반환된다.

> 이때 연 월은 반드시 지정해야한다. 연 월을 지정하지 않은 경우 1970년 1월 1일 0시를 나타내는 객체를 반환한다.

## 30.2 Date 메서드

### 30.2.1 Date.now

> 1970년 1월 1일 0시를 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

### 30.2.2 Date.parse

> 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

### 30.2.3 Date.UTC

> 1970년 1월 1일 0시를 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

> Date.UTC 는 new Date(year month[, day, hour, minute, second, millisecond]) 같은 형식의 인수를 사용해야 한다.
> KST 가 아닌 UTC 로 인식된다.

### 30.2.4 Date.prototype.getFullYear

> Date 객체 연도를 나타내는 정수를 반환한다.

```javascript
new Date('2020/07/26').getFullYear(); // 2020
```

### 30.2.5 Date.prototype.setFullYear

> 연도를 나타내는 정수를 설정한다. 월, 일 설정도 가능하다.

```javascript
const today = new Date();

today.setFullYear(1900, 0 , 1);
today.getFullYear(); // 1900
```

### 30.2.6 Date.prototype.getMonth

> Date 객체의 월을 나타내는 0 ~ 11 정수를 반환한다.

### 30.2.7 Date.prototype.setMonth

> Date 객체의 월을 나타내는 0 ~ 11 정수를 설정한다.

### 30.2.8 , 9 Date.prototype.getDate, setDate

> 날짜를 나타내는 정수 반환 및 설정

### 30.2.10 Date.prototype.getDay

> 요일을 나타내는 정수를 반환한다.

### 30.2.11 ~ 12 Date.prototype.getHours, setHours

> 시간을 반환 및 설정

### 30.2.13 ~ 14 Date.prototype.getMinutes, setMinutes

> 분을 반환 및 설정

### 30.2.15 ~ 16 Date.prototype.getSeconds, setSeconds

> 초 반환 및 설정

### 30.2.17 ~ 18 Date.prototype.getMilliseconds, setMilliseconds

> 밀리초 반환 및 설정

### 30.2.19 ~ 20 Date.prototype.getTime, setTime

> 경과된 시간을 밀리초로 반환한다 or 설정한다.

### 30.2.21 Date.prototype.getTimezoneOffset

> 로컬 시간과의 차이를 분 단위로 반환한다.

### 30.2.22 Date.prototype.toDateString

> 사람이 읽을 수 있는 형식의 문자열로 반환한다.

### 30.2.23 Date.prototype.toTimeString

> 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```javascript
const today = new Date('2020/07/24/12:30');

today.toTimeString(); // 12:30:00 GMT+0900
```

### 30.2.24 Date.prototype.toISOString

> ISO 8601 형식으로 객체의 날싸, 시간을 표현한 문자영을 반환한다.

### 30.2.25 Date.prototype.toLocaleString

> 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열로 반환


```javascript
const today = new Date('2020/07/24/12:30');

today.toString();
today.toLocaleString();
today.toLocaleString('ko-kr');
today.toLocaleString('en-US');
today.toLocaleString('ja-JP');
```

### 30.2.26 Date.prototype.toLocaleTimeString

```javascript
const today = new Date('2020/07/24/12:30');

today.toString();
today.toLocaleTimeString();
today.toLocaleTimeString('ko-kr');
today.toLocaleTimeString('en-US');
today.toLocaleTimeString('ja-JP');
```

## 30.3 Date 를 활용한 예제

```javascript
(function printNow() {
    const today = new Date();
    
    const dayNames = [
        '(일요일)',
        '(월요일)',
        '(화요일)',
        '(수요일)',
        '(목요일)',
        '(금요일)',
        '(토요일)',
    ]
    
    const day = dayNames[today.getDay()];
    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const date = today.getDate();
    let hour = today.getHours();
    let minute = today.getMinutes();
    let second = today.getSeconds();
    const ampm = hour >= 12 ? 'PM' : 'AM';
    
    hour %= 12;
    hour = hour || 12;
    
    minute = minute < 10 ? '0' + minute : minute;
    second = second < 10 ? '0' + second : second;
    
    const now = `${year}년 ${month}월 ${data}일 ${day} ${hour}:${minute}:${second}`

    console.log(now);
    
    setTimeout(printNow, 1000);
})();
```

---

생략

