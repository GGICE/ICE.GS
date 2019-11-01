---
title: CSS 知识清单
tags:
  - web development
permalink: css-zhi-shi-qing-dan
id: 165
updated: '2016-01-23 23:46:55'
date: 2016-01-23 21:54:43
---

记录一些用时还需要去检索用法的一些css属性。<small>部分内容可能是从他处粘贴过来，若有侵犯请告知！</small>

注意：非标准属性会用 <code>*</code> 标注
## 1 word-wrap word-break white-space

### 描述：
word-wrap、word-break为css3属性，分别定义允许长单词或 URL 地址换行到下一行；定义自动换行的处理方法；white-space 定义如何处理元素内的空白。

值得说明的是，三者的定义会出现覆盖影响。

### 取值：

word-wrap 默认值 normal，会继承
<table class="dataintable">
<tbody><tr>
<th style="text-align: left">值</th>
<th style="text-align: left">描述</th>
</tr>

<tr>
<td>normal</td>
<td>只在允许的断字点换行（浏览器保持默认处理）。</td>
</tr>

<tr>
<td>break-word</td>
<td>在长单词或 URL 地址内部进行换行。</td>
</tr>
</tbody>
</table>

word-break 默认值normal，会继承
<table class="dataintable">
<tbody><tr>
<th style="text-align: left">值</th>
<th style="text-align: left">描述</th>
</tr>

<tr>
<td>normal</td>
<td>使用浏览器默认的换行规则。</td>
</tr>

<tr>
<td>break-all</td>
<td>允许在单词内换行。</td>
</tr>

<tr>
<td>keep-all</td>
<td>只能在半角空格或连字符处换行。</td>
</tr>
</tbody>
</table>

white-space 默认值 normal, 会继承
<table class="dataintable">
<tbody><tr>
<th style="text-align: left">值</th>
<th style="text-align: left">描述</th>
</tr>

<tr>
<td>normal</td>
<td>默认。空白会被浏览器忽略。</td>
</tr>

<tr>
<td>pre</td>
<td>空白会被浏览器保留。其行为方式类似 HTML 中的 &lt;pre&gt; 标签。</td>
</tr>

<tr>
<td>nowrap</td>
<td>文本不会换行，文本会在在同一行上继续，直到遇到 &lt;br&gt; 标签为止。</td>
</tr>

<tr>
<td>pre-wrap</td>
<td>保留空白符序列，但是正常地进行换行。</td>
</tr>

<tr>
<td>pre-line</td>
<td>合并空白符序列，但是保留换行符。</td>
</tr>

<tr>
<td>inherit</td>
<td>规定应该从父元素继承 white-space 属性的值。</td>
</tr>
</tbody>
</table>

### 测试：

<iframe width="100%" height="300" src="//jsfiddle.net/kxqgh11d/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## *2 text-rendering

### 描述：

Text-rendering 属性是一个非标准属性，本意是用来设置SVG的属性，之后Gecko/WebKit/Blink等内核也支持了普通DOM元素设置Text-rendering。
主要用来告诉渲染引擎（rendering engine）渲染文字的时候如何来优化，浏览器根据这个属性来权衡速度、易读性、几何精度等方面。不同系统、不同浏览器的渲染引擎不同，效果不同。

### 取值：

<table>
  <thead>
    <tr>
      <th style="text-align: left">值</th>
      <th style="text-align: left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>auto</code></td>
      <td>浏览器用渲染速度、易读性、几何精度等自动优化来绘制文本，不同浏览器效果不同</td>

    </tr>
    <tr>
      <td><code>optimizeSpeed</code></td>
      <td>绘制文本时渲染速度优先，会禁用使用特殊的字距调整和某些字体的连字</td>
    </tr>
    <tr>
       <td><code>optimizeLegibility</code></td>
       <td>绘制文本时易读性优先，允许使用特殊的字距调整和某些字体的连字</td>
    </tr>
    <tr>
       <td><code>optimizeLegibility</code></td>
       <td>绘制文本时几何精度优先，可以使某些字体非间距非几何对称，达到更好的显示效果</td>
    </tr>
  </tbody>
</table>

### 其他：

在大多情况话看不出明显区别，对一些特殊英文字体可能有效果！
参考：[http://www.feeldesignstudio.com/2013/05/text-rendering/](http://www.feeldesignstudio.com/2013/05/text-rendering/)
[http://www.feeldesignstudio.com/2013/05/text-rendering/](http://www.feeldesignstudio.com/2013/05/text-rendering/)
