## 垂直居中布局
html结构
```html
<ul class="parent">
  <li class="child">梁凤波</li>
</ul>
```
## css实现方法
#### 方法一：line-height
```css
.parent {
    text-align: center;
}

.child {
    display: inline-block;
}
```

#### 方法二：table-cell + vertical-align
- 优点：兼容性比较好
- 缺点：vertical-align属性具有继承性，会导致父级元素的文本也是居中显示的
```css
.parent {
    display: table-cell;
    height: 100px;
    border:1px solid red;
    vertical-align: middle;
}

.child {
}
```

#### 方法三：absolute + transform
- 优点：父级元素是否脱离文档流，不影响子级元素水平居中效果
- 缺点：transform是css3新增的属性，浏览器支持情况不好
```css
.parent {
    position: relative;
}

.child {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```

#### 方法四：flex + align-items

```css
.parent {
    display: flex;
    align-items: center;
}

.child {
}
```

#### 方法五：grid + align-self
```css
.parent {
    display: grid;
}

.child {
    align-self: center;
}
```