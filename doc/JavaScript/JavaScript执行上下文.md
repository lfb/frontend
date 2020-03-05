## 执行上下文 Ececution Context
执行上下文指当前的执行环境中的，变量、函数声明，参数，作用域链和this等信息。

包含的3个重要属性：
1. 变量对象（Variable Object, VO）
2. 作用域链（Scope Chain）
3. this

```js
const ExecutionContext = {
    VO: window, // 变量对象
    ScopeChain: {}, // 作用域链
    this: window
};
```


包含2个上下文：
1. 全局执行上下文（只有一个）
2. 函数执行上下文（每次调用函数都会创建一个新的上下文）
> JS引擎首先会创建一个执行上下文栈，作用是管理上下文。在JS解释器初始化时首先创建一个全局上下文且压入上下文栈顶中，随着遇到函数调用时，会创建一个新的函数执行上下文且压入到上下文栈顶中，随着函数执行完后，会被上下文栈顶弹出，直回到全局上下文。

例子：
```js
var a = 10
fn1()

function fn1 () {
    fn2()
}

function fn2 () {
}

/* ECS (Execution Context Stack) */
ECS = ['globalContext']
ECS.push('<fn1> functionContext')
ECS.push('<fn2> functionContext')
ECS.pop()
ECS.pop()
ECS = ['globalContext']   
```

#### 执行上下文的生命周期
执行上下文的生命周期分为2个：创建阶段和执行阶段。
- 创建阶段：
    - 创建变量对象
    - 建立作用域链
    - 确定 this 的指向
- 执行阶段：
    - 变量的赋值
    - 函数的引用
    - 执行其他的代码
- 执行完毕后，从上下文栈顶弹出，等待垃圾回收

## 变量对象
变量对象全称：Variable Object，变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。因为不同执行上下文下的变量对象稍有不同，分为：全局上下文下的变量对象和函数上下文下的变量对象。
- 进入上下文阶段：
    - 如果是函数上下文，会处理函数参数：如果没有传入，则初始化参数值为undefined
    - 函数声明：如果发生函数名字相同，则后者会覆盖前者
    - 变量声明：初始化变量为undefined，如果发生与其他变量或者函数声明同名，则会忽略 
- 代码执行阶段：
    - 按顺序执行，进行修改赋值

#### 全局上下文
```js
[[global]] = {
    Math: <..>,
    String: <...>,
    window: global
}
```

#### 函数上下文
函数有个特别的地方就是用 AO 来代表 VO，其实他们是同样的东西。

```js
function fn(a, b) {
   function bar() {}
   var c = 30
   var d = function baz() {}
}
fn(10, 20)
```

进入上下文阶段，此时函数的 VO 为：
```js
VO = {
    arguments: {
        0: 10,
        1: 20,
        length: 2
    },
    bar: <reference to function 'bar'>
    c: undefined,
    d: undefined
}
```

进入代码执行阶段，此时函数的 VO 为：
```js
VO = {
    arguments: {
        0: 10,
        1: 20,
        length: 2
    },
    bar: <reference to function 'bar'>,
    c: 30,
    d: <reference to FunctionExpression 'd'>
}
```

## 作用域链
当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

JavaScript 采用的是静态词法作用域，也就是说函数的作用域在函数定义的时候就决定了，因为函数有一个内部属性 [[scope]]，当函数创建的时候，就会保存所有父变量对象到其中，你可以理解 [[scope]] 就是所有父变量对象的层级链，但是注意：[[scope]] 并不代表完整的作用域链！

代码：
```js
var a = 10
function fn(a) {
    var b = 30
}
fn(20)
```

整个流程，全局上下文入栈：
```js
let ECS = ['globalContext']
```

此时的 fn 作用域链进行链接，此时这个作用域链是词法作用域生成的作用域链
```js
fn.[[scope]] = [
    globalContext.VO
]
```

当函数执行的时候，fn 函数上下文压入栈顶：

```js
ECS.push('<fn functionContext>')
```

然后进入上下文创建阶段：
1. 初始化变量对象
    1. 初始化arguments, 函数声明，变量声明 
