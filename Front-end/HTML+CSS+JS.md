# 开始

为了成为全栈工程师开始走出第一步！

首先来以搭建一个好看而且功能性强的个人网站为目标吧！

前有好奇！（向前指）

## 主要参考资料&学习资料

学习路线：https://objtube.github.io/front-end-roadmap/

主要学习教程：https://web.qianguyihao.com/

副文档参考：https://www.w3school.com.cn/html/index.asp

# 01 - 整体认识

如果想要搭建一个网站，我们需要的编写的是**网页**，由用户的**浏览器**通过**网络（web）**来访问。

## Web标准

由于浏览器的多种多样，我们需要以一个统一的标准来编写我们的网页来使得我们的网页能够在不同的浏览器正常访问（或者说以我们想要的方式展现出来），而这个标准就是**Web标准**。

Web标准包含三个方面：

### 结构标准（HTML）

用于对网页元素进行整理和分类。HTML 全名是超文本标记语言（HyperText Markup Language）,述页面的**结构**。如果把网页比作一个人的话，HTML相当是网页的肉体。

### 表现标准（CSS）

用于设置网页元素的版式、颜色、大小等外观样式。CSS全称是层叠样式表（Cascading Style Sheets）, **美化页面的样式**。还是以人为比方，CSS相当于是网页的衣服。

### 行为标准（JS）

用于定义网页的交互和行为。JS全称是JavaScript，是一种即时编译型的脚本语言，为页面**提供交互和行为**。以人为比方的话，相当于是网页的行为动作。

## 浏览器

分成两部分：**渲染引擎** 与 **JS 引擎**

### 渲染引擎

又称浏览器内核，负责将 HTML 和 CSS 文件解析成我们看到的页面并且显示出出来（有点像是一种编译器），是造成页面显示不同的主要原因。

目前主要的渲染引擎有这些：Blink、Webkit、Trident、Gecko

##### Webkit

苹果公司自己的内核，是开源内核，但是除了苹果自带的 Safari 浏览器外，Android 自带的浏览器也大部分以此为内核

##### Blink

谷歌基于 Webkit 自研发的一个分支，主要是在 Chrome 、Opera 上使用

##### Gecko

主要是在火狐浏览器上使用的内核，是开源内核，可拓展性极高

##### Trident

时代的眼泪：IE 浏览器的内核，也经常被称作 IE 内核


### JS 引擎

 用来解析网页中的JavaScript代码。浏览器本身并不会执行JS代码，而是通过 JS 引擎执行，代码时会逐行解释每一句源码并运行（可以类比 python）。

# 02 - HTML

## HTML 是什么？

HTML全称为 Hyper Text Markup Language ，译为**超文本标记语言**。

从两个角度去理解这个名字，首先是**超文本**：超文本其实很好理解，一是指多媒体等不局限于文本限制的**信息传递方式**（比如说图片啊视频啊这些）；除此之外，还可以指跨文件访问的**超级链接文本**（比如说从一个网站访问另一个网站这样）

其次是**标记语言**。标记语言并不是编程语言，它的作用只是通过标记来**描述**一种结构或者相互关系。标记语言是通过引擎解析，而编程语言是通过编译器编译。

总而言之，HTML 是负责**描述文本语义**的语言。在这里语义是指文本的属性，比如说标题啊等等等等，这些语义是由 HTML 的标签来描述的，之后会讲的各种标签的作用就是给文本增加某一个语义。

## HTML 的专有名词解释

- 网页：由各种标记组成的一个页面

- 标签：也叫标记，描述一个**元素的起止和属性**的记号，通常是成对出现的，叫做双标记，分为开始标签（开放标签）和结束标签（闭合标签） (e.g. `<p> </p>  `)

- 元素：由**标记和标记所包含的内容**的一个整体，元素名就是标记名  (e.g. `<p> hello world </p>  `就是一个 `p` 元素)

- 属性：**描述一个标记**的更详细的信息，通常包含在标记的尖括号中，对应的属性值以`" "`在属性名后包起来给出。 (e.g.`<p id="title">`中`id`就是标签`<p>`的一个属性，对应的值是`"title"`)

## HTML 结构

*一睹芳容*

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hello world</title>
</head>
<body>
    <h3>hello!</h3>
    <a href="https://www.ragativ.top">自 产 自 销</a>
</body>
</html>

