---
title: "Fallthrough Attributes"

categories:
  - Vue

toc: true
toc_label: "Contents"
toc_icon: "book"
toc_sticky: true
---

Fallthrough Attributes에 대해 알아보자.

# TL;DR

"폴스루(fallthrough: 대체) 속성"은 컴포넌트에 전달되는 속성 또는 `v-on` 이벤트 리스너 이지만, 이것을 받는 컴포넌트의 `props` 또는 `emits`에서 명시적으로 선언되지 않은 속성이다. 일반적인 예로는 `class`, `style`, `id` 속성이 있다.

# 속성 상속

컴포넌트가 싱글 루트 엘리먼트를 렌더링하면 폴스루 속성이 루트 엘리먼트의 속성에 자동으로 추가된다.

예를 들어 `<BaseButton>` 컴포넌트가 있다고 가정하자.

```html
<!-- <BaseButton>의 템플릿 -->
<template>
  <div>
    <button>클릭</button>
  </div>
</template>
```

그리고 이 컴포넌트를 다음과 같이 부모 컴포넌트에서 사용하면

```html
<BaseButton class="large"></BaseButton>
```

최종 렌더링은 다음과 같다.

```html
<template>
  <div class="large">
    <button>클릭</button>
  </div>
</template>
```

여기서 `<BaseButton>`은 `class`를 허용되는 프로퍼티로 선언하지 않았다. 따라서 `class`는 폴스루 속성으로 처리되어 루트 엘리먼트에 자동으로 추가된다.

# 속성 상속 비활성화

컴포넌트가 속성을 자동으로 상속받지 않도록 하려면, `inheritAttrs: false`를 설정하면 된다.

```html
<template>
  <div>
    <button v-bind="$attrs">클릭</button>
  </div>
</template>

<script setup>
  defineOptions({
    inheritAttrs: false,
  });
</script>
```

그리고 이 컴포넌트를 다음과 같이 부모 컴포넌트에서 사용하면

```html
<BaseButton class="large"></BaseButton>
```

최종 렌더링은 다음과 같다.

```html
<template>
  <div>
    <button class="large">클릭</button>
  </div>
</template>
```

이외에 JavaScript에서 폴스루 속성에 접근할 수 있는 방법도 있으니 아래 Reference를 참고하자.

**Reference**

- [Fallthrough Attributes​](https://vuejs.org/guide/components/attrs)
- [폴스루 속성](https://ko.vuejs.org/guide/components/attrs)
