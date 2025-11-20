# Grid Layout

## 对比Flexbox vs Grid

Flexbox的强项
- 一堆布局：沿单轴排列元素
- 空间分配：灵活的对齐和空间控制
- 响应式友好：自动折行和伸缩

遇到的挑战
- 二维矩阵布局：仪表盘、图库
- 精确定位需求： 圣杯布局、复杂表格
- 行列同时控制：需要更精细的控制

一维布局 vs 二维布局

## Grid的核心要素 

3个关键概念

### 1、Grid容器（Grid Container）

一旦声明，内部的直接子元素自动称为Grid项

```css
.container {
  display: grid;
}
```

### 2、网格轨道（Grid Tracks）
- 行轨道
- 列轨道
- 内容就在这些轨道里面排列

### 3、网格线（Grid Lines）
- 分割轨道的坐标系
- n列网格 = n+1条垂直直线
- m行网格=m+1条水平线

## 最基础属性

grid-template-columns & groid-template-rows 定义网格的列轨道和行轨道

```css
.container {
  grid-template-columns: 200px 1fr 100px;
}

.container2 {
  grid-template-rows: 100px auto;
}
```

gap 定义网格轨道之间的距离

```css
.container {
  gap: 10px;

  gap: 20px 10px; // 行列
}
```

注意： 从一维思考到二维思考 同时考虑行和列

---

固定单位：px、em、rem
- 提供可预测、绝对的尺寸
- 适用于固定侧边栏、导航栏等场景

相对单位：%、vw、vh
- 相对于容器或视口的尺寸
- 善于创建与父容器紧密关联的布局

自动单位：auto
- 由内容决定轨道尺寸
- 遵循内容优先原则

**弹性单位**： fr
- Grid布局最具特性的单位
- 代表可用空间的一份

理解：传统单位的局限性
在没有fr单位之前，我们面临的挑战

百分比的问题
```css
.container {
  display: grid;
  grid-template-columns: 33.3% 33.3% 33.3%;
  gap: 20px;
}

/*
长度超出 出现滚动条
需要计算 calc
需要手动计算剩余空间
gap属性会影响总宽度
响应式调整困难
*/
```

fr单位的优势
fr单位让空间分配变得简单直观

等宽三列布局
```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}

/*
核心优势：
自动计算：浏览器处理所有计算
间距友好：自动扣除gap空间
比例清晰：直观表达空间分配意图
*/
```

## fr 单位的工作原理 (fr如何分配剩余空间)
```css
.container {
  display: grid;
  width: 800px;
  grid-template-columns: 200px 1fr 2fr;
  gap: 20px;
}
```

计算过程：
1、总宽度：800px
2、固定宽度： 200px 第一列
3、间距总和：40px (2个gap x 20px)
4、剩余空间：800px - 200px - 40px = 560px
5、fr总份数：1fr + 2fr = 3fr
6、每份大小：560px / 3 = 186.67px

## repeat() 函数 简化重复模式

基础用法

```css
grid-template-columns: 1fr 1fr 1fr 1fr 1fr;

/* repeat */
grid-template-columns: repeat(5, 1fr);
```

复杂模式重复
```css
/* repeat */
grid-template-columns: repeat(3, 100px 1fr);

/* 等价于 */
grid-template-columns: 100px 1fr 100px 1fr 100px 1fr;
```

--- 

精准控制布局
1、网格线定位 —— 像素级精准控制 
2、span关键字 —— 动态跨域技术
3、命名网格区域 —— 语义化布局方案

### 基于网格线的精准定位
核心属性

```css
.item {
  /* 列方向定位 */
  grid-column-start: 1;
  grid-column-end: 3;

  /* 行方向定位 */
  grid-row-start: 2;
  grid-row-end: 4;
}
```

简洁写法：

```css
.item {
  /* 列 */
  grid-column: 1 / 3;

  /* 行 */
  grid-row: 2 / 4;
}
```

负数索引技巧

