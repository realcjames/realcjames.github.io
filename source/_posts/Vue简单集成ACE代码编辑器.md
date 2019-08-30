---
title: Vue简单集成ACE代码编辑器
copyright: true
date: 2019-08-30 10:41:19
categories:
	--Vue
tags:
	--Vue
top:
---

# Vue简单集成ACE代码编辑器

需求：想在前端显示（或编辑）代码呗。。

> 其实项目原有一个code mirror，不过我做完才发现。。
>
> 算了下次再改吧。。

 <!--more-->



## 踩坑阶段

1. 先去github看看官方文档，照着[embedding-ace](https://ace.c9.io/#nav=embedding)试着集成了下，发现报错，百度也没找到原因，试了其他类似的方法，继续报错。。遂放弃，此处浪费1小时+；

   > 内心吐槽。。ace这个文档写的真xx烂。。我们的习惯就是npm install 然后 import就好了呀

2. 万念俱灰之际，打开了[这篇帖子](https://blog.csdn.net/YoshinoNanjo/article/details/82978668)，实属缘分，大概看看，npm install+import，熟悉的感觉，就参照他先试试了。

   > 本文基本参照此贴编写，实际上就是写给自己看的简单教程，侵删。。



## 开始集成

1. 安装依赖包

```
npm install ace-builds --save-dev
```

2. main.js中引入

``` 
import ace from 'ace-builds'
Vue.use(ace)
```

3. 创建一个vue组件去包裹编辑器

```javascript
<template>
  <div class="ace-container">
    <!-- 官方文档中使用 id，这里禁止使用，在后期打包后容易出现问题，使用 ref 或者 DOM 就行 -->
    <div class="ace-editor" ref="ace"></div>
  </div>
</template>
<script>
import ace from 'ace-builds'
import 'ace-builds/webpack-resolver' // 在 webpack 环境中使用必须要导入
import 'ace-builds/src-noconflict/theme-monokai' // 默认设置的主题
import 'ace-builds/src-noconflict/mode-javascript' // 默认设置的语言模式

export default {
  name: 'CodeFormat',
  props: {
    value: {
      type: String,
      required: true
    }
  },
  mounted() {
    this.aceEditor = ace.edit(this.$refs.ace, {
      maxLines: 20, // 最大行数，超过会自动出现滚动条
      minLines: 10, // 最小行数，还未到最大行数时，编辑器会自动伸缩大小
      fontSize: 12, // 编辑器内字体大小
      theme: this.themePath, // 默认设置的主题
      mode: this.modePath, // 默认设置的语言模式
      tabSize: 4, // 制表符设置为 4 个空格大小
      readOnly: true,
      highlightActiveLine: false,
      value: this.codeValue
    })
  },
  data() {
    return {
      aceEditor: null,
      themePath: 'ace/theme/github', // 不导入 webpack-resolver，该模块路径会报错
      modePath: 'ace/mode/html', // 同上
      codeValue: this.value
    }
  },
  watch: {
    value(newVal) {
      this.aceEditor.setValue(newVal)
    }
  }
}
</script>
```

4. 需要的地方直接引入

```javascript
import CodeFormat from '@/components/CodeFormat'
```

 	然后

```vue
<code-format :value="jeditor.value"></code-format>
```

​	别忘了这一步

```javascript
export default {
  name: 'RichText',
  components: {
    CodeFormat
  },
}
```

> 不知道为什么vue要多这一步。。对我这个react吹来说实在有点不习惯。。。



## 使用中的问题

1. 没有实时更新的问题

   加一个父传值的监听，同时要去调用setValue这个方法，毕竟这个ace也不是用vue做的：

   ```javascript
    watch: {
       value(newVal) {
         this.aceEditor.setValue(newVal)
       }
     }
   ```



## 吐槽

这个ace。。文档实在是。。全英文不说（虽然咱六级557，但我是个热爱中文的码农），还很难找，安装集成浪费了好久，找api浪费了好久，最后在[这里](https://github.com/ajaxorg/ace/wiki/Configuring-Ace)找到了配置，然而看完了我竟然不知道value在哪配，我此时真的是想yarn remove了，但是吧，抬头看看右上角，

{% asset_img 20190830141313.png star image %}

20K的star数，比咱月薪还高。。算了算了。。我也勉强点个star继续用吧~

> 还是感谢之前csdn那个大佬。。。不然我真要用code mirror了