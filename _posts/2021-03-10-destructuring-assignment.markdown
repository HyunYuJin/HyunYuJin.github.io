---
layout: post
title: 구조분해할당
date: 2021-03-10
description: 변수처럼 꺼내쓰는 구조분해할당
---

> 💡 구조분해 할당 방식은 JS에서 많이 사용되는 자료구조인 Object와 Array에서 특정 데이터만을 변수처럼 꺼내와 사용하고 싶을 때 사용하는 방식으로 이름처럼 정말 분해하는 것처럼 사용합니다.
특히 React를 이용해서 개발할 때 자주 볼 수 있는 방식입니다.

<br/>

#### 예시
```javascript
const example = { a: 123, b: { c: 234, d: 345 } };
const { a, b: {d} } = example; // 이렇게 간결하고 헷갈리지 않게 써줄 수 있다.
console.log(a); // 123
console.log(d); // 345

const arr = [1, 2, 3, 4, 5];
const x = arr[0];
const y = arr[1];
const x = arr[4];
const [x, y, , , z] = arr; // 공백 부분은 다른 변수에 할당하지 않겠다는 뜻이다.
```

→ <code style="color: #FF3636;">this</code>를 사용하는 함수의 경우는 구조분해 할당을 사용하지 않는게 좋습니다.
