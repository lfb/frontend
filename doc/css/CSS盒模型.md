## 盒模型
**盒模型是什么？**
页面的所有元素都可以被看作一个矩形盒子，这个盒子包含元素的 `context, padding, border, margin`。

通过修改 **box-sizing：context-box（默认） | border-box**
属性可以改变计算盒子大小的方式。

**context-box**: width/height = context，下面的盒子的宽度设置为100px，那么它的盒模型属性分别为：
- context = 100px 
- padding 10 * 2 = 20px
- border 10 * 2 = 20px
- margin 10 * 2 = 20px

```css
.box {
  box-sizing: context-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  margin: 10px auto;
  border: 10px solid #2d8cf0;
}
```

**border-box**: width/height = context + padding + border，下面的盒子的宽度设置为100px，那么它的盒模型属性分别为：
- context = 60px 
- padding 10 * 2 = 20px
- border 10 * 2 = 20px
- margin 10 * 2 = 20px

```css
.box {
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  margin: 10px auto;
  border: 10px solid #2d8cf0;
}
```

**JS获取盒子宽度**

oDiv.style.width
```
1 只能操作行内样式
2 只包括内容区，不包括border和padding部分
3 返回值带单位，返回值的数据类型是string
```

oDiv.offsetWidth
```
1 它获取的数据类型是number，如350，没有单位
2 结果包括内边距，包括边框
```

oDiv.currentWidth
```
1 可获取样式表中的样式。
2 它获取的数据类型是字符串，如300px
3 结果不包括内边距，不包括边框
4 只能用在IE浏览器中
```
getComputedStyle(oDiv,null).width
```
1 可获取样式表中的样式。
2 它获取的数据类型是字符串，如300px
3 结果不包括内边距，不包括边框
4 只能用在非IE浏览器中
5 返回值是带单位的，数据类型是字符串。
```

 Element.getBoundingClientRect()方法返回元素的大小及其相对于视口的位置
 ```js  
var box = document.querySelector('.box')
box.getBoundingClientRect().width
 ```
 
## 外边距折叠
垂直方向上的两个外边距相遇时，会折叠成一个外边距，折叠后外边距的高度等于两者中较大的那个高度。

比如：

```html
<div class="box1"></div>
<div class="box2"></div>
```

```css
.box1 {
  width: 100px;
  height: 100px;
  background: red;
  margin-bottom: 100px;
}

.box2 {
  /* 2 个盒子发生折叠，实际上的外边距是 200px */
  margin-bottom: 200px;
  width: 100px;
  height: 100px;
  background: blue;
}
```

## BFC 块级格式上下文

BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，它规定了内部如何布局，是决定块盒子的布局及浮动相互影响范围的一个区域，并且与这个区域外部毫不相干。

BFC内部特性：
1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如 此。
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算

## BFC创建方式
- 根元素
- float属性不为none
- position为absolute或fixed
- display为inline-block, table-cell, flex 等
- overflow不为visible( hidden,scroll,auto) 的元素

## BFC的作用及原理

#### 两栏布局
第3条：每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。因此，虽然存在浮动的元素.left，但.main的左边依然会与包含块的左边相接触。

```html
<div class="left">left</div>
<div class="main">main</div>
```
```css
.left {
  float: left;
  width: 180px;
  height: 300px;
  background: #f66;
}

.main {
  height: 420px;
  background: #fcc;
}
```

第4条：BFC的区域不会与float box重叠。我们可以通过通过触发main生成BFC， 来实现自适应两栏布局。
```css
.left {
  float: left;
  width: 180px;
  height: 300px;
  background: #f66;
}

.main {
  overflow: hidden;
  height: 420px;
  background: #fcc;
}
```
![两栏布局](/images/css/box-sizing/left-main.png)

#### 清除内部浮动
开始，box 下的元素都设置为浮动，脱离文档，此时 box 的高度为 0
```html
<ul class="box">
  <li class="item"></li>
  <li class="item"></li>
  <li class="item"></li>
</ul>
```
```css
.box {
}

.item {
  float: left;
  width: 120px;
  height: 120px;
  background: #f66;
}
```
修改：根据BFC布局规则第6条：计算BFC的高度时，浮动元素也参与计算，为达到清除内部浮动，我们可以触发box生成BFC，那么box在计算高度时，box内部的浮动元素item也会参与计算。
```css
.box {
  overflow: hidden;
  border: 5px solid #f00;
}

.item {
  float: left;
  width: 120px;
  height: 120px;
  background: #f66;
}
```

#### 防止垂直 margin 重叠
垂直方向上的两个外边距相遇时，会折叠成一个外边距，折叠后外边距的高度等于两者中较大的那个高度。
```html
<ul class="box1"></ul>
<ul class="box2"></ul>
```
```css
.box1 {
  width: 120px;
  height: 120px;
  background: #f66;
  margin-bottom: 100px;
}

.box2 {
  margin-top: 100px;
  width: 120px;
  height: 120px;
  background: #f66;
}
```
如果想要防止垂直 margin 重叠，根据BFC布局规则第二条：Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
```html
<ul class="wrap-box"></ul>
<div class="inner">
  <ul class="inner-box"></ul>
</div>
```

```css
.wrap-box {
  width: 120px;
  height: 120px;
  background: #f66;
  margin-bottom: 100px;
}

.inner {
  overflow: hidden;
}

.inner-box {
  margin-top: 100px;
  width: 120px;
  height: 120px;
  background: #f66;
}
```
其实以上的几个例子都体现了BFC布局规则第五条：BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。






