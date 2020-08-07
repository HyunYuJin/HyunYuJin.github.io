---
layout: post
title: Modularization (모듈화)
date: 2020-05-11 12:09:00
description: 자바스크립트 공부
---

# 스코프 문제

```html
<script src="a.js"></script>
<!-- 요 사이에 찬이가 작성한 c.js를 로딩시켰다. -->
<script src="c.js"></script>
<script src="b.js"></script>
```

```javascript
'use strict'
// 내가 작성한 a.js
var num = 1;

// 찬이가 작성한 c.js
// 어쩌구 저쩌구
var num = 2;

// 내가 작성한 b.js
window.onload = function() {
	'use strict'
	var btnA = document.getElementById('a');
	var btnB = document.getElementById('b');
	btnA.onclick = function() {
		console.log(num);
	};
	btnB.onclick = function() {
		console.log(num);
	}
};
```

버튼 a와 b를 누르면 내가 작성한 a.js의 num이 아니라 찬이가 작성한 b.js의 num인 2가 출력된다.

이는 a.js보다 c.js를 더 늦게 불러왔고 js의 스코프는 모듈 단위가 아니기 때문이다.

이를 극복하는 패턴이 **네이스페이스 패턴**

# 네임스페이스 패턴

찬이가 작성한 스크립트를 어디에서 불러오든 찬이가 util 객체만 건드리지 않는다면 내 코드는 안전을 보장 받을 수 있다.

```javascript
// 내가 작성할 a.js
var util = util || {};
util.name = util.name || 1;

// 부사수가 작성할 c.js
var util2 = util2 || {};
util2.num = util2.num || 2;

// 내가 작성할 b.js
window.onload = function() {
	'use strict';
	var btnA = document.getElementById('a');
	var btnB = document.getElementById('b');
	btnA.onclick = function() {
		console.log(util.name);
	};
	btnB.onclick = function() {
		console.log(util.name);
	};
};
```

하지만 네임 스페이스 패턴을 이용해 모듈화를 진행하면 **전역 스코프**를 더럽히는 단점이 있다. 이를 해결한게 ES2015의 import / export 문법

# import / export 문법

- 지원가능한 브라우저 / 노드가 없다.
    - Babel이라는 트렌스파일러를 이용해서 ES5로 변환해도 Node.js에서 밖에 안되고 브라우저에서는 모듈들을 하나로 합쳐주는 웹팩과 같은 모듈 번들러를 이용할 수 밖에 없다.
- 전역 스코프를 더럽히지 않고 네이밍도 자유로워지고 불러올 때도 입맛에 맞춰서 불러올 수 있다.

```javascript
// export 키워드로 모듈을 내보낸다.(변수라고 보이지만 이 변수가 하나의 일을 수행하는 모듈이 될 수도 있다.)
// a.js
export const a = 11;
export const b = 22;
const ajax = function(url) {
	// 비동기 통신을 하는 함수
};

// 이것을 외부에서 사용할 수 있게 끔 모듈로 만드는데 default라는 것을 이용한다.
// 단  default로 만들 수 있는 모듈은 파일 단 한 개 뿐이다.
export default c;

// b.js
export const a = 33;
export const b = 44;
const getLastIndex = function(arr) {
	// 배열의 마지막 인덱스를 구하는 함수.
};

export default c;

// a 파일에 존재하는 모듈과 b 파일에 존재하는 모듈들을 써서 실제로 앱을 만들어보자
// app.js

// default 키워드로 export 한 모듈은 {} 안에 안 써줘도 된다.
// 그냥 export 한 모듈은 {} 안에 모듈 이름을 적어줘야 한다.
// export 한 모듈을 한번에 다 import하려면 * as A 형식으로 써주면 A 객체의 프로퍼티 모듈이 전부 바인딩 된다.
import ajax, * as A from './a.js';

// default 키워드로 export한 모듈은 이름을 어떻게 불러와도 사용이 가능하다.
// b.js에 a란 모듈을 불러오려고 했는데 이미 a라는 모듈이 불러와졌거나 a라는 이름이 마음에 안들면
// as 라는 별칭 키워드를 써서 _a로 이름을 바꿔서 불러오고 있다.
// b는 export 했지만 안 쓴다면 export하지 않아도 된다.
import getLastIndex, { a as _a } from './b.js';

console.log(A.a); // 11
console.log(A.b); // 22
console.log(_a); // 33

// 모듈을 마음대로 불러다 쓸 수 있다.
ajax('http://www.naver.com');
getLastIndex([1, 2, 3, null, undefined, 7]);
```
