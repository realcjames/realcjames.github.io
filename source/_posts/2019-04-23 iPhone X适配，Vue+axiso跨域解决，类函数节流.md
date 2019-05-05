---
title: 2019-04-23 iPhone X适配，Vue+axiso跨域解决，类函数节流
date: 2019-05-05 16:53:23
copyright: true
category:
  -Diary
tag:
  -Diary
---
<!-- more -->
## iPhone X适配
**问题：**公众号中的页面（Vue）有一个留言板功能，输入框是底部absolute的，点击以后，正常会键盘上浮并把整个页面推上去，input框在键盘上方，但iOS尤其是iPhone X，出现了遮挡

**解决过程：**

1.先用了Element.scrollIntoView()无效； 

2.尝试设置各种viewport-fit之类的无效；

3.最后用了聚焦时document.body.scrollTop = document.body.scrollHeight手动滑动到底部解决。

**遗留问题：**

1.Safari下的iPhone X，底部留白很严重，用了上述方法，键盘和input之间会出现很多留白；

2.这种方案感觉还是不够优雅。。。。

## Vue+axiso跨域
**问题：**ajax跨域。。。

**解决过程：**之前的项目里面是在package.json里面配置的跨域，这里是在webpack里面的config/index.js里面配置的proxyTable，这个网上教程还是比较多的。

**遗留问题：**这种和之前的有什么区别，为啥之前的在package.json里面配置？

## 类函数节流
**需求：**页面有个摇一摇功能，之前是每次摇都会请求，现在要改成每500ms一次，发送500ms内的请求总数

**解决过程：**

在网上找了个throttle函数节流的实现，但是这里的结构其实是这样的：

```
window.addEventListener("devicemotion", () => {
// xxx
if(xx) {
// 要在这里加一个节流函数
}
// xxx
}, false);
```

这样其实是不对的。。。节流函数标准结构应该是

```
window.addEventListener("devicemotion", _throttle(func, wait), false);
```

最后只能自己实现了一个类似的，vue里面定义一个data this.shakeTimer

```
this.shakeCount += 1;
if (!this.shakeTimer) {
  this.shakeTimer = setTimeout(() => this.run(this.shakeCount), 500);
}
```

然后在this.run里面先把this.shakeTimer和this.shakeCount清空再去请求（否则在请求返回之前的那段时间摇的次数就不会被计入了）

引申问题：似乎iOS和安卓的摇一摇速度不一样，怎么解决？手动控制多少毫秒只能有一次？

