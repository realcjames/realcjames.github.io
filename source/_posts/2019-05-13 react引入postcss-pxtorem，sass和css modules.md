---
title: modules
copyright: true
date: 2019-05-13 15:27:28
categories:
	-Diary
	-React
tags:
	-Diary
	-React
---

# react引入postcss-pxtorem，sass和css modules

**需求: ** taro应该暂时不支持styled-components，于是决定在这个预热的项目里面，用sass+css modules，并且因为是移动端，响应式是必须的，这里选择pxtorem。

 <!--more-->

## sass和css modules

**介绍：**网上太多了，这里就不贴了

**配置：**其实最新的create-react-app脚手架应该是自带了这两个功能，只不过有一点需要注意，在yarn eject展开webpack的配置后，看到在config/webpack.config.js文件里面，有这样一段：

```javascript
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
```

应该可以理解为，.module.(css | scss | sass)后缀的，才能启用css modules功能。

**使用：**

```javascript
import styles from 'css/PreorderMeasurement.module.scss';

render() {
    return <div className={styles.test} />
}
```

**自定义class名：**

默认的生成class名规则是这样的：

>Creates a class name for CSS Modules that uses either the filename or folder name if named `index.module.css`.
>For `MyFolder/MyComponent.module.css` and class `MyClass` the output will be `MyComponent.module_MyClass__[hash]`. For `MyFolder/index.module.css` and class `MyClass` the output will be `MyFolder_MyClass__[hash]`
编译完以后是这样的：

```html
<div class="PreorderMeasurement_test11__3hBMi"></div>
```

修改这里的配置：

```javascript
  {
     test: sassModuleRegex,
     use: getStyleLoaders(
           {
              importLoaders: 2,
              sourceMap: isEnvProduction && shouldUseSourceMap,
              modules: true,
              // getLocalIdent: getCSSModuleLocalIdent, 这里是默认配置
              localIdentName: '[path]__[name]__[local]___[hash:base64:5]',
            },
            'sass-loader',
 	  ),
},
```

再编译以后就变成这样了：

```html
<div class="src-css-__PreorderMeasurement-module__test11___3oqbM"></div>
```

# postcss-pxtorem

**安装配置：**

> 1. yarn add ...
>
> 2. yarn eject 打开webpack的相关配置
>
> 3. 在config/webpack.config.js里找到

```javascript
// Adds PostCSS Normalize as the reset css with default options,
// so that it honors browserslist config in package.json
// which in turn let's users customize the target behavior as per their needs.
postcssNormalize(),
```

> 这一行，在后面加上：

```javascript
require('postcss-pxtorem')({
   rootValue: 100,
   unitPrecision: 5,
   propWhiteList: ['*'],
   minPixelValue: 0,
}),
```

> 4. index.html里面，head标签里加上：

```html
<script>
        (function (doc, win) {
            var docEl = doc.documentElement,
            resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
            recalc = function () {
                var clientWidth = docEl.clientWidth;
                if (!clientWidth) return;
                docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';
            };
            if (!doc.addEventListener) return;
            win.addEventListener(resizeEvt, recalc, false);
            doc.addEventListener('DOMContentLoaded', recalc, false);
        })(document, window);
</script>
```

里面的rootValue，100，750等都是可以配置的，具体看设计稿和需求，一般设计稿都是按750出的，然后100的话，方便计算rem，毕竟也可能会遇到要写行内的情况

> 注：有没有方法让行内写的px也转成rem?

