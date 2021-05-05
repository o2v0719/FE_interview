# HTML 面试内容总结

### 1. Doctype声明的作用？如果去掉Doctype声明会怎样？

+ \<!DOCTYPE>声明位于文档的最前面，处于\<html>标签之前。它的主要作用是告诉浏览器用什么样的文档类型规范来解析文档。

+ 浏览器解析文档有两种方式：”严格模式“和”混杂模式“。

  - 严格模式：浏览器以自身支持的最新标准（W3C）来对页面排版和运行JS。如果文档包含严格的DOCTYPE声明，那么那么它一般以严格模式呈现。

  - 混杂模式：页面以宽松的向后兼容（Backwards Compatibility向过去兼容）的方式显示，模拟老式浏览器的行为以防止站点无法工作。DOCTYPE声明不存在或格式不正确会导致文档以混杂模式呈现。

------

 ### 2. 什么是W3C？W3C标准是什么？
 + W3C 是万维网联盟（World Wide Web Consortium）的缩写。
 + W3C标准主要关注网页的三个部分：结构、表现和行为。分别对应了结构化标准（XHTML，XML）、表现标准语言：CSS和行为标准：（DOM、ECMAScript）等。这些标准大部分由W3C起草发布，也有一些是其他标准组织者指定的标准，如（European Computer Manufactures Association）的ECMAScript标准。
+ 具体的标准有：标签闭合，标签小写，不乱嵌套，使用外链的css和js、结构行为和表现分离等等。

------
### 3. 区分一下XHTML、XML、HTML？
+ XML
  - XML是*Extentsible Markup Language* 可扩展标记语言。没有标签集、没有语法规则，但有句法规则，要求每个标签又要有结束标签，不得含有次序颠倒的标签，而且在语句构成上应符合技术规范的要求。
+ HTML
  - HTML是*Hypertext Markup Language*超文本标记语言。HTML文本是由HTML命令组成的描写叙述性文本，HTML命令能够说明文字、图形、动画、声音、表格、链接等。HTML是网络的通用语言,一种简单、通用的全置标记语言。它同意网页制作人建立文本与图片相结合的复杂页面，这些页面能够被网上不论什么其它人浏览到，不管使用的是什么类型的电脑或浏览器。
+ XHTML
  - XHTML 是*EXtensible HyperText Markup Language*扩展超文本标签语言。XHTML是HTML的升级版。
  - XHTML是HTML像XML的一个过渡语言。它比HTML严谨性会高点。然后基本语言都还是沿用的HTML的标签。仅仅只是废除了部分表现层的标签，同时在标准上要求高了点，比方标签的严格嵌套，标签结束，标签的元素名和属性名大小写敏感等等。XHTML要求全部的标签和属性的名字都必须使用小写。
  - 通过把 HTML 和 XML 各自的好处加以结合，我们得到了在如今和未来都能派上用场的标记语言 - XHTML。XHTML 能够被全部的支持 XML 的设备读取。同一时候在其余的浏览器升级至支持 XML 之前，XHTML 使我们有能力编写出拥有良好结构的文档。

