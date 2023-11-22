---
title: vue-grid-layout
date: 2023-11-20 18:53:30
tags: Vue
categories: 
- programming
- Vue
---

## vue-grid-layout

### 特性

- 可拖拽
- 可调整大小
- 静态部件
- 拖拽和调整大小时进行边界检查
- 增减部件避免重建栅格
- 可序列化和还原的布局
- 自动化rtl支持
- 响应式

### 属性

#### GridLayout

- layout：数据源，其数据项为object，每条数据有i、x、y、w、h属性
- reponsiveLayouts：设置responsive为true，该配置作为栅格中每个断点的初始布局
- colNum：栅格系统的列数，值为自然数
- rowHeight：每行的高度，单位是像素
- maxRows：定义最大行数
- margin：栅格中元素的边距，值是两个number的数组，第一个元素是水平边距，第二个是垂直边距，单位像素
- isDraggable：是否可拖拽
- isResizable：元素是否可以调整大小
- isMirrored：是否可以镜像翻转
- autoSize：表示容器是否自动调整大小
- verticalCompact：标识布局是否垂直压缩
- useCssTransforms:是否使用css属性
- responsive：是否为响应式
- breakpoints：为响应式布局设置断点
- cols：设置每个断点对应的列数
- useStyleCursor：标识是否使用动态鼠标指针样式

#### GridItem

- i：栅格中的元素id
- x：标记栅格元素位于第几列
- y：标记栅格元素位于第几行
- w：标记栅格元素的初始宽度，值为colWidth的倍数
- h：标记栅格元素的初始高度，值为rowHeight的倍数
- minW：栅格最小宽度，如果w小于minW，minW会被w覆盖
- maxW：栅格元素的最大宽度，w大于maxW，maxW被覆盖
- minH
- maxH
- isDraggable：是否可拖拽，如果为null则取决于父容器
- isResizable
- static：是否为静态(无法拖拽、调整大小或被其他元素移动)
- dragignoreFrom：栅格元素中哪些子元素无法触发拖拽事件，值为css-like选择器
- dragAllowFrom：标识栅格元素中哪些子元素可以触发拖拽事件，值为 css-like 选择器
- resizeIgnoreFrom

### 基础使用

#### 引入

```js
import VueGridLayout from 'vue-grid-layout'
```

#### 加入到Vue组件

```js
export defaultt{
	components:{
		GridLayout:VueGridLayout.GridLayout,
		GridItem:VueGridLayout.GridItem
	}
}
```

#### 加入网格

```vue
<template>
  <div class="dashboard-container">
    <grid-layout
            :layout="layoutData"
            :col-num="12"
            :row-height="30"
            :is-draggable="true"
            :is-resizable="true"
            :is-mirrored="false"
            :vertical-compact="true"
            :margin="[10, 10]"
            :use-css-transforms="true">

        <grid-item v-for="item in layoutData"
                   :x="item.x"
                   :y="item.y"
                   :w="item.w"
                   :h="item.h"
                   :i="item.i"
                   :key="item.i">
            {{item.i}}
        </grid-item>
    </grid-layout>  
  </div>
</template>
```