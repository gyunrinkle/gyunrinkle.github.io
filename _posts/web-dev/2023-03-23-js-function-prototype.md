---
layout: post
title: 함수의 prototype 프로퍼티
subtitle: 리터럴 오브젝트의 프로토타입과 뭐가 다른 거지?
categories: [web-dev]
tags: [javascript, function, prototype]
comments: true
sitemap:
  changefreq: daily
---

# 함수의 prototype property

- 리터럴뿐만 아니라 `new F()`와 같이 생성자 함수로도 새로운 객체를 만들 수 있음
- 생성자 함수(`F`)의 prototype을 의미하는 `F.prototype`은 우리가 익히 알고 있는 일반적인 property.

```javascript
"use strict";

let animal = {
  eats: true,
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("흰 토끼"); //  rabbit.__proto__ == animal

console.log(rabbit.eats); // true

```

- `Rabbit.prototype = animal`은 `new Rabbit`을 호출해 만든 새로운 객체의 `[[Prototype]]`을 `animal`로 설정하라는 뜻.