> 更多内容参考: [html、xhtml、xml的定义和区别](https://www.cnblogs.com/iamspecialone/p/11227978.html)

-------

### 4. HTML5 为什么要写\<!DOCTYPE HTML>?

+ HTML5 不基于SGML，不需要对DTD进行引用，但是需要`doctype`来规范浏览器的行为。
+ HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。

> DTD:(Document Type Definition),文档类型定义。声明DTD表示反映出用的是什么类型的标记语言，以便规范自己编写合法的代码，同时让浏览器正确地显示代码。
>
> > 更多内容参考：[什么是DTD？](https://www.cnblogs.com/Ferry/archive/2009/04/09/1432438.html)

-------

### 5.行内元素有哪些？块级元素有哪些？空（void）元素有哪些？行内元素和块级元素有什么区别？

+ 行内元素有： a   b   span  img   input   select	strong
+ 块级元素有：div   ul	ol	li	dl	dt 	dd	h1 h2...	p

+ 空元素有：\<br>  \<hr>  \<img>   \<input>   \<link>   \<meta>等。
+ 行内元素不可以设置宽高，不独占一行；块级元素可以设置宽高，独占一行。

-------
### 6. 什么是置换元素和非置换元素？
+ **置换元素**（replaced element）指用来置换元素内容的部分不由文档的内容直接表示。最常见的置换元素是\<img>，其内容由文档之外的图像文件替换显示。
+ **非置换元素**（nonreplaced element）指元素的内容由浏览器根据文档中元素自身的内容展示。HTML元素大部分都是非置换元素。
------
### 7. 简述一下src和href的区别？
+ 请求src资源时，会将其指向的资源下载并应用到文档中，常用的有script、img、iframe等。
  - `<script src="a.js"></script>`**当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕**，图片和框架等元素也如此，类似于将指向的资源嵌入当前标签内。这也是为什么要把js脚本放在底部而不是头部的原因。
+ 请求href资源时，用来建立当前元素和指向资源文档之间的链接。常用的有：link、a。
  - `<link href="common.css" rel="stylesheet"/>`浏览器会识别该文档为css文件，就会**并行下载资源并且不会停止对当前文档的处理**。这也是为什么建议使用`link`方式来加载css，而不是使用`@import`的方式。

------

### 8. img标签的alt属性和title属性有何异同？

+ alt属性：用来指定替换文字。当图片不能显示时，会显示alt指定的替换文字。

+ title属性： 该属性为设置该属性的元素提供建议性的信息。当鼠标指向图片后，这里的文字会延后显示出来。

------
### 9. strong和em标签的异同？
+ strong: **粗体**强调标签，表示内容重要性。<strong>strong</strong>
+ em：*斜体*强调标签，表示强烈强调。<em>em</em>

-------

### 10. 描述一下渐进增强和 优雅降级之间的区别？

+ **渐进增强**: 一开始现针对低版本浏览器构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能以达到更好的用户体验。

+ **优雅降级**：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

> 渐进增强，首先要保证最基础的功能在低版本的浏览器的实现，然后再向最新的高级浏览器兼容。优雅降级是从复杂的现状开始，并试图减少部分用户体验，以保证向低版本浏览器的兼容。

------
### 11. 描述一下 \<meta name="viewport"> 标签中的一些关键属性

+ viewport 指的是浏览器上用来显示网页的那部分区域。但viewport不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要打，也可能比浏览器的可视区域要小。

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maxmum-scale=1.0,user-scalable=no">
```

```js
// width : 设置viewport的宽度，为一个正整数，或字符串‘device-width’，表示设备屏幕宽度。
// height: 设置viewport的高度，一般设置了宽度，会自动解析出高度，可以不用设置。
// initial-scal: 初始默认缩放比例，为一个数字，可以是小数。
// minimum-scale: 允许用户最小缩放的比例，为一个数字，可以是小数。
// maxmum-scale: 允许用户最大缩放的比例，为一个数字，可以是小数。
// user-scalable: 是否允许用户手动缩放，”no“表示不允许，”yes“表示允许。
```

> 更多内容请参考：[meta标签viewport的深入理解](https://www.cnblogs.com/gaogch/p/10628613.html)

------

### 12. 列举HTML5和CSS3的新特性。

+ HTML5：

  - 增加了video，radio多媒体标签。

  - 增加了canvas元素作为容器，通过js可以绘制图形。

  - 增加了一系列语义化标签header、aside、nav、main、section、article、footer等。

  - 增加了Web Storage本地化存储方案：localStorage（永久存储机制）和sessionStorage(跨会话存储机制)

+ CSS3
  - CSS3 提出了新的盒模型方案：box-sizing:border-box;
  - 提出了border-radius属性，可以用于设置四个边框角的半径属性。
  - 定义了两种类型的渐变（gradient）：
    + 线性渐变：向上、向下、向右、向左、对角方向（linear gradients）
    + 径向渐变：由中心向外（radial-gradient）
  - 提出了字体规则，使用CSS3，网页设计师可以使用他/她喜欢的任何字体。当你发现您要使用的字体文件时，只需简单的将字体文件包含在网站中，它会自动下载给需要的用户。您所选择的字体在新的CSS3版本有关于@font-face规则描述。您"自己的"的字体是在 CSS3 @font-face 规则中定义的。
  - 新增“变形”属性 transform，可以控制元素的二维或三维的平移、缩放或旋转。
  - 新增“动画”属性 ,@keyframes 规则可以定义动画的关键帧。
  - 新增“弹性盒模型”，flex布局。当页面需要适应不同的屏幕大小以及设备类型时，确保元素拥有恰当的行为的布局方式。

