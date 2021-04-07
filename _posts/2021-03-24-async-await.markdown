---
layout: post
title: async-await, try-catch
date: 2021-03-24
description: 함수 실행 순서를 정해주는 async-await와 에러방지코드 try-catch
---

> 💡 오늘은 async-await, 그리고 함께 쓰이는 try-catch에 대해서 정리해보겠습니다.

<br />

### async-await
<code style="color: #FF3636;">async-await</code>는 코드 실행의 순서를 정해주는 역할을 합니다.
함수의 실행 순서를 딱! 고정해주기 위해서라고 생각하면 쉬울 거에요 :)

장점은, 기존의 <code style="color: #FF3636;">callback function</code>, <code style="color: #FF3636;">Promise</code>와 같은 비동기 처리 방식의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성하도록 도와줍니다.

```javascript
async function logName() {
	var user = await fetchUser('domain.com/users/1');

	// await로 순서를 정해준 구문이 완료되어야 실행됩니다.
	if (user.id === 1) console.log(user.name);
}
```

<br />

#### 왜 사용하나요?
네트워크나 파일시스템에 접근하는 것과 같은 무거운 작업을 하는 경우 어떤 작업부터 실행하는지 우린 알수가 없습니다.
이는 Javascript의 비동기라는 특징 때문입니다.
1. 외부 API 작업
2. 앱의 경우, 위치 정보 권한과 같은 휴대폰 자체의 기능

> 정리해서❗
함수의 실행 순서를 꼭 지켜야 하는 부분에서 즉 함수의 실행 순서를 고정하기 위해서 사용하는 것이 async-await입니다.
비동기 처리 방식인 것을 동기처리를 해주기 위해서요!

<br/>

#### async-await 기본 문법
* 일반적으로, <code style="color: #FF3636;">await</code>가 적용되는 비동기 처리 코드는 <code style="color: #FF3636;">Promise</code>를 반환합니다.
```javascript
async function 함수명() {
	await 비동기 처리 메서드 명()
}
```

<br/>

#### async-await 실용 예제
```javascript
// 사용자 정보가 담긴 Promise 객체 반환
function fetchUser() {
	var url = 'https://url/users/1';
	return fetch(url).then(function(response) {
		return response.json();
	})
}

// 할 일 정보가 담긴 Promise 객체 반환
function fetchTodo() {
	var url = 'https://url/todos/1';
	return fetch(url).then(function(response) {
		return response.json();
	})
}

// 할일 제목 출력
async function logTodoTitle() {
	var user = await fetchUser();
	if (user.id === 1) {
		var todo = await fetchTodo();
		console.log(todo.title);
	}
}
```

<br/>

#### try-catch
<code style="color: #FF3636;">try-catch</code>는 에러방지 코드로 주로 API를 요청할 때 자주 사용됩니다.

작업을 할 때 API 호출을 제대로 했을지라도, <code style="color: #FF3636;">서버측</code>에서 혹은 휴대폰과 같은 <code style="color: #FF3636;">기기 자체</code>에서 등 외적으로 오류가 발생할 수가 있는데, 이런 상황들을 처리하기 위한 코드입니다.

* `try{}`에서는 API 요청을 위한 작업 코드 정의합니다.
* `catch{}`에서는 에러가 발생했을 때 실행할 코드를 작성합니다. (어떤 에러가 났는지 확인할 수 있고 에러처리, 후속처리를 해줄 수 있습니다.)

```javascript
async function logTodoTitle() {
	try {
		var user = await fetchUser()
		if (user.id === 1) {
			var todo = await fetchTodo()
			console.log(todo.title)
		}
	} catch (error) {
		console.log(error)
	}
}
```