```js
fnContext = {
    AO: {
        arguments: {
            0: 20
            length: 1
        },
        b: undefined
    },
    Scope: fn.[[scope]]
}
```

接着把 AO 压入 fn 作用域顶端，fn 函数创建的时候，保存的是根据词法所生成的作用域链，fn 执行的时候，会复制这个作用域链，作为自己作用域链的初始化，然后根据环境生成变量对象，然后将这个变量对象，添加到这个复制的作用域链，这才完整的构建了自己的作用域链。至于为什么会有两个作用域链，是因为在函数创建的时候并不能确定最终的作用域的样子，为什么会采用复制的方式而不是直接修改呢？应该是因为函数会被调用很多次吧。
```js
fnContext = {
    AO: {
        arguments: {
            0: 20,
            length: 1
        },
        b: undefined
    },
    Scope: [AO, fn.[[scope]]]
}
```

#### this
再下一步就是确定 this 的指向，这步的 this 指向 window，因为 fn 是独立调用：

```
this === window
```

this 的指向是函数被调用的才确立的，分5种情况：
- 普通函数调用，在严格模式下指向 undefined，反之指向 window
- 作为对象函数调用，指向该对象
- 作为 call/apply/bind 函数调用，指向传入绑定的 this 值
- 函数被 new 操作符调用，指向的是新创建的实例对象
- 最后一个是箭头函数下的 this 指向包裹它外层的this


再下一步就是进入函数执行阶段，按顺序进行执行，对变量进行赋值等操作：
```js
fnContext = {
    AO: {
        arguments: {
            0: 20,
            length: 1
        },
        b: 30
    },
    Scope: [AO, fn.[[scope]]]
}
```

最后函数执行完毕，fn 函数上下文从上下文栈弹出：

```js
ECS = ['globalContext']
```

...等待垃圾回收。

## 闭包
再聊一聊闭包下的上下文的运行情况，写一个闭包的例子：
```js
function outer() {
  var b = 10

  return inner

  function inner() {
    console.log(b ++)
  }
}

var fn = outer()
fn() // 10
```

首先在函数执行之前，也就是 outer 定义时，保存词法作用域的作用域链：
```js
outer.[[scope]] = [
    globalContext.VO
]
```

然后进入函数上下文，开始初始化阶段：
- 初始化 arguments
- 处理 inner 函数声明
- 处理 b 变量声明

此时的 outerContext 的 AO 为：
```js
outerContext = {
    AO: {
        arguments: {
            length: 0
        },
        inner: <reference function inner>
    },
    Scope: outer.[[scope]]
}
```

然后下一步到了 outer 函数执行的阶段，且返回 inner 函数，保存到了 fn 函数：

```js
outerContext = {
    AO: {
        arguments: {
            length: 0
        },
        inner: <reference function inner>
    },
    Scope: [AO, outer.[[scope]], global.[[scope]]]
}
```

执行 fn 函数的初始化阶段：

```js
fnContext = {
    VO: {
        arguments: {
            length: 0
        }
    },
    Scope: [VO, inner.[[scope]], outer.[[scope]], global.[[scope]]]
}
```

执行 fn 函数上下文的阶段，查询的是 b 变量，在当前作用域下没有 b 变量，则用户作用域链去它上一层的作用域找到了，则输出 10。

值得注意的点：
- 闭包所访问的变量，是每次运行父函数都重新创建，互相独立的。

```js
// 接着上一步的代码
fn() // 10
fn() // 11

var fn2 = outer()
fn2() // 10
fn2() // 11
```

- 同一个函数中创建的自由变量是可以在不同的闭包共享的。
```js
function outer() {
	var a = 10
	function bar() {
		console.log(a ++)
	}
	function baz{
		console.log(a ++)
	}
	
	return {
		bar,
		baz
	}
}
var fn =  outer()
fn.bar() // 10
fn.baz() // 11
```

闭包的优点：
- 灵活方便
- 封装
- 模块化


闭包的缺点：
- 空间浪费
- 内存泄露
- 内存消耗
