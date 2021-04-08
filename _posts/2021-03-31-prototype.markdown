---
layout: post
title: 프로토타입
date: 2021-03-31
description: 프로토타입을 기반으로 하는 언어인 Javascript
---
> 💡 Javascript는 프로토타입을 기반으로 하는 언어입니다.
다른 Java와 같은 객체지향언어는 클래스를 기반으로 하는 언어인데 특이하게도 Javascript는 객체지향 언어이지만 클래스를 기반으로 하지 않는데요, 프로토타입이란 무엇인지에 대해서 알아보겠습니다.

<br />

### 프로토타입이란?
객체를 원형으로 하는 프로토타입은 이를 이용해서 새로운 객체를 만들어나갈 수 있도록 합니다. 때문에 객체를 확장하여 객체지향 프로그래밍을 할 수 있도록 합니다.

Javascript에서 함수를 정의하면 동시에 해당 함수의 <code style="color: #FF3636;">Prototype Object</code>도 동시에 생성됩니다.

사람이 기본적으로 심장을 가지고 있는 것처럼 함수 내부에도 <code style="color: #FF3636;">prototype</code>이라는 것을 기본적으로 가지게 됩니다.

이 <code style="color: #FF3636;">prototype</code>은 <code style="color: #FF3636;">Prototype Object</code>를 참조합니다.

<code style="color: #FF3636;">Prototype Object</code>는 기본적으로 멤버 변수로 <code style="color: #FF3636;">constructor</code>와 <code style="color: #FF3636;">Prototype Link</code>라는 <code>`__proto__`</code>를 가지고 있습니다.

여기서 <code style="color: #FF3636;">constructor</code> 속성은 함수를 참조합니다.

<blockquote>
	⭐️ 여기서!!! <code style="color: #FF3636;">Prototype object</code>를 참조하는 <code style="color: #FF3636;">prototype</code>은 <code style="color: #FF3636;">new</code>라는 연산자와 해당 함수를 통해 생성된 객체의 원형이 되는 객체입니다.
</blockquote>

<br />

모든 객체들은 <code>`__proto__`</code>속성을 가지고 있습니다.  요 친구 덕분에 함수에 직접 연결하지 않고도 함수의 <code style="color: #FF3636;">prototype</code> 객체를 알아보고 연결할 수 있습니다.

<code style="color: #FF3636;">new</code> 생성자로 생성된 객체는 생성되기 위해 사용된 원형인 <code style="color: #FF3636;">Prototype Object</code>를 가리키는 <code>`__proto__`</code>라는 숨겨진 링크로 참조하고 있습니다.

<br />

### 객체 멤버
이해를 쉽게 돕기 위해 저는 오리 이야기🐥를 예로들겠습니다.
<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/prototype01.jpg">
</div>

대빵오리는 함수를 정의하여 오리A, 오리B, 오리C를 만들려고 합니다.
```javascript
function Duck() {
	this.hair = 3;
	this.eyes = 2;
	this.body = 1;
	this.foot = 2;
}

const duckA = new Duck();
const duckB = new Duck();
const duckC = new Duck();
```
→ 하지만 이렇게 함수 안에서 선언해주면 메모리 측면에서는 총 9개가 쌓이게 됩니다.

이를 해결하기 위해서 <code style="color: #FF3636;">prototype</code>을 사용할 수 있습니다.

```javascript
function Duck() {}

Duck.prototype.hair = 3;
Duck.prototype.eyes = 2;
Duck.prototype.body = 1;
Duck.prototype.foot = 2;

Duck.prototype.getType() = function () {
	return '예쁜오리';
}

const duckA = new Duck();
const duckB = new Duck();
const duckC = new Duck();
```

다음처럼 Duck이라는 함수의 <code style="color: #FF3636;">Prototype Object</code>에 멤버 변수로 eyes, body, foot 그리고 <code style="color: #FF3636;">getType()</code>을 추가할 수 있습니다.


만약 duckA을 이용해서 <code style="color: #FF3636;">getType()</code>의 return 값을 수정한다면, 멤버를 수정하는 것이 아니라 해당 객체(duck A)의 **멤버로 추가할** 수 있습니다.
```javascript
...

duckA.prototype.getType() = function () {
	return '미운오리';
}
```

### Prototype Chaining

위의 오리 이야기에서 duckA 객체에서 <code style="color: #FF3636;">getType()</code>이라는 함수를 찾을 때 duckA의 <code>`__proto__`</code>라는 Prototype link를 통해서 <code style="color: #FF3636;">Duck Prototype Object</code>에 있는 <code style="color: #FF3636;">getType</code>를 찾아서 호출합니다.

그런데, 만약에 <code style="color: #FF3636;">Duck Prototype Object</code>에 <code style="color: #FF3636;">getType()</code>라는 함수가 없다면? <code>`__proto__`</code>로 상속 순서에 따라 차근차근 타고 올라가다보면 가장 최상단에 <code style="color: #FF3636;">Object Prototype</code>에 도달하게 됩니다.

<code style="color: #FF3636;">Object prototype</code>에서 상속된 메소드 중에 <code style="color: #FF3636;">getType</code>가 있다면 찾아서 호출하게 됩니다.

이렇게 선언된 객체에서 **Prototype link를 이용해 상위 객체로 변수 또는 함수를 탐색해나가는 과정** 을 <code style="color: #FF3636;">Prototype Chaining</code>이라고 합니다.
