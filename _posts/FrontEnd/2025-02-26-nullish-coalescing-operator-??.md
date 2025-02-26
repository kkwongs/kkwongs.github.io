---
title: "nullish 병합 연산자 '??'"

categories:
  - JavaScript

toc: true
toc_label: "Contents"
toc_icon: "book"
toc_sticky: true
---

nullish 병합 연산자 '??'를 알아보자.

# TL;DR

- nullish 병합 연산자 `??`를 사용하면 피연산자 중 '값이 할당된' 변수를 빠르게 찾을 수 있다.

```javascript
// height가 null이나 undefined인 경우, 100을 할당
height = height ?? 100;
```

- `??`의 연산자 우선순위는 대다수의 연산자보다 낮고 `?`와 `=` 보다는 높다.
- 괄호 없이 `??`를 `||`나 `&&`와 함께 사용하는 것은 금지되어있다.

```javascript
let x = 1 && 2 ?? 3; // SyntaxError: Unexpected token '??'
let x = (1 && 2) ?? 3; // 제대로 동작
```

# `??`와 `||`의 차이

- `||`는 첫 번째 truthy 값을 반환합니다.
- `??`는 첫 번째 정의된(defined) 값을 반환합니다.

```javascript
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

`height || 100`은 `height`에 `0`을 할당했지만 `0`을 falsy 한 값으로 취급했기 때문에 `null`이나 `undefined`를 할당한 것과 동일하게 처리한다. 따라서 `height || 100`의 평가 결과는 `100`이다.

반면 `height ?? 100`의 평가 결과는 `height`가 정확하게 `null`이나 `undefined`일 경우에만 100이 된다.

이런 특징 때문에 높이처럼 `0`이 할당될 수 있는 변수를 사용해 기능을 개발할 땐 `||`보다 `??`가 적합하다.

# 연산자 우선순위

[`??`의 연산자 우선순위](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_precedence#table)는 5로 꽤 낮다.

**Reference**

- [nullish 병합 연산자 '??'](https://ko.javascript.info/nullish-coalescing-operator)
