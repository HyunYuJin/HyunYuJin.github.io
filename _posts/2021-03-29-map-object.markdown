---
layout: post
title: Map 객체
date: 2021-03-29
description: key-value 형태로 값을 저장해주는 Map 객체
---
> 💡 Map 객체는 key-value 형태로 값을 저장해줍니다. 그래서 자바스크립트로 해쉬 알고리즘을 풀때 유용하게 쓰이는 내장 함수 입니다.

<br />

### 메서드
<ul>
	<li>set(key, value): Map에 값을 설정할 때 사용합니다.</li>
	<li>get(key): key에 해당하는 value 값을 반환할 때 사용합니다.</li>
	<li>has(key): Map에 key가 존재하는지의 유무를 확인할 때 사용합니다.</li>
	<li>size(): Map의 크기를 구할 때 사용합니다.</li>
</ul>
<hr />
<ul>
	<li>평범한 객체를 Map으로 만들고 싶을때는?</li>
	<code style="color: #FF3636;">Object.entries(obj)</code>
	<li>Map을 객체로 변환하고 싶을 때는?</li>
	<code style="color: #FF3636;">Object.fromEntries(map)</code>
</ul>

<br />

### Map vs Object

#### Key의 type
<ul>
	<li>Map: 어떤 값이든 type으로 지정할 수 있습니다.</li>
	<li>Object: String, Symbol</li>
</ul>

### 순회할 때
<ul>
	<li>Map: Bulit-in interable(반복 가능한 객체). 바로 순회가 가능합니다.</li>
	<li>Object: key의 배열을 얻어 해당 배열을 이용해 순회합니다.(obj[key])</li>
</ul>

<br />

### 크기
<ul>
	<li>Map: size 속성을 이용해서 손쉽게 크기를 얻을 수 있습니다.</li>
	<li>Object: 직접 판별해야 합니다.</li>
</ul>
```javascript
Object.key(obj).length
```

<br />

### 정렬
<ul>
	<li>Map: 삽입한 순서대로 정렬이 됩니다.</li>
	<li>Object: 하지 않습니다. Object는 <code style="color: #FF3636;">prototype</code>을 가지기 때문에 잘못하면 <code style="color: #FF3636;">key</code>의 충돌이 일어날 수 있습니다.</li>
</ul>

<br />

### 그럼 각각 언제 사용할까요?

#### Object
<ul>
	<li>Object는 데이터를 저장하기 위한 간단한 구조입니다.</li>
	<li><code style="color: #FF3636;">Key</code>가 <code style="color: #FF3636;">String</code>이거나 <code style="color: #FF3636;">integer(또는 Symbol)</code>인 경우, Map을 생성하는 것보다 기본 Object를 생성하는 데 훨씬 적은 시간이 걸립니다.</li>
	<li><code style="color: #FF3636;">JSON→Object</code> 또는 <code style="color: #FF3636;">Object→JSON</code>으로 변환해야하는 경우에 사용합니다.</li>
</ul>

#### Map
<ul>
	<li>
		Key의 추가/삭제가 빈번한 경우에 사용합니다.
		<ul><li>Map은 순수한 Hash이고, Object는 그보다 복잡하기 때문에 속도가 Map이 더 뛰어납니다.</li></ul>
	</li>
	<li>Key의 순서가 보장되어야 할 때 사용합니다. Map은 Iteration 기반으로 하여 기본적으로 순서를 유지하기 때문입니다. (Array가 아니어도 반복문을 돌 수 있는 이유는 바로 이 Interation 속성 때문이기 때문이라 합니다!)</li>
	<li>데이터의 크기가 클 때 사용하면 Object를 사용할 때보다 더 나은 성능을 보입니다.</li>
</ul>
