---
layout: post
title: Javascript 4가지의 for문 차이
date: 2020-08-05 15:09:00
description: 도대체 Basic for, forEach, for of, for in의 차이가 뭐야!
---

#### 보통 자바스크립트의 for문은 4가지 형태가 있다.

<ul>
	<li>Basic for</li>
	<li>forEach</li>
	<li>for in</li>
	<li>for of</li>
</ul>

그렇다면 이들의 차이점은 무엇일까?

    const array = ['A', 'B', 'C', 'D'];
    console.log('Basic');

    for (let i = 0; i < array.length; i++) {
			console.log(array[i]);
    }

    console.log('ForEach');
    array.forEach(function(j) {
      console.log(array[j]);
    });

    console.log('For in');
    for (let k in array) {
      console.log(array[k]);
    }

    console.log('For of');
    for (let z of array) {
      console.log(array[z]);
    }

    return 0;

##### 결과 값은 다음과 같다.
이전에 검색해봤을 때는 for of인 경우는 Symbol.iterator일 경우만 가능하다나 그래서 잘 안와닿았는데 쉽게 말하자면 이렇단다.

1. Basic for와 for in의 경우는 반복 변수에 index를 return 하지만
2. forEach와 for of는 해당 값을 return한다.

forEach의 콜백함수의 매개변수는 3가지다.

첫번째로는 해당 값, 두번째로는 index, 세번째는 배열.
하지만 위에서는 하나의 매개변수만 설정되어 있기 때문에 해당 값이 return된 것이다.

	const array = ['A', 'B', 'C', 'D'];

	console.log('ForEach');
	array.forEach(function(j) {
		console.log(j);
	});

	console.log('For of');
	for (let z of array) {
		console.log(z);
	}

다음과 같이 수정하면 값을 출력할 수 있다.
<br/>
##### 또한 for in의 경우는 Object에서도 쓸 수 있다.
	const object = { 1: "A", 2: "B", 3: "C", 4: "D" };
	for (let z in object) {
	  console.log(object[z]);
	}

<br/>
### 결론
forEach와 for of는 **배열의 반복**에 사용된다.

forEach의 경우는 **callback 함수가 필요해서** 굳이 이걸 쓸 필요 없이 같은 기능을 하기 위해 for of가 등장한 것이다.

<br/>
<hr>
<br/>
참고문헌: <a href="https://aljjabaegi.tistory.com/354">알짜배기 프로그래머의 블로그</a>
