---
title: grid
date: 2023-11-27 16:12:47
tags: CSS
categories:
- programming
- CSS
---

## Grid布局

### 概述
grid布局也称网格布局，是最强大的css布局方案

![image-20230918085932162](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309180859228.png)

### 有关概念

#### 容器和项目
- 采用网格布局的区域称为容器
- 容器内采用网格定位的子元素称为项目
```vue
<div>
	<div><p>1</p></div>
</div>
```
- 如上方代码所示，最外层div是容器
- 内层的div是项目，只能是顶层的子元素，不包括项目的子元素
- grid布局只对项目生效

#### 行和列
- 行：水平区域--row
- 列：垂直区域--column
- 沟槽：行与行、列与列之间的间隙--gutter

#### 单元格
行与列的交叉区域

#### 网格线
- 划分网格的线
- 水平网格先划出行，网格线比行数多一
- 垂直网格线划出列，网格线比行数多一

### 容器属性
- 容器属性 <
- 项目属性

#### display

#### grid-template-colums属性&grid-template-rows属性
- colums指定每一列的宽度
- rows指定每一行的高度

```css
/* 使用绝对单位 */
.container{
	display:grid;
	grid-template-colums: 100px 100px 100px;
	grid-template-rows: 100px 100px 100px;
}
/* 使用百分比 */
.container{
	display:grid;
	grid-template-colums: 33.3% 33.3% 33.3%;
	grid-template-rows: 33.3% 33.3% 33.3%;
}

/* 避免重复写 */
.container{
	display:grid;
	grid-template-colums: repeat(3, 33.3%);
	grid-template-rows: repeat(3, 33.3%);
	grid-template-columns: repeate(2, 100px 2px 80px);
}
```

![image-20230921091916198](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309210919365.png)

```css
/*auto-fill*/
// 单元格固定，容器大小不确定
.container{
	display:grid;
	grid-template-colums: repeat(auto-fill, 100px);
}

/* fr */
// 若两列宽度分别为1fr、2fr，则后面这列是前面的两倍
.container{
	display:grid;
	grid-template-colums: 1fr 1fr;
}

/* 网格线命名 */
.container {
	display:grid;
	grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
	grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4;]
}

/* 布局实施 */
// grid-temlate-colummns属性对网页布局非常有用，两栏式布局如下
.container {
	display:grid;
	grid-template-columns:70% 30%;
}

// 传统的12网格布局
.container {
	display:grid;
	grid-template-columns:repeat(12,1fr);
}
```

#### grid-gap
- grid-gap-columns用于设置列间距
- grid-gap-rows用于设置行间距
- 现在的标准中可以省略前面的grid-，写成row-gap、column-gap
```css
.container {
	grid-gap-columns:20px;
	grid-gap-rows:20px;
}
```
- grid-gap是grid-gap-column加上grid-gap-rows的合并的简写，现在可写为gap
```css
// grid-gap: <grid-row-gap> <grid-column-gap>
// 如果省略第二个值，那么浏览器默认认为第二个值等于第一个
.container {
	grid-gap:20px 20px;
}
```

#### grid-template-areas属性
- 网格布局允许指定区域(area)，一个区域由单个或多个单元格组成
- 区域可以使用grid-template-areas属性来定义
```css
.container {
	display:grid;
	grid-tempalte-columns:100px 100px 100px;
	grid-template-rows:100px 100px 100px;
	/*先划分出九个单元格，然后定名未a到i的九个区域，分别对应9个单元格*/
	grid-template-areas:'a b c'
						'd e f'
						'g h i'
						
	// 多个单元格合并
	// 将9个单元格合并为a、b、c三个区域
	grid-template-areas:'a a a'
						'b b b'
						'c c c'
						
	// 布局实例be like
	grid-template-areas:'header header header'
						'main main sidebar'
						'footer footer footer'
						
	// 某些区域不需要，使用 . 代替
	grid-template-areas:'a . c'
						'd . f'
						'g . i'
}
```

