## 水平居中布局
html结构
```html
<ul class="parent">
  <li class="child">梁凤波</li>
</ul>
```
## css实现方法
#### 方法一：inline-block + text-align
- 优点：浏览器兼容性比较好
- 缺点：text-align属性具有继承性，导致子元素的文本也是居中显示的
```css
.parent {
    text-align: center;
}

.child {
    display: inline-block;
}
```

#### 方法二：table || block + margin 
- 优点：只需要对子元素进行设置就可以达到效果
- 缺点：如果子元素脱离文档流，margin的属性值则无效了
```css
.parent {
}

.child {
    display: table;
    margin: 0 auto;
}

或者 

.child {
    display: block;
    margin: 0 auto;
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
    left: 50%;
    transform: translateX(-50%);
}
```

#### 方法四：flex + justify-content
- 优点：简单快捷
- 缺点：css3新增的属性，浏览器支持情况不好
```css
.parent {
    display: flex;
    justify-content: center;
}

.child {
}
```

#### 方法五：flex + margin
- 优点：简单快捷
- 缺点：css3新增的属性，浏览器支持情况不好
```css
.parent {
    display: flex;
}

.child {
    margin: auto;
}
```

#### 方法六：grid + justify-self
- 优点：简单快捷
- 缺点：css3新增的属性，浏览器支持情况不好
```css
.parent {
    display: grid;
}

.child {
    justify-self: center;
}
```

#### 方法七：grid + justify-content
- 优点：简单快捷
- 缺点：css3新增的属性，浏览器支持情况不好
```css
.parent {
    display: grid;
    justify-content:center;
}

.child {
}
```