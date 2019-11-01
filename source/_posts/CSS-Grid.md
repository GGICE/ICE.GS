---
title: CSS Grid
tags:
  - web development
permalink: css-grid
id: 183
updated: '2017-03-10 09:12:29'
date: 2017-03-08 14:15:37
---

CSS Grid 一种二维布局系统, 最初草案于2011年4月发布，最初在IE10上实现，通过`-ms-`使用。经过6年的探索和发展，17年有了较为稳定的候选版本，Firfox在52版本中实现了对[CSS Grid Layout Module Level 1](https://www.w3.org/TR/css-grid-1/)的支持，Chrome在57版本实现了支持，Safari在10.1版本实现支持。

![](/content/images/2017/03/Screen-Shot-2017-03-09-at-12-06-32-PM.png)

相比Flex Grid是为了解决二维布局问题，Flex则解决一维布局问题；在Grid诞生之前没有很恰当解决二维布局的方法，很久之前采用的是基于表格的布局方式，后来一般是采用float布局结合Flex等实现布局，但是这些都并非原属性设计的使用初衷，Grid才是一种更好的布局方案，随着草案的完善和浏览器的支持，之后肯定会得到广泛应用。

![](/content/images/2017/03/Screen-Shot-2017-03-09-at-11-16-48-AM.png)

# 概念和术语
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-5-59-21-PM.png)
## Grid Lines
指形成网格的水平和垂直线
## Grid Tracks and Cells
Grid Tracks 指相邻网格间的区域
Grid Cells 指2个相邻行网格线和2个相邻列网格线之间的区域
Grid Areas 由一或多个Cell组成的区域

