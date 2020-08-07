---
layout: post
title: 인터페이스 (Interface)
date: 2020-05-14 15:09:00
description: 자바스크립트 공부
---

### 인터페이스는 객체나 클래스가 아닌 하나의 **명세**라고 볼 수 있다.

# 인터페이스 예

```javascript
const obj = {
  test: num => Math.pow(num, 2),
  testing: false,
  name: "양간장",
  age: 24,
  etc: "위 키들을 제외하고는 어떠한 키가 오던 상관이 없다."
}
```

* test 인터페이스의 조건을 충족하므로 test 인터페이스를 따르는 객체이다.
  * test라는 키를 가지고 있다.
    * test라는 키의 값은 숫자를 매개변수로 받고 매겨변수로 받은 숫자를 제곱해서 반환한다.
  * testing이라는 키를 가지고 있다.
    * testing이라는 키의 값은 boolean 값을 가진다.
* test2 인터페이스의 조건을 충족하므로 test2 인터페이스를 따르는 객체이다.
  * name이라는 키를 가지고 있다.
    * name이라는 키의 값은 string 값을 가진다.
  * age라는 키를 가지고 있다.
    * age라는 키의 값은 number 값을 가진다.

# 심볼(Symbol)
Symbol은 ES6에서 새로 생긴 7번째 자료형이고 원시값(Object를 제외한 모든 값, 불변값)이다.

* ES5 자료형
  * Boolean
  * String
  * Object
  * Number
  * Null
  * Undefined

### 예외의 버그

```javascript
console.log(0 === -0); // true
console.log(NaN === NaN); // false
console.log(typeof null); // object
```

### 해결방법

```javascript
console.log(Object.is(0, -0)); // false -> Object.is()가 새로 생겼다.
console.log(Object.is(NaN, NaN)); // true
console.log(isNaN(NaN)); // true
console.log(Number.isNaN(NaN)); // true -> ES6에서 isNaN()가 새로 생겼다.
```
