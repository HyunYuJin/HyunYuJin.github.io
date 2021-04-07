---
layout: post
title: 🔙 Javascript 뒤로가기 버튼 막기 이벤트 제어
date: 2020-07-27
description: 페이지에서 임의로 뒤로가기 버튼 막기
---

### 📜 History control (popState / pushState)
안타깝게도 브라우저 엔진 안에서 구동되는 웹 프로그램의 구조상 한계 때문에 웹 프로그램 내 스크립트를 통해 100% 제어할 수 있는 방법은 없습니다.

그래도 제한적으로나마 <code style="color: #FF3636;">pushStat</code>e와 <code style="color: #FF3636;">popState</code> 이벤트를 이용하여 뒤로가기 버튼 이벤트를 핸들링할 수 있습니다.

<br />

#### history.pushState ⤵️
페이지 진입시 먼저 <code style="color: #FF3636;">history.pushState</code> 인터페이스를 통해 새로운 <code style="color: #FF3636;">history state</code>를 추가하여 바로 이전 페이지로 갈 수 없도록 합니다.
```javascript
history.pushState(state, title, url);
// state: 새 history 항목과 관련된 Javascript 객체.
// title: 새 history 항목의 title
// url: 새 history 항목의 url
```

<br />

#### history.popState ⤴️
이후 페이지가 뒤로가면서 생기는 <code style="color: #FF3636;">history</code>의 변경을 <code style="color: #FF3636;">popState</code> 이벤트로 캐치하여 처리하는 방식입니다.

<code style="color: #FF3636;">history</code>가 변경되는 시점에 window 내에서 핸들링 된 히스토리 변경이 아닌 외부에 의한 변경인지 판단합니다.

```javascript
window.onpopstate = ((event) => {
    history.go(event.currentTarget.length);
});
```
