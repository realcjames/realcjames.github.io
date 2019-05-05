---
title: 2019-04-24 监听页面回退，清除eventListener
copyright: true
date: 2019-05-05 16:56:39
categories:
   -Diary
tags:
   -Diary
---
<!-- more -->
## 监听页面回退
**问题：**小程序跳转到公众号的某个页面链接，中间经过授权页面，跳过去以后再回退会回到授权页。

**解决方案：**
```
created() {
  if (window.history && window.history.pushState) {
    window.addEventListener("popstate", this.goBack, { once: true });
    // 这个once表示 listener 在添加之后最多只调用一次。如果是 true， listener 会在其被调用之后自动移除
    // 本来是计划在destroyed里面remove这个listener，但是似乎还没生效就destroy了，所以只能这样了
  }
}

goBack() {
  this.$router.go(-1);
}
```
**遗留问题：**能不能还是在destroyed里面remove这个listener?

## 清除eventListener
```
destroyed() {
  window.removeEventListener("devicemotion", this.shakeListner, false);
}
```