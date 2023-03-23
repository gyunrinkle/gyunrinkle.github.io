---
layout: post
title: js async와 await
subtitle: 어렵군
categories: [web-dev]
tags: [async, await, javascript]
comments: true
sitemap:
  changefreq: daily
---

# async 함수

```javascript
"use strict";

async function f() {
  return 1;
}

```
- function 앞에 `async` 키워드를 붙이면 해당 함수는 항상 `promise`를 반환
- `promise`가 아닌 값이라도 fulfilled 상태의 resolved promise로 값을 감싸 이행된 promise를 반환

```javascript
"use strict";

async function f() {
  return 1;
}

f().then(console.log); // 1

```

- 상기 코드로 확인 가능
- 하기와 같이 명시적으로 promise 반환도 가능

```javascript
"use strict";

async function f() {
  return Promise.resolve(1);
}

f().then(console.log); // 1

```

# `await`

- JS는 `await` 키워드를 만나면 `promise`가 처리될 때까지 기다린다.
- 결과는 그 이후에 반환된다.

```javascript
"use strict";

async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000);
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  console.log(result); // "완료!"
}

f();

```

- `await`는 `promise.then`보다 좀 더 세련되게 `promise`의 result를 얻게 해주는 문법
- `promise.then`보다 가독성이 좋다.

# 에러 핸들링

- `promise`가 정상적으로 이행되면 `await promise`는 promise 객체의 result에 저장된 값을 반환
- 거부되면 throw문을 작성한 것처럼 에러가 던져짐

```javascript
"use strict";

async function f() {
  await Promise.reject(new Error("에러 발생!"));
}

console.log(f());

```

```javascript
"use strict";

async function f() {
  throw new Error("에러 발생!");
}

console.log(f());

```

- 상기 두 코드는 같은 결과를 발생시킴

```javascript
"use strict";

async function f() {
  try {
    let response = await fetch(
      "https://port-0-hello-spring-3xcah2glbjfmfti.gksl2.cloudtype.app/cake_lists"
    );
    let 케이크 = await response.json();
    console.log(케이크);
  } catch (err) {
    // fetch와 response.json에서 발행한 에러 모두를 여기서 잡습니다.
    console.log(err);
  }
}

f();

```

- 상기 코드와 같은 구조로 코딩 가능



