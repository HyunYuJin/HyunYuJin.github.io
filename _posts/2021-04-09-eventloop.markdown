---
layout: post
title: 이벤트 루프
date: 2021-04-09
description: 이벤트 루프를 통해서 단일 스레드 극복하기
---

> 💡 사실상 JS 자체에 비동기성이라는 직접적인 개념이 있지는 않다고 합니다.
> JS엔진은 호출할 때마다 프로그램의 실행을 시간에 따라 처리합니다.

<br />

### 이벤트 루프 동작 순서
#### 예시코드
해당 예시코드를 바탕으로 하겠습니다.

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
<br />
<ol>
  <li> 함수를 호출하면 그 함수는 'call stack'이라는 곳에 추가됩니다.

  쉽게 생각해서 이는 그냥 스택이라고 생각하면 되는데, 선입후출로 실행됩니다.

  따라서 해당 함수가 값을 return하면 그 함수는 스택에서 빠져나오게 됩니다.
    <ol>
      <li>우선, duckRespond()가 call stack에 추가됩니다.</li>
      <li>그 다음 setTimeout()가 call stack에 추가됩니다.</li>
      <li>setTimeout()의 콜백 함수인 () => { return "Hey! I am ugly duck."; }가 WEB API에 추가됩니다.</li>
      <li>그동안 순차적으로 setTimeout()과 duckRespond()가 call stack에서 빠져나옵니다. (선입후출!)</li>
    </ol>
  </li>
  <li>WEB API 내에서는 setTimeout()에서 지정해준 시간동안 타이머가 돌아갑니다.</li>
  <li>타이머가 종료된 후 해당 콜백 함수는 Queue라는 공간에 넣어지게 됩니다.

  해당 콜백 함수는 자신의 차례가 될 때까지 Queue에서 대기합니다.
    <div style="background-color: #eee; padding: 1rem;">
      ⚠️ 콜백 함수가 지정해준 시간 이후에 바로 call stack에 추가되어 값을 return하는 것이 아닙니다!! Queue라는 공간에 먼저 푸쉬 됩니다!
    </div>
  </li>
  <li>대망의 이벤트 루프!가 등장하게 됩니다.

  이벤트 루프는 Queue와 call stack을 이어주는 역할을 합니다.

  만약, call stack 내의 함수들이 모두 값을 return하고 call stack이 비워지면

  <ol>
    <li>Queue에 있는 첫번째 항목부터 call stack에 추가됩니다.</li>
    <li>해당 항목이 값을 return합니다.</li>
    <li>call stack에서 해당 항목이 pop 됩니다.</li>
  </ol>
  </li>
</ol>


<br />
<hr />
<br />
##### 참고
[https://kkangdda.tistory.com/73](https://kkangdda.tistory.com/73)
