## 水平居中布局
```html
<ul class="parent">
  <li class="child">水平居中布局</li>
</ul>
```

#### 方法一：inline-block + text-align
```css
.parent {
    text-align: center;
}

.child {
    display: inline-block;
}
```

#### 方法二：table || block + margin 
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
```css
.parent {
    display: flex;
    justify-content: center;
}

.child {
}
```

#### 方法五：flex + margin
```css
.parent {
    display: flex;
}

.child {
    margin: auto;
}
```

#### 方法六：grid + justify-self
```css
.parent {
    display: grid;
}

.child {
    justify-self: center;
}
```

#### 方法七：grid + justify-content
```css
.parent {
    display: grid;
    justify-content:center;
}

.child {
}
```