<iframe height='265' scrolling='no' title='CSS-GRID' src='//codepen.io/GGICE/embed/preview/ryjYLw/?height=265&theme-id=dark&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/GGICE/pen/ryjYLw/'>CSS-GRID</a> by GGICE (<a href='http://codepen.io/GGICE'>@GGICE</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

# Grid Container 属性
* display
* grid-template-columns
* grid-template-rows
* grid-template-areas
* grid-template
* grid-column-gap
* grid-row-gap
* grid-gap
* grid-auto-columns
* grid-auto-rows
* grid-auto-flow
* justify-items
* grid
* justify-content
* align-content
* justify-items
* align-items

## `grid-template-columns 和 grid-template-rows`
定义行列轨迹

    grid-template-columns: none | <track-list> | <auto-track-list>
    grid-template-rows: none | <track-list> | <auto-track-list>

### `<track-list>`

    [ <line-names>? [ <track-size> | <track-repeat> ] ]+ <line-names>?

#### `<track-size>`
`<track-breadth>`

* css 长度如px、%
* flexible 长度如 1fr (比例分配剩余的空间) [🌰](https://igalia.github.io/css-grid-layout/breadth.html)
* ` min-content` 字符，不超出边界的最小尺寸
* `max-content` 字符，装配内容的所需的最小尺寸 [🌰](https://igalia.github.io/css-grid-layout/max-min-content.html)
*  `auto` 字符，占据所有剩余的空间

```
.grid {
  display: grid;
  grid-template-columns: min-content max-content auto;
}
```
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-8-06-53-PM.png)

`计算函数:`

`minmax(<inflexible-breadth> , <track-breadth>)`

可以是由minmax（）函数定义的范围，其中第一个值为最小值，第二个值为最大值。对于这种情况，最小值不能是flexible长度，因此您可以使用除flexible单位外的所有类型的值作为<track-breadth>。

注意：在后期版本中可能支持第第一个最小值为flexible单位

`fit-content(<length-percentage>)`

表示公式min（max-content，max（auto，argument）），其计算类似于auto（即minmax（auto，max-content））。

`auto`
作为最大值时，等于`max-content`。作为最小值时，相当于`min-content`。
注意：自动轨道大小（并且只有自动轨道大小）可以通过align-content和justify-content属性进行扩展。


#### `<track-repeat>`
利用一个repeat函数帮助我们实现一个很多行很多列的网格。
```
repeat( [ <positive-integer> ] , [ <line-names>? <track-size> ]+ <line-names>? )
```
```
grid-template-columns: 30px 100px 30px 100px 30px 100px 30px 100px;

/* same as above but with the repeat() function */
grid-template-columns: repeat(4, 30px 100px);
```

`<line-names>`

给网格线添加一个名字，可以是除了`span` 以外的任意字符串，网格线可以有多个名字。

```
grid-template-columns: [first sidebar-start] 250px [content-start] 1fr [last];
grid-template-rows: [first header-start] 100px [content-start] 1fr [footer-start] 100px [last];
```
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-8-34-44-PM.png)
[🌰](https://igalia.github.io/css-grid-layout/named-grid-lines.html)

##`<auto-track-list>`

```
[ <line-names>? [ <fixed-size> | <fixed-repeat> ] ]* <line-names>? <auto-repeat>

[ <line-names>? [ <fixed-size> | <fixed-repeat> ] ]* <line-names>?

```
`<fixed-size>`

1. `<fixed-breadth>`
2. `minmax( <fixed-breadth> , <track-breadth> )`
3. `minmax( <inflexible-breadth> , <fixed-breadth> )`

`<fixed-repeat>`
```
repeat( [ <positive-integer> ] , [ <line-names>? <fixed-size> ]+ <line-names>? )
```
`<auto-repeat>`

```
repeat( [ auto-fill | auto-fit ] , [ <line-names>? <fixed-size> ]+ <line-names>? )
```
`auto-fill` 生成尽可能多的列，以适应可用空间，而不会导致网格溢出。
`auto-fit` 同上，区别在于将折叠任何空的重复轨道（这意味着它们的大小为0px）。

## `grid-template-area`

    grid-template-areas: none | <string>+

此属性指定命名网格区域，还提供了网格结构的可视化，使得网格容器的总体布局更容易理解。

#### `<string>+`

每个单独的字符串创建一个行，而字符串中的每个单词创建一个列。所有字符串必须具有相同数量的单词，否则声明无效。使用一个或多个'.'的序列（U + 002E FULL STOP）表示空单元，其是网格中的未命名区域。

```
.grid-container {
  display: grid;
  grid-template-areas: "logo stats"
                       "score stats"
                       "board board"
                       "... controls";
}

.logo { grid-area: logo; }
.score { grid-area: score; }
.stats { grid-area: stats; }
.board { grid-area: board; }
.controls { grid-area: controls; }
```
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-9-04-58-PM.png)

## `grid-template`

```
grid-template: none | [ <‘grid-template-rows’> / <‘grid-template-columns’> ] | [ <line-names>? <string> <track-size>? <line-names>? ]+ [ / <explicit-track-list> ]?
```
把 <‘grid-template-columns’>, <‘grid-template-rows’> and <‘grid-template-areas’>放置在一个属性中描述

```
grid-template: [header-top] "a a a" [header-bottom] [main-top] "b b b" 1fr [main-bottom] / auto 1fr auto;
```

等于

```
grid-template-areas: "a a a"
                     "b b b";
grid-template-rows: [header-top] auto [header-bottom main-top] 1fr [main-bottom];
grid-template-columns: auto 1fr auto;
```
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-9-11-13-PM.png)

## `grid-column-gap` 和 `grid-row-gap`

```
grid-column-gap: <length> | <percentage>
grid-row-gap: <length> | <percentage>
```

## `grid-gap`

```
grid-gap: <‘grid-row-gap’> <‘grid-column-gap’>?
```
若只声明grid-row-gap则grid-column-gap也为该值

## ` grid-auto-columns` 和 `grid-auto-rows`

