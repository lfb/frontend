## CSS3多列布局-columns
- columns-count：定义列的数量：参数：auto | number值
- columns-width
- column-gap
- column-rule
- column-span
- columns-fill(兼容性不好)

![CSS3多列布局](/images/css/layout/CSS3多栏布局.png)

#### columns-width 规定列的宽度
定义列的宽度，参数：auto | length

```html
<div class="parent">
    <div class="column1">column1</div>
    <div class="column2">column2</div>
    <div class="column3">column3</div>
</div>
```

```css
.parent {
    column-count: 3;
    column-width: 200px;
    /*等同于*/
    columns: 3 200px;
}
```

#### column-gap 列间的间隔
```css
.parent {
    column-count: 3;
    column-width: 200px;
    column-gap: 20px;
}
```

#### column-rule-width 设置列之间的颜色规则
参数：
- column-rule-width： 列表的边框宽度
- column-rule-color： 列表的边框颜色
- column-rule-style：列表的边框样式

```css
.parent {
    column-count: 3;
    column-width: 200px;
    column-gap: 20px;
    column-rule-width: 5px;
    column-rule-color: #2db7f5;
    column-rule-style: dashed;
    /*等于*/
    column-rule: 5px #2db7f5 dashed;
}
```

#### column-span 定义一个列元素是否跨列
```css
.parent {
    column-count: 2;
    column-width: 200px;
    column-gap: 20px;
    column-rule-width: 5px;
    column-rule-color: #2db7f5;
    column-rule-style: dashed;
    /*等于*/
    column-rule: 5px #2db7f5 dashed;
}

.column1 {
    background: red;
}

.column2 {
    background: green;
}

.column3 {
    background: blue;
    column-span: all;
}
```

#### columns-fill(兼容性不好) 规定如何对列进行填充：
参数：
- auto 根据内容撑开
- balance 根据最高的列的高度为准
```html
<div class="parent">
    <div class="column1">
        column1
    </div>

    <div class="column2">
        <p>column2column2</p>
        <p>column2column2</p>
    </div>
    <div class="column3">
        <p>column3</p>
        <p>column3</p>
        <p>column3</p>
        <p>column3</p>
        <p>column3</p>
    </div>
</div>
```

```css
.parent {
    column-count: 3;
    column-width: 200px;
}

.column1, .column2, .column3 {
    column-fill: balance;
}

.column1 {
    background: red;
}

.column2 {
    background: green;
}

.column3 {
    background: blue;
}
```