```

### 文档声明头 DTD

这里面的`<!DOCTYPE html>`是文档声明头（ DocType Declaration，简称 DTD ）。任何一个标准的HTML页面，第一行必须是 `<!DOCTYPE _____ >`。DTD可告知浏览器文档使用哪种 HTML 或 XHTML 规范，这部分在之后再说，上述的代码中是对应的 HTML5 的 DTD。

需要注意的是，**DTD 不属于 HTML 标签**。

### 其他注意点

- 标签和属性的名字**不区分大小写**，但是建议统一使用小写
- 如果存在连续**多个空白字符**(空格、制表符、回车、换行等)，浏览器显示时将**只解析为一个空格字符**
- HTML**依靠标签的嵌套关系来表示元素之间的嵌套**的，需要与Python中缩进的功能区分开来。但是良好的缩进能够有效的增强代码可读性

## HTML 元素

### 元素的构成

```html
<标签 属性1="属性值" 属性2="属性值"> 内容 </标签>
```

### 标签的类型

#### 结构标签

##### 文档标签`<html>`

```html
<html lang="en">
    ...
</html>
```
页面中最大的标签，也称作**根标签**，网页中所有的内容都**必须包含**在文档标记之间。可以通过设置`lang`属性来指定界面的语言类型，通常使用`en`和`zh-CN`作为值来分别指定界面语言为英文和中文。

##### 头标签`<head>`

```html
<head>
	...
</head>
```

head 标签包含了所有的头部标签元素。这些元素描述了这个html文档的各种属性和信息，其中包括文档的标题、在 Web 中的位置以及和其他文档的关系等。

###### title 标签

```html
<title>这里是标题捏</title>
```

指定整个网页的标题，在浏览器最上方显示。是**必须在头标记中出现**的一个子标签。而且只能出现一个。

###### base 标签

```html
<base href="http://www.ragativ.top/">
```

指定这个标签之后的所有 `src` 和 `herf`  在使用相对地址的时候以`<base>`中的`herf`属性的属性值为基准。比如说在经过上述的标签描述后，在后文的某一个地方有这样一个元素 ：`<a herf="about.html">关于<\a>`，实际上就是指向`http:/www.ragativ.top/about.html`的超链接；又或者有这样一个元素 ：`<img src="/images/logo.png">`，实际上就是指向了`http:/www.ragativ.top/images/logo.png`的图片进行展示。

###### meta 标签 

meta表示“元”。“元”配置，就是表示基本的配置项目。元数据不会在网页中显示，但会被浏览器、搜索引擎等程序解析和应用。它是通过对特定的属性赋值来实现对文档的配置。通常一个meta标签对应一个配置。下面说说常用的配置：

- 字符集 charset

```html
<meta charset="UTF-8"> 
```

设置网站所使用的字符集，**必须存在**。如果出现乱码，记得看看是不是字符集设置错了。一般为了保证字库覆盖所有使用到的字，常常设置为`UTF-8`；而如果追求小巧和快速并且只需要中文和少数外语和符号，`GBK`肯定是更优的选择。

- 文档关键字 name="keyword"

```html
<meta name="keywords" content="HTML,CSS,JS">
```

文档关键字适是用于搜索引擎的，会在搜索对应关键字的时候让网站被检索到。多个关键字需要在双引号内使用逗号分隔。

- 页面描述 name="description"

```html
<meta name="description" content="啊什么都没有的网页呢">
```

文档关键字适是用于搜索引擎的，会在搜索到网站的时候展示文档关键字来介绍网站

- 自动刷新 http-equiv="refresh"

```html
<meta http-equiv="refresh" content="30;URL">
```

这个功能是在30秒后跳转到 URL 链接，URL部分可以不写，对应的效果就是刷新网页。



##### 正文标签`<body>`

```html
<body>
	...
<\body>
```

body 标签包含了文档的所有的显示内容，页面上的内容基本上都是嵌套在正文标签里的。正文标签也有特殊的属性可以设置。

- `bgcolor`：设置整个网页的背景颜色。
- `background`：设置整个网页的背景图片。
- `text`：设置网页中的文本颜色。
- `leftmargin`：网页的左边距，以像素为单位(e.g. 8px)。
- `topmargin`：网页的上边距。
- `rightmargin`：网页的右边距。
- `bottommargin`：网页的下边距。
- `link`：超链接没有点击的时候显示的颜色
- `alink`：超链接点击了但是还没有松开的时候显示的颜色
- `vlink`：超链接点击之后的时候显示的颜色



#### 排版标签

##### 注释标签 `<!--  -->`

```html
<!-- 注释 -->
```

##### 标题标签`<h1>`-`<h6>`

```html
<h1>ZWEI!</h1>
```

h1 - h6 设置一到六级标题，标题的大小随数字的增大而减小
