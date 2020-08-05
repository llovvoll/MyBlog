---
layout: single
title: "只要一行 CSS 實現錨點連結跳轉效果 scroll-behavior 屬性"
date: 2020-08-05
modified:
description: "只要一行 CSS 實現錨點連結跳轉效果 scroll-behavior 屬性"
categories:
  - "Tech Article"
  - "Front-End"
tags:
  - Css
---

網頁內部的錨點跳轉效果很常見，通常在返回頁首及文章內的跳轉，效果 Y 軸呈現慢慢滾動過去的感覺，這種效果大多人可能使用 JavaScript 達成，但是 CSS 也是可以做到，而且更快更方便，只要加入下面一行 CSS 就可以。

```CSS
body {
    scroll-behavior: smooth;
}
```

只要使用 [scroll-behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior) 屬性就可以達成

[scroll-behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior) 屬性主要有兩個參數，auto 及 smooth，auto 為預設值，其效果會直接滾動到目標，smooth 則可以緩慢的滾動到目標。