#### grid-auto-flow属性
- 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格
- 默认顺序是先行后列，即填满第一行，再开始放入第二行
- 这个顺序由grid-auto-flow决定，默认为row，即放置的方向是行
```css
// 容器元素放置顺序
.container{
	// 行(默认)
	grid-auto-flow:row;
	
	// 列
	grid-auto-flow:column;
	
	// 除了某些指定位置的元素其他元素的排列顺序
	grid-auto-flow:row dense;
	grid-auto-flow:column dense;
}

```

#### justify-items/align-items/place-items属性
- justify-items属性设置单元格内容的水平位置(左中右)
- align-items设置单元格内容的垂直位置(上中下)
```css
.container {
	justify-items: start | end | center| stretch;
	align-items: start| end | center | stretch;
}
```
- justify和align属性值为start时预览：

![image-20230921101251999](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211012038.png)

![image-20230921101313631](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211013669.png)

- place-items是align-items和justify-items的合并简写
```js
place-items:<align-items> <justify-items>
```

#### justify-content/align-content/place-content属性
- justify-content属性是整个内容区域在容器里面的水平位置(左中右)
- align-content属性是整个内容区域在容器里面的垂直位置(上中下)
```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```
如： justify-content: start &  justify-content: space-around

													 ![image-20230921102343585](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211023627.png)

![image-20230921102522919](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211025961.png)

- start：对其起始边框
- end
- center
- stretch：拉伸至整个边框
- space-around：每个项目两侧相等
- space-between：项目之间间隔相等，项目容器边框之间没有间隔
- space-evenly：相等，项目容器之间也是同样长度的间隔

#### grid-auto-columns/grid-auto-rows属性
- 使浏览器自动创建的多余网格的列宽和行高

- 自动创建网格之外的项目宽高

- 例子：`grid-auto-rows:100px;`

  ![image-20231127145009600](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311271450685.png)

#### grid-template属性 & grid属性
- grid-template是grid-template-columns、grid-template-rows、grid-temlate-areas这三个属性的合并简写
- grid是grid-template-columns、grid-template-rows、grid-temlate-areas、grid-auto-rows、gird-auto-colums、grid-auto-flow这六个属性的合并简写

### 项目属性
- 容器属性
- 项目属性 <

#### grid-column-start/end & grid-row-start/end属性
```css
.item-1 {
	grid-column-start：2；
	grid-column-end：4；
}
```

![image-20230921105615486](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211056544.png)

```css
.item-1 {
	grid-column-start：1;
	grid-column-end：3;
	grid-row-start: 2;
	grid-row-end: 4;
}
```

![image-20230921105806486](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211058525.png)

```css
// 除了指定为第几个网格线，还可以指定为网格线的名字
.item-1 {
	grid-column-start: header-start;
	grid-column-end:header-end;
}
```

```css
// 可以使用span关键字，表示"跨越"，即左右边框(上下边框)之间跨越多少个网格
.item-1 {
	grid-column-start:span 2;
	// or
	grid-column-end:span 2;
	
	// 若产生重叠血药使用z-index属性指定项目的重叠顺序
}
```

![image-20230921110635038](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211106079.png)

#### grid-column/row属性
grid-column/row是grid-column-start/end & grid-row-start/end的合并简写
```css
.item {
	grid-column: <start-line> / <end-line>;
	grid-row: <start-line> / <end-line>;
}
```

#### grid-area属性
- 指定项目放在哪一个区域
```css
.item-1 {
	grid-area: e;
	
	// 可以通过grid-column-start/end & grid-row-start/end合并简写直接指定项目的位置
	
	grid-name: <row-start> / <column-start> / <row-end> / <column-end>
}
```

![image-20230921115552530](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211155686.png)

#### justify-self/align-self/place-self属性
- justify-self属性设置单元格内容的水平位置(左中右)，根justify-item类似，但只作用于单个项目
- align-self属性设置单元格内容的垂直位置(上中下)
```css
.item {
	justify: start | end | center | stretch;
	align-slef : start | end | center | stretch;
}
```
- justify设置为start如下

  ![image-20230921120531613](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309211205863.png)
