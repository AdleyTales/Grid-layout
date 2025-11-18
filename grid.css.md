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

## Geid的核心要素 

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
网格线定位、span关键字、命名区域