```css
.item {
  /* 从第一列延伸到最后一列 */
  grid-column: 1 / -1;
}
```

### 使用span关键字实现动态跨越
有时我们不关心具体的结束位置，只关心要跨越多少个单元格

基本语法
```css
.item-a {
  grid-column: 2 / span 2;
}

.item-b {
  grid-column: span 3;
}
```

span的灵活性在响应式应用

```css
.responsive-item {
  /* 在小屏幕上跨越2列 */
  grid-column: span 2;
}

@media (min-width: 768px) {
  /* 在大屏上跨越4列 */
  grid-column: span 4;
}
```

### 命名网格区域
在响应式设计中优势

grid-template-areas: xxx;

justify-items: center;
align-items: center | end;
align-self: stretch;
justify-self: center;


## 总结

1、轨道间距 gap 无边缘问题 替代传统的margin方案
2、网络整体对齐 justify-content / align-content  (align-items: stretch;)
3、单元格内项目对齐 justify-items/self align-items/self

## 响应式网格与自动布局

之前是media方案

minmax() 函数 为轨道尺寸设定弹性便捷 （防止浪费空间 100px 小屏幕上压缩 1fr） min: 轨道的最小尺寸，保证内容的可读性 max：轨道的最大尺寸，通常用1fr等弹性单位

```css
.grid-container {
  display: grid;
  grid-template-columns: minmax(300px,1fr) minmax(300px,1fr); /* 小于600px 出现滚动条保证内容的可读性 大于600 1fr 1fr 占满屏幕两列 */
} 
```


auto-fit vs auto fill 两种自动布局模式的区别 这两个关键字可以在空间内尽可能创建制定尺寸的轨道，但处理剩余空间的方式不同
  auto-fill 保守填充
  auto-fit  适应填充 *大多少场景

  99% 的响应式卡片布局都应该用 auto-fit，而不是 auto-fill。

```css
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));

grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

repeat(auto-fit, minmax(250px, 1fr)) = 响应式、自适应、永远贴边、永远不小于250px的完美网格布局，现代前端开发中几乎是标配写法。

minmax(250px, 1fr) 在 CSS Grid / Flex 中的详细解释
它是 CSS 中 minmax() 函数的典型用法，常出现在 grid-template-columns 或 grid-auto-columns 中。

250px最小值（硬约束）这条列在任何情况下宽度都不能小于 250px。即使容器很窄，也会强制至少 250px（可能导致溢出或换行）。1fr最大值（弹性约束）当容器还有剩余空间时，这条列可以弹性拉伸，和其他 fr 单位一起平分剩余空间。

关键点总结：

250px 是铁底：列永远不会比 250px 窄（除非容器本身比 250px 还窄，那就会溢出或被浏览器强制换行）。
1fr 是弹性上限：空间多的时候，它会积极抢占地盘，和其他 fr 单位一起把容器塞得满满的，不留白。

```css
/* 手机优先，最小 280px（常见卡片宽度） */
repeat(auto-fit, minmax(280px, 1fr))

/* 更宽的卡片，比如后台管理面板 */
repeat(auto-fit, minmax(350px, 1fr))

/* 想限制最大宽度不至于太大（超大屏也不会太宽） */
repeat(auto-fit, minmax(300px, 400px))   /* 最大400px */
```

一句话总结：
minmax(250px, 1fr) = “至少给我250px宽，剩下的空间能分多少我拿多少” —— 这是实现完美响应式卡片网格的核心秘诀。

---

grid-auto-flow: dense; /* 关键：dense 自动填空 */
grid-column: span 2;
grid-row: span 2;
grid-auto-rows: 100px;

/* 关键！继承父网格轨道 */
grid-template-columns: subgrid; 子网格不再是独立的网格，而是直接“借用”父网格的列宽和行高，实现完美的对齐。里面的子元素会直接使用父网格的 100px、1fr、200px 三条列轨道，完全自动对齐，再也不用重复写列宽！