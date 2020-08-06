---
layout: post
title: Async & Await는 무엇인가?
date: 2020-05-08 10:09:00
description: 지겹도록 많이 본 async & await가 대체 뭐지!
---

#### async & await는 무엇인가?
기존의 비동기 처리 방식인 콜백 함수와 Promise의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 도와준다.

```javascript
	async function logName() {
		var user = await fetchUser('domain.com/users/1')
		if (user.id === 1) {
			console.log(user.name)
		}
	}
```

#### async & await 적용된 코드와 그렇지 않은 코드
자바스크립트의 비동기 처리 코드가 실행 순서를 보장 받을 수 있는 방법

##### 콜백 함수를 사용
  ```javascript
  function logName() {
  	var user = fetchUser('domain.com/users/1', function(user) {
  		if(user.id === 1) {
  			console.log(user.name)
  		}
  	})
  }
  ```

##### async & await
  ```javascript
  async function logName() {
  	var user = await fetchUser('domain.com/users/1')
  	if(user.id === 1) {
  		console.log(user.name)
  	}
  }
  ```

<br/>
#### async & await 기본 문법

```javascript
async function 함수명() {
	await 비동기 처리 메서드 명()
}
```

일반적으로 await의 대상이 되는 비동기 처리 코드는 Axios 등 Promise를 반환하는 API 호출 함수다.

<br/>

##### 예제

```javascript
function fetchItems() {
	return new Promise(function (resolve, reject) {
		var items = [1, 2, 3];
		resolve(items)
	})
}

async function logItems() {
	var resultItems = await fetchItems()
	console.log(resultItems)
}
```

await를 사용하지 않았다면 데이터를 받아온 시점에 console을 출력할 수 있게 콜백 함수나 .then()을 사용해야 했을 것이다.
하지만 async await 문법 덕에 비동기에 대한 사고를 하지 않아도 되는 것이다.

<br/>
###### async & await 실용 예제

```javascript
// 사용자 정보가 담긴 Promise 객체 반환
function fetchUser() {
	var url = 'https://url/users/1'
	return fetch(url).then(function(response) {
		return response.json();
	})
}

// 할 일 정보가 담긴 Promise 객체 반환
function fetchTodo() {
	var url = 'https://url/todos/1'
	return fetch(url).then(function(response) {
		return response.json();
	})
}

// 할일 제목 출력
async function logTodoTitle() {
	var user = await fetchUser()
	if (user.id === 1) {
		var todo = await fetchTodo()
		console.log(todo.title);
	}
}
```

**장점: 기존의 비동기 처리 코드 방식으로 사고하지 않아도 된다.**

<br/>

#### async & await 예외 처리
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
