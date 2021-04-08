---
layout: post
title: 동기처리, 비동기처리
date: 2021-04-08
description: 단일 스레드로 구성된 JS가 여러 일을 동시에 할 수 있는 이유!
---

> 💡 Js는 한번에 하나의 일만 수행할 수 있는 단일 스레드로 구성이 되어있습니다.
그런데 한의 일이 30초 이상 걸리면 어떨까요? Js는 브라우저의 메인 스레드에서 돌아가기 때문에 모든 UI가 막히게 될 것입니다.
다행히도 JS에서는 WEB API를 제공하여 비동기 처리를 할 수 있도록 해주는데요, 오늘은 동기와 비동기에 대해서 정리해보겠습니다.

<br />

### 동기란?
동기는 말 그대로, 동시에 일어난다는 것을 의미합니다.
요청과 결과가 동시에 일어나는 것인데요, 시간이 오래걸리든, 짧게 걸리든 요청한 결과가 바로 보여져야 합니다.

<br />

### 비동기란?
이와 반대로, 동시에 일어나지 않는 것을 의미합니다.
요청한 결과가 그 자리에서 바로 보여지지 않는다는 것을 약속합니다.

이처럼 단일 스레드로 구성된 언어에서는 비동기 처리가 필수인데요,
비동기 처리방식을 사용할 때 주의할 점은 언제 결과 값을 반환할지 모르기 때문에 동기로 처리하는 방법을 적절하게 섞어서 사용해야하는 법을 알아야 합니다.

대표적으로 setTimeout, Promise, callback, async-await와 같은 처리방법이 있습니다.

<!-- 사실 저는 setTimeout, async-await를 많이 사용해서 Promise, callback을 제대로 사용해보지 못했기 때문에 연습이 더 필요할 것 같습니다. -->

<br />

### 동기적인 Javascript
```javascript
function firset {
  setTimeout(() => {
    console.log("The First function has been called.");
  }, 1000);
}

function second() {
  setTimeout(() => {
    console.log("The Second function has been called.");
  }, 500);
}

first();
second();
```

first 함수가 먼저 호출되고 second 함수가 그 다음에 호출이 됩니다.
하지만 first 함수의 setTimeout을 통해 1초 뒤에 문장이 출력된다하면,
second 함수의 setTimeout을 통해 0.5초 뒤에 문장이 출력되기 때문에 출력 값은 다음과 같습니다.

```bash
The Second function has been called.
The First function has been called.
```

그런데, 참으로도 이상합니다. JS는 단일 스레드라 한번에 하나의 작업밖에 못하죠?
어떻게 더 늦게 호출된 second 함수의 결과 값이 앞서 호출한 firest 함수의 결과 값보다 먼저 출력 될까요?
**동시성** 이 어떻게 가능할까요?!

이와 관련해서는 JS의 엔진의 구조부터 파악해야합니다.
Event Loop에 관한 설명은 다음을 참고해주세요!

<br />

### Promise
promise는 어떤 작업이 성공했을 때는 resolve를 호출하고,
실패를 했을 경우에는 reject를 호출합니다.

함수 실행이 성공했을 때 then()으로 받을 수 있고
함수 실행이 실패했을 때 catch()로 받을 수 있습니다.

그래서 장점은 성공과 실패를 모두 잡을 수 있고 콜백 지옥을 해결할 수 있다고 합니다.

```javascript
const myPromise = new Promise((resolve, reject) => {
  console.log('myPromise');
  setTimeout(() => {
    resolve('Success!');
  });
});

myPromise.then((response) => {
  console.log(response);
}).catch((error) => {
  console.error(error);
});
```

<br />

### async-await
ES8에서 새로 소개된 문법입니다.

저는 처음에 async-await를 이해하기까지 시간이 좀 걸렸는데요, (사실 아직도 완벽하게 이해한건지 모르겠습니다..)
Promise를 잘 알고 있을수록 이해하기가 쉽습니다.
```javascript
(async function asyncFunction() {
  var result = await new Promise(resolve => {
    setTimeout(() => {
        resolve('resolved');
      }, 2000);
  });
  console.log(result);
  // "resolved"
})();
```

<ul>
  <li>async function은 내부에 await를 쓰기 위해 사용합니다.</li>
  <li>await는 반환된 Promise의 resolve 상태가 될때까지 대기합니다.</li>
  <li>그리고 resolve 값이 반환됩니다.</li>
  <li>만약 중간에 reject를 만나면 해당 async function 스코프의 작업이 중단되며, 이후의 스코프 작업이 이어집니다.</li>
</ul>

#### try-catch
try-catch문으로 await에서 발생한 reject를 catch할 수 있습니다.

```javascript
(async function() {
  try {
    // Promise resolve
    var a = await new Promise((resolve, reject) => {
      resolve(10);
    });
    console.log(a);
    // 10

    // async function 원시값 반환
    var b = await (async function() {
      return 20;
    })();
    console.log(b);
    // 20

    // async function Promise 반환
    var c = await (async function() {
      return new Promise(resolve => {
        resolve(30);
      });
    })();
    console.log(c);
    // 30

    // Promise reject
    var d = await new Promise((resolve, reject) => {
      reject(40);
    });
    console.log(d);
  } catch (e) {
    console.log(e);
    // 40
  }
})();
```
