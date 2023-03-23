---
layout: post
title: prototype 상속
subtitle: 어렵다 어려워
categories: [web-dev]
tags: [javascript, prototype]
comments: true
sitemap:
  changefreq: daily
---

# `[[Prototype]]`

- JS 객체는 명세서에서 명명한 `[[Prototype]]`이라는 숨김 Property를 갖는다.
- 이 숨김 Property는 `null`이거나 다른 객체에 대한 참조가 되는데, 다른 객체를 참조하는 경우 대상을 Prototype이라 부른다.
- 객체에서 property를 읽으려 하는데, 해당 프로퍼티가 없으면 JS는 자동으로 Prototype에서 Property를 찾는다.

```javascript
"use strict";

let animal = {
  eats: true,
};
let rabbit = {
  jumps: true,
};

rabbit.__proto__ = animal; // (*)

// 프로퍼티 eats과 jumps를 rabbit에서도 사용할 수 있게 되었습니다.
console.log(rabbit.eats); // true (**)
console.log(rabbit.jumps); // true

```

- 프로토타입 체인은 하기와 같이 길어질 수 있다.

```javascript
"use strict";

let animal = {
  eats: true,
  walk() {
    console.log("동물이 걷습니다.");
  },
};

let rabbit = {
  jumps: true,
  __proto__: animal,
};

let longEar = {
  earLength: 10,
  __proto__: rabbit,
};

// 메서드 walk는 프로토타입 체인을 통해 상속받았습니다.
longEar.walk(); // 동물이 걷습니다.
console.log(longEar.jumps); // true (rabbit에서 상속받음)


```

# Prototype Chain의 제약 사항

1. 순환 참조는 허용 X. `__proto__`를 이용해 닫힌 형태로 다른 객체를 참조하면 error 발생
2. `__proto__`의 값은 객체나 `null`만 가능. 딴 자료형은 무시.

# Prototype은 읽기 전용

- Prototype은 Property를 읽을 때만 사용한다.
- Property를 추가, 수정하거나 지우려면 객체에 직접해야 된다.

```javascript
"use strict";

let animal = {
  eats: true,
  walk() {
    /* rabbit은 이제 이 메서드를 사용하지 않습니다. */
  },
};

let rabbit = {
  __proto__: animal,
};

rabbit.walk = function () {
  console.log("토끼가 깡충깡충 뜁니다.");
};

rabbit.walk(); // 토끼가 깡충깡충 뜁니다.


```

# `this`가 나타내는 것

- **메서드를 객체에서 호출했든 프로토타입에서 호출했든 상관없이 `this`는 언제나 `.` 앞에 있는 객체이다.**

```javascript
"use strict";

// animal엔 다양한 메서드가 있습니다.
let animal = {
  walk() {
    if (!this.isSleeping) {
      console.log(`동물이 걸어갑니다.`);
    }
  },
  sleep() {
    this.isSleeping = true;
  },
};

let rabbit = {
  name: "하얀 토끼",
  __proto__: animal,
};

// rabbit에 새로운 프로퍼티 isSleeping을 추가하고 그 값을 true로 변경합니다.
rabbit.sleep();

console.log(rabbit.isSleeping); // true
console.log(animal.isSleeping); // undefined (프로토타입에는 isSleeping이라는 프로퍼티가 없습니다.)

```