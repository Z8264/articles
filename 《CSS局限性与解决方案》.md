# CSS 局限性与解决方案

CSS Cascading Style Sheets 层叠样式表

使用CSS来进行前端的样式开发，但随着项目的逐渐庞大，CSS技术局限性也越发明显，主要的问题有两个：

* 作用域问题。
* 复杂度问题。

例如，开发时命名太大众化会造成冲突，过渡复杂又难以维护。随着技术的不断发展，行业解决方案也呈现百花齐放的开源生态，接下来我们介绍下不同的解决方案。

* 命名规范
* 预处理
* 后处理
* 模块化
* CSS IN JS
* Shadow Dom

### 命名规范解决方案

技术发展早期，由于前端工程化等技术没有得到广泛的普及，所以出现很多以命名规范为主要技术手段来解决问题的方案。主要代表规范体系有：BEM、OOCSS、AMCSS、SMACSS、SUITCSS、ITCSS。

* BEM 【Blocks，Elements，Modifiers】

BEM 设定了块、元素、修饰符三个概念。书写规则上：

```html
<div class="father">
   <div class="father-child"></div>
</div>
```

用单横线 `-` 来表示块与块之间的父子关系。例如，div `father` 与子块 div `child`, 可分别命名为 `.father`,`.father-child`

```html
<div class="father">
   <div class="father-child">
   		<input type="text" class="father-child__input"/>
   </div>
</div>
```

用双下划线`__`来表示块与元素之间的关系。例如，div `child`子块中有一个元素 Input，可以命名为 `.father-chidle__input`,  

```html
<div class="father">
   <div class="father-child father-child--green">
   		<input type="text" class="father-child__input"/>
   </div>
   
   <div class="father-child father-child--red">
   		<input type="text" class="father-child__input"/>
   </div>
</div>
```

用双横线`--`来表示具体块和元素的状态。例如，div `father`存在两种不同的背景，何以分别增加修饰类，命名为`.father-child--green` 和 `father-child--red`

* OOCSS **Object Oriented CSS 面向对象的CSS**，主要是通过命名规范来实现“结构和样式分开” 和 “容器和内容分开” 

* AMCSS **Attribute Modules for CSS，即 CSS的属性模块**，简单来说就是通过css属性选择器来模块化CSS，而不是使用class来描述。
* SMACSS **Scalable and Modular Architecture for CSS 可扩展和模块化的css架构**，讲CSS命名划分为了Base、Layout、Module、State、Theme 五个种类。
* SUITCSS **SUIT CSS是一种专注于为基于组件的开发改善CSS创作体验的方法**，在工具、组件，变量进行了命名约定。
* ITCSS，**nverted Triangle CSS 倒三角CSS**，ITCSS的主要原则之一是将CSS代码库分为几个部分（称为layer），这些部分采用倒三角形的形式。



### 预处理 Sass / Less 

既然编写原生的 CSS 并不友好，是否可以创建一种新的语言呢？

其基本思想是，用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，这就是CSS预处理器。代表技术 SASS、LESS、Stylus。

以SASS为例，预处理提供的典型能力有：

* CSS 变量声明 与 变量引用
* 嵌套CSS
* 父选择器标志符



【增加视频演示】



### 后处理 PostCSS

css后处理器是对 css 进行处理，并最终生成 css 预处理器，它属于广义上的 css 预处理器。代表技术PostCSS。

以 PostCSS 为例，对已经开发完成的 css 进行处理：

* 自动获取浏览器的流行度和能够支持的属性，并根据这些数据帮你自动为 CSS 规则添加前缀。
* 帮你将最新的 CSS 语法转换成大多数浏览器都能理解的语法，并根据你的目标浏览器或运行时环境来确定你需要的 polyfills。
* 支持和处理CSS 模块化。
* 通过使用 stylelint 强化一致性约束并避免样式表中的错误。

PostCSS 为 CSS 模块化的发展提供了很好的技术基础。



### CSS 模块化

随着 react、vue 等框架的普及使用，我们常常将页面拆分成许多个组件模块。因此，CSS 模块化相关的技术得到了迅速发展。主流技术方案，CSS Modules，Vue scoped css。

**CSS Modules** ，具体示例：

定义CSS：

```
/* style.css */
.className {
  color: green;
}
```

引入CSS：

```
import styles from "./style.css";
// import { className } from "./style.css";

element.innerHTML = '<div class="' + styles.className + '">';
```

最终编译结果:

```
啊
```



**Vue scoped css** 

Vue 组件作用域 scoped css，这是 Vue 提供的解决方案，具体示例：

```
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

编译结果

```
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```





### CSS In JS

CSS in JS，意思就是使用 JS 语言写 CSS，脱离单独的 CSS 文件，以实现 css 的模块化。CSS in JS 其实是一种编写思想，目前已经有超过 上百种开源方案的实现，代表技术 styled-components。

具体示例：

```
import React from "react";
import styled from "styled-components";

// 创建一个带样式的 h1 标签
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// 创建一个带样式的 section 标签
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// 通过属性动态定义样式
const Button = styled.button`
  background: ${props => (props.primary ? "palevioletred" : "white")};
  color: ${props => (props.primary ? "white" : "palevioletred")};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// 样式复用
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

<Wrapper>
  <Title>Hello World, this is my first styled component!</Title>
  <Button primary>Primary</Button>
</Wrapper>;
```



### Shadow Dom

一种浏览器技术原生支持的技术解决方案。

Web components 的一个重要属性是封装——可以将标记结构、样式和行为隐藏起来，并与页面上的其他代码相隔离，保证不同的部分不会混在一起，可使代码更加干净、整洁。其中，Shadow DOM 接口是关键所在，它可以将一个隐藏的、独立的 DOM 附加到一个元素上。

![clipboard.png](https://segmentfault.com/img/bVbny53?w=932&h=458)

Firefox（从版本 63 开始），Chrome，Opera 和 Safari 默认支持 Shadow DOM。基于 Chromium 的新 Edge 也支持 Shadow DOM；而旧 Edge 未能撑到支持此特性。

