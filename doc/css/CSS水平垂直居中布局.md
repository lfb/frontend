## 水平垂直居中布局
水平垂直居中布局就是既要水平居中，也要垂直居中

```html
<div class="parent">
  <div class="child">水平垂直居中布局</div>
</div>
```

## 实现方式

#### 方法一：垂直：table-cell + vertical-align + 水平：block + margin
- 优点：兼容性好
- 缺点：vertical-align属性具有继承性，会导致父级元素的文本也是居中显示的
```css
.parent {
    display: table-cell;
    vertical-align: middle;
}

.child {
    display: block;
    margin: 0 auto;
}
```

#### 方法二：absolute + margin auto
- 父元素设置相对定位，子元素设置绝对定位，margin值设置auto，四个方向设置为0
- 注意：子元素需要定宽高
```css
.parent {
    position: relative;
}

.child {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

#### 方法三：absolute + transform
- 优点：父级元素是否脱离文档流，不影响子级元素水平居中效果
- 缺点：transform是css3新增的属性，浏览器支持情况不好

```css
.parent {
    position: relative;
    height: 100px;
    border: 1px solid red;
}

.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

#### 方法四：flex +  justify-content + align-items
```css
.parent {

    display: flex;
    align-items: center;
    justify-content: center;
}

.child {
}
```

#### 方法五：grid + align-self + justify-self
```css
.parent {
    display: grid;
}

.child {
    align-self: center;
    justify-self: center;
}
```