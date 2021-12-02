---
title: '브라우저의 동작 원리'

categories:
  - FrontEnd

toc: true
toc_label: 'Contents'
toc_icon: 'book'
toc_sticky: true
---

브라우저가 어떤 과정을 통해 우리가 작성한 마크업을 페이지에 렌더링하는지 알아보자.

# TL;DR

1. HTML 마크업을 처리하고 DOM 트리를 빌드한다. ("**무엇을**" 그릴지 결정한다.)
2. CSS 마크업을 처리하고 CSSOM 트리를 빌드한다. ("**어떻게**" 그릴지 결정한다.)
3. DOM 및 CSSOM을 결합하여 렌더링 트리를 형성한다. ("**화면에 그려질 것만**" 결정한다.)
4. 렌더링 트리에서 레이아웃을 실행하여 각 노드의 정확한 위치 및 크기를 계산한다. ("**Box-Model**"을 생성한다.)
5. 개별 노드를 화면에 페인트한다.

# DOM(Document Object Model)

먼저 HTML 마크업 처리 과정이다. (바이트 → 문자 → 토큰 → 노드 → 객체 모델)

1. 브라우저가 HTML의 원시 바이트를 디스크나 네트워크에서 읽어와서, 해당 파일에 대해 지정된 인코딩에 따라 개별 문자로 변환한다. ex) Characters: `<html><head>...</head><body>...</body></html>`
2. 변환된 문자열을 W3C HTML5 표준에 지정된 고유 토큰으로 변환한다. ex) Tokens: `StartTag: html` `StartTag:head` `...` `EndTag: head` `...`
3. 방출된 토큰은 해당 속성 및 규칙을 정의하는 '객체'로 변환한다. ex) Nodes: `html` `head` `meta` `body` `...`
4. 노드 간의 관계를 트리 데이터 구조로 연결하여 DOM을 생성한다.

브라우저는 HTML 마크업을 처리할 때마다 위의 모든 단계를 수행한다. 즉, 바이트를 문자로 변환하고, 토큰을 식별한 후 노드로 변환하고 DOM 트리를 빌드한다.

DOM 트리는 문서 마크업의 속성 및 관계를 포함하지만 각 노드들이 렌더링될 때 어떻게 표시할지에 대해서는 알려주지 않는다. 이것은 CSSOM의 역할이다.

# CSSOM(CSS Object Model)

브라우저는 단순한 페이지의 DOM을 생성하는 동안 외부 CSS 스타일시트인 style.css를 참조하는 문서의 헤드 섹션에서 링크 태그를 접한다.

HTML과 마찬가지로, 수신된 CSS 규칙을 브라우저가 이해하고 처리할 수 있는 형식으로 변환해야 한다. 따라서 HTML 대신 CSS에 대해 HTML 프로세스를 반복한다.

CSS 바이트가 문자로 변환된 후 차례로 토큰과 노드로 변환되고 마지막으로 CSSOM이라는 트리 구조에 링크된다.

# Render-Tree, Layout, Paint

DOM 트리와 CSSOM 트리를 결합하여 **렌더링 트리**를 형성한다. 이 렌더링 트리는 표시되는 각 노드의 **레이아웃**을 계산하는 데 사용되고 픽셀을 화면에 렌더링하는 **페인트** 단계를 거친다.

DOM트리, CSSOM 트리는 서로 독립적인 객체이다. 이 두 가지를 결합하여 브라우저가 화면에 픽셀을 렌더링하도록 하려면 어떻게 해야 할까?

1. DOM 트리의 루트에서 시작하여 표시되는 노드 각각을 트래버스한다.

   - 일부 노드는 표시되지 않으며(ex: `<script>`, `<meta>`, ...), 렌더링된 출력에 반영되지 않으므로 생략된다.
   - 일부 노드는 CSS를 통해 숨겨지며 렌더링 트리에서도 생략된다. ex) `display: none`

2. 표시된 각 노드에 대해 CSSOM 규칙을 찾아 적용한다.
3. 표시된 노드를 콘텐츠 및 계산된 스타일과 함께 내보낸다.

최종 출력은 화면에 표시되는 모든 노드의 콘텐츠 및 스타일 정보를 모두 포함하는 렌더링 트리이다. 렌더링 트리가 생성되었으므로 '레이아웃' 단계로 진행할 수 있다.

레이아웃 단계에서 브라우저는 렌더링 트리의 루트에서 시작하여 렌더링 트리를 트래버스하고, 뷰포트 내에서 각 노드의 정확한 위치와 크기를 정확하게 캡처하는 Box-Model이 출력된다.

마지막으로, 표시되는 노드의 계산된 스타일 및 위치에 대해 파악했으므로 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환하는 '페인팅' 또는 '래스터화' 단계를 통해 화면에 정보를 전달할 수 있다.

**Reference**

- [Web fundamentals - Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)
- [Interview_for_Beginner - FrontEnd](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/FrontEnd)
