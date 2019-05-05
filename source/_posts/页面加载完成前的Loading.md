---
title: 页面加载完成前的Loading
date: 2019-05-05 17:32:44
tags: ["web"]
---
```html
<div class="loading"></div>
```

```js
document.addEventListener('readystatechange', function () {
  if (document.readyState == 'complete') {
    document.querySelector('.loading').style.display = 'none'
  }
})