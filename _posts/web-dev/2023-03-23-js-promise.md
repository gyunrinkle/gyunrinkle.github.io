---
layout: post
title: JS Promise
subtitle: 프로미스 나인..
categories: [web-ev]
tags: [javascript, promise]
comments: true
sitemap:
  changefreq: daily
---

# 프라미스

1. producing code는 원격에서 스크립트를 불러오는 것과 같이 시간이 걸리는 일을 한다.
2. consuming code는 producing code의 결과를 기다렸다가 이를 소비한다. 이때 소비 주체(함수)는 여럿이 될 수 있다.
3. promise는 producing code와 consuming code를 연결해주는 특별한 자바스크립트 객체

```javascript
"use strict";

let promise = new Promise(function (resolve, reject) {
  // executor (제작 코드, '가수')
});

```

상기와 같이 `promise`객체를 만들 수 있다.

- `new Promise`에 전달되는 함수는 exectuor(실행자, 실행함수)라 불린다.
- executor의 parameter `resolve`와 `reject`는 js에서 자체 제공하는 callback
	- `resolve(value)`: 일이 성공적으로 끝난 경우 그 결과를 나타내는 `value`와 함께 호출
	- `reject(error)`: error 발생 시 에러 객체를 나타내는 `error`와 함께 호출

요약:
executor는 자동으로 실행되는데 여기서 원하는 일이 처리됨 -> 처리가 끝나면 executor는 처리 성공 여부에 따라 `resolve`나 `reject`를 호출

## `promise` 객체 내부 property
- `state`: 처음엔 `pending`-> `resolve`가 호출되면 `fulfilled`, `reject`가 호출되면 `rejected`로 변함
- `result`: 처음엔 `undefined`-> `resolve(value)`가 호출되면 `value`, `reject(error)`가 호출되면 `error`로 변함


# `then`, `catch`, `finally`

```javascript
"use strict";

promise.then(
  function (result) {
    /* 결과(result)를 다룹니다 */
  },
  function (error) {
    /* 에러(error)를 다룹니다 */
  }
);

```

- `.then`의 첫 번째 인자는 프라미스가 이행됐을 때 실행되는 함수, 여기서 실행결과를 받음
- `.then`의 두 번째 인자는 프라미스가 거부됐을 때 실행되는 함수, 여기서 error를 받음

```javascript
"use strict";

let promise = new Promise(function (resolve, reject) {
  setTimeout(() => resolve("완료!"), 1000);
});

// resolve 함수는 .then의 첫 번째 함수(인수)를 실행합니다.
promise.then(
  (result) => console.log(result), // 1초 후 "완료!"를 출력
  (error) => console.log(error) // 실행되지 않음
);

```

```javascript
"use strict";

let promise = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// reject 함수는 .then의 두 번째 함수를 실행합니다.
promise.then(
  (result) => console.log(result), // 실행되지 않음
  (error) => console.log(error) // 1초 후 "Error: 에러 발생!"을 출력
);

```

```javascript
"use strict";

let promise = new Promise((resolve) => {
  setTimeout(() => resolve("완료!"), 1000);
});

promise.then(console.log); // 1초 뒤 "완료!" 출력


```