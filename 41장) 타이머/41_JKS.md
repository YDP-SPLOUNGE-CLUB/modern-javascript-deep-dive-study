# 41. 타이머

## 41.1 호출 스케줄링

> 함수를 명시적으로 호출하면 함수가 즉시 실행된다. 만약 함수를 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면
> 타이머 함수를 사용한다. 이를 호출 스케줄링 이라 한다.

> 자바스클비트는 타이머를 생성할 수 있는 타이머 함수 setTimeout 과 setInterval 타이머를 제거할 수 있는 타이머 함수
> clearTimeout 과 clearInterval 을 제공한다. 타이머 함수는 ECMAScript 사양에 정의된 빌트인 함수가 아니다.

> 하지만 브라우저 환경과 Node.js 환경에서 모두 전역 객체의 메서드로서 타이머 함수를 제공한다.
> 타이머 함수는 호스트 객체이다.

> 타이머 함수 setTimeout 과 setInterval 은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성한다.

> 다시 말해 타이머 함수 setTimeout 과 setInterval 이 생성한 타이머가 만료되면 콜백 함수가 호출된다.

> setTimeout 함수는 생성한 타이머가 한 번 동작하며 setInterval 은 반복 동작한다.

> 자바스크립트 엔진은 단 하나의 실행 컨텍스트 실행 환경을 가지기 때문에
> 두 가지 이상의 테스크를 동시에 실행할 수 없다. 싱글 스레드로 동작한다.

## 41.2 타이머 함수

### 41.2.1 setTimeout / clearTimeout

> setTimeout 함수는 두 번째 인수로 전달받은 시간으로 단 한번만 동작하는 타이머를 생성한다.

1. func
   - 타이머가 만료된 후 호출될 콜백 함수.

2. delay
   - 타이머 만료 시간, setTimeout 함수는 delay 시간으로 단 한번만 동작하는 타이머를 생성
   - delay 시간이 설정된 타이머가 만료되면 콜백 함수가 즉시 호출되는 것이 보장되지는 않는다.

3. param
   - 호출 스케줄링된 콜백 함수에 전달해야할 인수가 있다면 세 번째 인수 이후로 전달할 수 있다.

```javascript
setTimeout((name) => {
    console.log(`Hi ${name}`)
}, 1000, 'Lee');
```

> setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id 를 반환한다. setTimeout 함수가 반환한 타이머 id 는 브라우저 환경인 경우 숮자이며
> Node 환경일 경우 객체다.

> setTimeout 함수가 반환한 타이머 id 를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.
> clearTimeout 은 호출 스케줄링을 취소한다.

```javascript
const timerId = setTimeout(() => {
    console.log('Hi')}, 1000);

clearTimeout(timerId);
```

### 41.2.2 setInterval / clearInterval

> setInterval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다.

> 이후 타이머가 만료될 때마다 첫 번쨰 인수로 전달받은 함수가 반복 호출된다. 타이머가 취소될 때까지 계속된다.

> setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 id 를 반환한다.

> setInterval 함수가 반환한 타이머 id 를 clearInterval 함수의 인자로 전달하면 타이머를 취소할 수 있다.

## 41.3 디바운스와 스로틀

> scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다.
> 이러한 이벤트를 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다. 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를
> 그룹화하여 과도한 이벤트의 호출을 방지하는 프로그래밍 기법이다.

> 다음 에제의 버튼을 짧은 시간 간격으로 연속해서 클릭했을 때 일반적인 이벤트 핸들러와 디바운스, 스로틀된 이벤트 핸들러의 호출 빈도가 어떨게 다른지 살펴보자.

```html
<!DOCTYPE html>
<html>
    <body>
        <button>click Me</button>
        <pre>일반 클릭 이벤트 <span class="normal">0</span></pre>
        <pre>디바운스 클릭 이벤트 <span class="debounce">0</span></pre>
        <pre>스로틀 클릭 이벤트 <span class="throttle">0</span></pre>
        
        <script>
            const button = document.querySelector('button');
            const normalMsg = document.querySelector('.normal');
            const debounceMsg = document.querySelector('.debounce');
            const throttleMsg = document.querySelector('.throttle');
            
            const debounce = (callback, delay) => {
                let timerId;
                return (...args) => {
                    if (timerId) clearTimeout(timerId);
                    timerId = setTimeout(callback, delay, ...args);
                }
            };
            
            const throttle = (callback, delay) => {
                let timerId;
                return (...args) => {
                    if (timerId) return;
                    timerId = setTimeout(() => {
                        callback(...args);
                        timerId = null;
                    }, delay);
                }
            };
            
            button.addEventListener('click', () => {
                normalMsg.textContent = +normalMsg.textContent + 1;
            });

            button.addEventListener('click', debounce(() => {
                debounceMsg.textContent = +debounceMsg.textContent + 1;
            }, 500));

            button.addEventListener('click', throttle(() => {
                throttleMsg.textContent = +throttleMsg.textContent + 1;
            }, 500));
        </script>
    </body>
</html>
```

> 20번 클릭한 경우

> 일반 클릭 이벤트 카운터 20
> 디바운스 클릭 이벤트 카운터 1
> 스로틀 클릭 이벤트 카운터 6

### 41.3.1 디바운스

> 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.

### 41.3.2 스로틀

> 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 작동하도록 한다.


---

생략