```
grid-auto-columns: <track-size>+
grid-auto-rows: <track-size>+
```
当grid item的列或行未由<'grid-template-columns'>或<'grid-template-rows'>定义时，将创建隐含网格轨道以保存这些项。我们可以使用<'grid-auto-columns'>和<'grid-auto-rows'>属性来控制这些隐式网格轨道的大小。我们还可以为这些隐式网格轨道指定多个轨道大小。

```
.grid {
  display: grid;
  grid-template-columns: 150px 150px;
  grid-auto-columns: 50px 100px;
}

.item {
  grid-column: 8;
}
```

![](/content/images/2017/03/Screen-Shot-2017-03-09-at-9-24-57-PM.png)

## `grid-auto-flow`

```
grid-auto-flow: [ row | column ] | dense
```
未明确定义放置在网格上的网格项，自动放置算法会自动放入这些项。此属性控制自动布置算法的工作原理。

`row` 默认值，自动布置算法将通过填充每一行并根据需要添加新行来放置网格项。

`column` 自动布置算法将通过填充每个列放置网格项，并根据需要添加新列

`dense` 默认为稀松排列，该值设置为稠密排列，若设置则算法将尝试适合在网格中较早的填充"孔"。这将最小化网格中“孔”的发生率。[🌰](https://igalia.github.io/css-grid-layout/autoplacement.html)

## `grid`

```
grid: <‘grid-template’> | <‘grid-template-rows’> / [ auto-flow && dense? ] <‘grid-auto-columns’>? | [ auto-flow && dense? ] <‘grid-auto-rows’>? / <‘grid-template-columns’>
```
```
<‘grid-template-rows’> / [ auto-flow && dense? ] <‘grid-auto-columns’>?

.grid {
  grid: 50px 75px / auto-flow;
}

/* is equivalent to */
.grid {
  grid-template-rows: 50px 75px;
  grid-template-columns: none; /* cannot be set explicitly with this syntax form */
  grid-template-areas: none; /* cannot be set explicitly with this syntax form */
  grid-auto-rows: auto; /* cannot be set explicitly with this syntax form */
  grid-auto-columns: auto;
  grid-auto-flow: column; /* can only set dense or not */
  grid-column-gap: 0; /* cannot be set explicitly with this syntax form */
  grid-row-gap: 0; /* cannot be set explicitly with this syntax form */
}
```

```
[ auto-flow && dense? ] <‘grid-auto-rows’>? / <‘grid-template-columns’>

.grid {
  grid: auto-flow dense / 30% 100px;
}

/* is equivalent to */
.grid {
  grid-template-rows: none; /* cannot be set explicitly with this syntax form */
  grid-template-columns: 30% 100px;
  grid-template-areas: none; /* cannot be set explicitly with this syntax form */
  grid-auto-rows: auto;
  grid-auto-columns: auto; /* cannot be set explicitly with this syntax form */
  grid-auto-flow: row dense; /* can only set dense or not */
  grid-column-gap: 0; /* cannot be set explicitly with this syntax form */
  grid-row-gap: 0; /* cannot be set explicitly with this syntax form */
}
```
## `justify-content`

```
justify-content: center | start | end | space-between | space-around | space-evenly
```
![](/content/images/2017/03/justify-content.png)

`space-around` 沿着行轴在网格容器内均匀分布网格轨道，使得每个网格轨道在其任一侧具有相等的空间，在任一端具有一半大小的空间。

`space-between` 沿着行轴在网格容器内均匀地分布网格轨道，其中第一网格轨迹与网格容器的起始边缘齐平，并且最后一个网格轨迹与网格容器的结束边缘齐平。

`space-evenly` 沿着行轴在网格容器内均匀分布网格轨道，使得任何2个相邻网格轨道之间的空间相同。

## `align-content`
```
align-content: center | start | end | space-between | space-around | space-evenly
```
## ` justify-items`
```
justify-items: center | start | end | stretch
```
![](/content/images/2017/03/justify-items.png)

`stretch` 默认，填充网格区域的宽度
[🌰](https://igalia.github.io/css-grid-layout/alignment-demo.html)
## `align-items`
```
align-items: center | start | end | stretch
```

# Grid Items 属性
* grid-column-start
* grid-column-end
* grid-row-start
* grid-row-end
* grid-column
* grid-row
* grid-area
* justify-self
* align-self

##  `grid-column-start`, `grid-column-end` and `grid-row-start`, `grid-row-end`

通过参考特定的网格线来确定网格项中的网格位置。 grid-column-start / grid-row-start是item开始的网格线，grid-column-end / grid-row-end是item结束的网格线。

```
grid-column-start: auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]
grid-column-end: auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]
grid-row-start: auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]
grid-row-end: auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]
```

`auto` 默认值，没有为此属性指定网格线，因此项目将自动放置以填充网格，并且默认跨度为1。

`<custom-ident>` 可以是网格线的数字索引，或命名的网格线。

`[ <integer> && <custom-ident>? ]`  对于重复命名的网格线，整数值n将定义具有指定名称的第n个网格线。整数值不能为0。
![](/content/images/2017/03/Screen-Shot-2017-03-09-at-10-14-06-PM.png)
```
.a { grid-column-start: 1 bar; grid-column-end: 3 foo; }
.b { grid-column-start: 1 bar; }
.c { grid-column-start: -1 foo; }
```

`[ span && [ <positive-integer> || <custom-ident> ] ]`  提供指定网格项目的网格跨度的选项。此值与指定的网格线一起将确定网格项的位置。网格项将从指定的网格线跨越N个轨道。

![](/content/images/2017/03/Screen-Shot-2017-03-09-at-10-17-55-PM.png)

```
.item{
   grid-column-start: span 2;
   grid-column-end: 4;
}
```
如果 integer 不指定则默认为1

##  `grid-row` 和 `grid-column`
```
grid-row: <grid-line> [ / <grid-line> ]?
grid-column: <grid-line> [ / <grid-line> ]?
```
这是在同一声明中为相应维度设置起始行和结束行的缩写。 grid-row属性是grid-row-start和grid-row-end的缩写，而grid-column属性是grid-column-start和grid-column-end的缩写。
网格线值由斜杠分隔。斜线之前的值表示开始网格线，斜线后的值表示结束网格线。

## `grid-area`
```
grid-area: <grid-line> [ / <grid-line> ]{0,3}
```
此缩写的顺序是row start / column-start / row-end / column-end

## ` justify-self`[🌰](https://igalia.github.io/css-grid-layout/demo-alignment.html)
```
justify-self: center | start | end | stretch
```

## `align-self`
```
align-self: center | start | end | stretch
```
# 栗子
响应式布局

<iframe height='265' scrolling='no' title='CSS-GRID-Responsive' src='//codepen.io/GGICE/embed/jBByzr/?height=265&theme-id=dark&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/GGICE/pen/jBByzr/'>CSS-GRID-Responsive</a> by GGICE (<a href='http://codepen.io/GGICE'>@GGICE</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>
经典blog界面
<iframe height='265' scrolling='no' title='CSS-GRID-BLOG' src='//codepen.io/GGICE/embed/EWWZJR/?height=265&theme-id=dark&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='http://codepen.io/GGICE/pen/EWWZJR/'>CSS-GRID-BLOG</a> by GGICE (<a href='http://codepen.io/GGICE'>@GGICE</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>
在z轴上的布局
<iframe height='265' scrolling='no' title='CSS-GRID-BLOG' src='https://igalia.github.io/css-grid-layout/z-index.html' style='width: 100%; border: 1px solid #000;'>
</iframe>
# 参考

[CSS Grid Layout Module Level 1](https://www.w3.org/TR/css-grid-1/)

[css-tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)

[Codrops CSS Reference](https://tympanus.net/codrops/css_reference/grid/)

[Grid by Example](https://igalia.github.io/css-grid-layout/)
