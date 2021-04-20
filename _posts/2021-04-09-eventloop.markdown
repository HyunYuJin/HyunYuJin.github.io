---
layout: post
title: 이벤트 루프
date: 2021-04-09
description: 이벤트 루프를 통해서 단일 스레드 극복하기
---

> 💡 사실상 JS 자체에 비동기성이라는 직접적인 개념이 있지는 않다고 합니다.
왜냐면 JS는 단일 스레 프로그래밍 언어이기 때문이죠. 단일 스레드란, Call Stack이 하나라는 뜻입니다.
하 그럼 Call Stack은 뭐고 이벤트 루프는 무엇이냐! 한번 알아봅시당 :)

<br />

### JS Engine
JS Engine은 <span style="color: #9777ff;">Memory Heap</span>과 <span style="color: #239dd9;">Call Stack</span>으로 구성되어 있습니다. (가장 유명한 것으로는 Google의 V8 Engine이라고 하네요!)

<ul>
  <li>
    <span style="color: #9777ff;">Memory Heap</span>:
    메모리할당이 일어나는 곳입니다.
    예를들어서, 우리가 선언한 함수, 변수 등이 담겨져 있습니다.
  </li>
  <li>
    <span style="color: #239dd9;">Call Stack</span>:
    코드가 실행될 때 쌓이는 곳으로 stack 형태로 쌓입니다.
  </li>
</ul>

<br />

### Web API
Web API는 JS Engine이 아닙니다. 
이는 브라우저에서 제공하는 API로,
<ul>
  <li>DOM</li>
  <li>AJAX</li>
  <li>Timeout</li>
</ul>
과 같은 것들이 있습니다.
Call Stack에 있는 비동기 함수들은 Web API로 밀어넣어지고 Web API의 callback 함수들은 Callback Queue에 밀어넣어집니다.

<br />

### Callback Queue
비동기적으로 실행될 Callback 함수가 보관(?)되는 공간으로, 이름대로 선입선출 룰을 따릅니다.

<br />

### 이벤트 루프 동작 순서
<ol>
  <li>우리가 작성한 코드가 순서대로 Call Stack에 담깁니다.</li>
  <li>비동기 함수가 있다면, Call Stack에서 실행된 비동기 함수는 Web API로 밀려들어아게되고 그에 대한 callback 값이 Callback Queue로 들어갑니다.</li>
  <li>Event Loop가 돌면서 Call Stack과 Callback Queue의 상태를 체크하여, Call Stack이 빈 상태가 되면 Callback Queue에 있는 내용을 Call Stack으로 밀어넣어줍니다.</li>
</ol>  

<div style="background-color: #9AABDB; margin: 10px 0 20px; padding:  0.5rem 0.7rem; border-radius: 5px">
  <p style="color: #444; font-weight: 600;">이러한 <code style="color: #FF3636;">Web API</code>, <code style="color: #FF3636;">Callback Queue</code>, <code style="color: #FF3636;">Event Loop</code>와 같은 기능들 덕분에 JS가 멀티스레드와 같은 기능을 수행할 수 있는 것입니다.🎉</p>
</div>

<br />

#### 예시코드

```javascript
function duckHello() {
    console.log("Hello! I am pretty duck.");
}

function duckRespond() {
  return setTimeout(() => {
    return "Hey! I am ugly duck.";
  }, 1000);
}

duckHello();
duckRespond();
```

<ol>
  <li> 함수를 호출하면 그 함수는 'Call Stack'에 추가됩니다.

  쉽게 생각해서 이는 그냥 스택이라고 생각하면 되는데, 선입후출로 실행됩니다.

  따라서 해당 함수가 값을 return하면 그 함수는 스택에서 빠져나오게 됩니다.
    <ol>
      <li>우선, duckRespond()가 Call Stack에 추가됩니다.</li>
      <li>그 다음 setTimeout()가 Call Stack에 추가됩니다.</li>
      <li>setTimeout()의 콜백 함수인 () => { return "Hey! I am ugly duck."; }가 WEB API에 추가됩니다.</li>
      <li>그동안 순차적으로 setTimeout()과 duckRespond()가 Call Stack에서 빠져나옵니다. (선입후출!)</li>
    </ol>
  </li>
  <li>WEB API 내에서는 setTimeout()에서 지정해준 시간동안 타이머가 돌아갑니다.</li>
  <li>타이머가 종료된 후 해당 콜백 함수는 Callback Queue라는 공간에 넣어지게 됩니다.

  해당 콜백 함수는 자신의 차례가 될 때까지 Callback Queue에서 대기합니다.
    <div style="background-color: #eee; padding: 1rem;">
      ⚠️ 콜백 함수가 지정해준 시간 이후에 바로 Call Stack에 추가되어 값을 return하는 것이 아닙니다!! Callback Queue라는 공간에 먼저 푸쉬 됩니다!
    </div>
  </li>
  <li>대망의 이벤트 루프!가 등장하게 됩니다.

  만약, Call Stack내의 함수들이 모두 값을 return하고 Call Stack이 비워지면

  <ol>
    <li>Callback Queue에 있는 <del>첫번째 항목부터</del> Call Stack에 추가됩니다.
    </li>
    <li>해당 항목이 값을 return합니다.</li>
    <li>Call Stack에서 해당 항목이 pop 됩니다.</li>
  </ol>
  </li>
</ol>


<br />
<hr />
<br />

##### 참고
[https://kkangdda.tistory.com/73](https://kkangdda.tistory.com/73)

[https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84](https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84)
