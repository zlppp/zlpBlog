---
title: grid网格布局
date: 2020-09-28 20:16:12
tags:
---
目前css布局方案中，网格布局可以算是最强大的布局方案了。
flex与grid的区别：
flex：可以看成 一维布局，容器存在两根轴：水平主轴和垂直交叉轴
grid：二维网格布局，将容器分成多个网格，可以更好的操作行和列，更加灵活

**grid并不能代替flex，两者一起用才更加强大**

>阮一峰的博客中grid的属性已经写得详细了
>http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html


容器的主要属性：
```javascript
display: grid | inline-grid | subgrid // 将容器设置为grid布局，行内grid
grid-template-columns: 100px 100px     // 创建网格列数(创建了两个100px的)
grid-template-row: // 创建网格行数
grid-gap: // 行和列的间隙
justify-items: start | end | center | stretch // 设置单元格内容水平位置
align-items: start | end | center | stretch // 设置单元格内容垂直位置
```
## 实际应用
### 两栏布局
![](/zlpBlog/images/grid/grid1.png)
```javascript
<div id="container">
  <div class="item">left</div>
  <div class="item">right</div>
  <div class="item">left</div>
  <div class="item">right</div>
  <div class="item">left</div>
  <div class="item">right</div>
</div>

#container{
  display: grid;
  grid-template-columns: 100px 100%; 
  grid-template-rows: 50px 60px 70px;
  grid-gap: 10px;
}
```

### 三明治布局
![](/zlpBlog/images/grid/grid2.png)
```javascript
#container{
  display: grid;
  grid-template-rows: 100px 100% 100px;
  grid-gap: 10px;
}
```

### 圣杯布局
![](/zlpBlog/images/grid/grid3.png)
```javascript
#container{
  display: grid;
  grid-template-rows: repeat(3, 100px); // repeat(): 简化重复的值，等同于grid-template-rows: 100px 100px 100px
  grid-gap: 10px // 行与行的间隔
}
.item-1 {
  grid-column: 1/4 
}
.item-5 {
  grid-column: 1/4
}
.item {
  background: #eee;
  text-align: center;
}
```

### from表单
![](/zlpBlog/images/grid/grid3.png)

```javaript
#container{
  display: grid;
  grid-template-columns: 3fr 2fr; // 将网格分成两份，左边占5/3, 右边占5/2
  grid-gap: 10px; // 网格间距为10px
}
.form {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 1fr 2fr; // 再将左边网格分成两栏
 text-align: right;
}
.button {
  grid-column: 1/3; // 按钮从第一条网格线开始  第三条网格线结束
  justify-self: end; // 水平居右对齐
}
```