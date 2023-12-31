---
title: flex
date: 2023-11-27 13:50:07
tags: CSS
categories:
- programming
- CSS
---

## Flex布局
### flex布局是
> Flexible Box、弹性布局，为盒模型提供最大的灵活性
> 设置为flex布局后，子元素的float、clear和vertical-align属性将失效

```css
.box {
	display:flex;
	// 行内元素可以使用flex
	display:inline-flex;
}
```

### 基本概念
- 容器：采用flex布局的元素
- 项目：采用flex布局的元素的子元素

![image-20230921134646539](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211346582.png)

### 容器的属性
- flex-dirction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

#### flex-dirction
- 决定主轴的方向(项目排列方向)
```css
.box {
	// 水平方向起点左端 | 水平方向起点右端 | 垂直方向起点上方 | 垂直方向起点下沿
	flex-dirction: row | row-reverse | column | column-reverse
}
```

![image-20230921135050409](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211351519.png)

#### flex-wrap
- 默认情况下项目排在一条线上(轴线)
- flex-wrap属性定义，如果一条轴线排不下如何换行
```css
.box{
	// 不换行 | 换行，第一行在上方 | 换行第，一行在下方
	flex-wrap: nowrap | wrap | wrap-reverse;
}

```

#### flex-flow
- flex-direction和flex-wrap的属性的简写
- 默认值是row nowrap
```css
.box {
	flex-flow:<flex-direction> || <flex-wrap>
}
```

#### justify-content
- 项目在主轴上的对其方式
```css
.box {
	// 左对齐 | 右对齐 | 居中对齐 | 两端对其，项目之间的间隔相等 | 每个项目两侧间隔相等
	justify-content: flex-start | flex-end | center | space-between | space-around
}
```

#### align-items
- 项目在交叉轴上如何对其
```css
.box {
	// 交叉轴起点对其 | 终点对其 | 中点对其 | 项目第一行文字的基线对其 | 占满整个容器的高度(默认)
	align-items: flex-start | flex-end | center | baseline | stretch
}
```

#### align-content属性
- 定义了多根轴线的对齐方式
- 若项目只有一根轴线，该属性不起作用
```css
.box {
	align-content: flex-start | flex-end | center | space-between | stretch
}
```

### 项目的属性
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-flex

#### order
- 定义项目排列的顺序
- 数值越小，排列越靠前，默认0
```css
.item {
	order: <interger>
}
```

#### flex-grow
- 项目的放大比例
- 默认0，即如果存在剩余空间，也不放大
```css
.item {
	// 若所有项目的flex-grow属性都为1，则它们将等分剩余空间
	flex-grow: <number>; /*default 0*/
}
```

#### flex-shrink
- 定义了项目缩小的比例
- 默认1，即如果空间不足，项目会缩小
```css
.item {
	// 若所有项目的flex-shrink属性都为1,当空间不足时，等比缩小
	flex-shrink: <number> /*default 1*/
}
```

#### flex-basis
- 定义了再分配多余空间之前，项目占据的主轴空间
- 浏览器根据这个属性，计算主轴是否有多余空间
- 默认值auto，即项目本来的大小
```css
.item {
	// 设置为根width或height属性用于的值，则项目占据固定空间
	flex-basis: <length> | auto: /*default：auto*/
}
```

#### flex
- flex-grow、flex-shrink和flex-basis的合并简写，默认值为 0 1 auto
- 后两个属性可选
```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

#### align-self
- 允许单个项目有与其它项目不一样的对对齐方式
- 可覆盖align-items属性
- 默认auto，即继承父元素的align-items属性，无父元素等同于stretch
