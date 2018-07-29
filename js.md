# ECMAScript

**[Base](#base)**
- [内置类型](#baseType)
  - [typeof](#typeof)
  - [instanceof](#instanceof)
  - Object.prototype.toString
- 作用域、作用域链
  - 闭包
- [原型、原型链](#prototype)
  - this
  - new 
  - call、apply、bind
  - 继承

**[Advance](#advance)**
- Promise
- async
- generator
- Event Loop(web、Node.js)
- Symbol
- Set、Map
- Proxy、Reflect、defineProperty
- CommonJS、ES6、AMD、CMD

  
**[Question](#question)**
- GET 请求传参长度误区
- 模块化、组件化
- 节流、防抖
- JS 垃圾回收机制




<h3 id="base">Base</h3>

<h4 id="baseType">内置类型</h4>

JS 中分为七种内置类型，七种类型又可分为两大类：基本类型、引用类型（Object）。  
基本类型： null、undefined、boolean、number、string、sy
mbol。

tips：
- 在 jS 中数字类型是浮点类型，没有整型。浮点类型基于 IEEE 754 标准实现。使用中有 0.1 + 0.2 !== 0.3 的问题存在。


<h4 id="typeof">typeof</h4>

**typeof** 对于基本类型来说相对较为友好，除了 null 会返回 object。
```
// 基本类型
typeof null // 'object'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof 1 // 'number'
typeof '1' // 'string'
typeof Sumbol() // 'symbol'
// 引用类型
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

tips:
- 在低版本浏览器中，undefined 可以被赋值。所以用 void 0 来表示 undefined。

#### instanceof
  instanceof 可以正确的判断对象的类型，因为内部机制是能否在原型链中找到该对象类型的 prototype。
  ```
    
  ```
### instanceof

用于实例和构造函数的对应。  
```
[1,2] instanceof Array // true

function Foo (name) {
  this.name = name
}
var foo = new Foo('dong')
foo instanceof Foo

// instanceof 实现
function instanceof(left, right) {
  right = right.prototype
  left =  left.__proto__

  while(true) {
    if (left === null) {
      return false
    }
    if (left === right) {
      return true
    } else {
      left = left.__proto__
    }
  }
}
```

tips:
- 判断基本类型可用 typeof 。判断构造函数的的实例，可用 instanceof
- 如果我们想更加准确的获取y一个变量的正确类型。可以通过Object.prototype.toString.call(xx)。这样我们就可以获得类型[object ${type}]。


<h4 id="prototype">原型和原型链</h4>

  每个函数都有个 prototype 属性，除了 Function.prototype.bind(), prototype 该属性指向**原型**。
  每个引用对象，都有 __proto__ 属性，该属性指向了创建该对象构造函数的原型。其实这个属性指向了 [[prototype]]，但是 [[prototype]] 属性是内部属性，我们并不能访问到，所以使用 __proto__ 访问。
  对象可通过 __proto__ 访问不属于该对象的属性，__proto__ 将对象访问属性过程连接起来组成了**原型链**

  **tips：**关于 Function.prototype.bind() 函数没有 prototype 属性猜想，因为是个永远改变函数内部 this 的指向，破坏了结构的问题，所以为没有 prototype 属性，也可能是 js 设计遗漏。

  #### this
    - 创建了对象
    - 原型指向
    - 构造函数 this 指向，执行函数赋予初始值
    - 返回对象

  ```
    function create() {
      //获取参数
      let args = [].slice.call(arguments)
      
      // 创建对象
      let obj = {}
      // 获取构造函数
      let Con = args.shift()
      // 改变对象 __proto__ 指向
      obj.__proto__ = Con.prototype
      // 执行构造函数，修改 this 指向对象
      Con.apply(obj, args)
      // 返回 obj
      return obj
    }
  ```

## 作用域和闭包

## 闭包

>闭包的定义：函数 A 返回了一个函数 B，并且函数 B 中 使用了函数 A 的变量，函数 B 就被称为闭包。
>要理解闭包我们就需要从 [ 执行上下文 ] 开始

### 执行上下文

在一段 JS 脚本（即一个 script 标签中）执行之前，要先解析代码（所以说 JS 是解析执行的脚本语言，解析的时候会创建一个 **全局执行上下文** 环境，先把代码中即将执行的（内部函数不算，因为你不知道函数何时执行）变量、函数声明都拿出来。变量暂时赋值为 undefined，函数则先生命好可使用（声明提前）。这是代码执行前开始工作的内容。  

另外函数执行时，也会创建一个 **函数执行上下文** 环境,跟 **全局上下文** 差不多，不过 **函数执行上下文** 中会多出this arguments和函数的参数。参数和arguments好理解，这里的this咱们需要专门讲解。

- 范围：一段`<script>`、js 文件或者一个函数
- 全局上下文：变量定义，函数声明
- 块级上下文：变量定义 ？// 自身认知所加
- 函数上下文：变量定义，函数声明，this，arguments

### this

**this 的值是在执行的时候才能确认，定义的时候不能确认！**
---- 因为this是执行上下文环境的一部分，而执行上下文需要在代码执行之前确定，而不是定义的时候

**this**的执行主要分为以下几种
- 作为构造函数执行
- 作为对象属性执行
- 作为函数执行
- 用于 call、apply、bind

### 作用域

作用域就是个独立的地盘，让变量不会外泄、暴露出去。  
**全局作用域、块级作用域、函数作用域**

#### 作用域链
首先认识一下什么叫做 **自由变量** 。如下代码中，console.log(a)要得到a变量，但是在当前的作用域中没有定义a（可对比一下b）。当前作用域没有定义的变量，这成为 **自由变量** 。自由变量如何得到 —— 向父级作用域寻找。

如果父级也没呢？再一层一层向上寻找，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是 **作用域链** 。

ps： **作用域链** 依据的是函数定义时的，而不是执行时。

## call、apply、bind 实现
```
// call
Function.prototype.call = function(){
  const arg = [...arguments]
  const context = arg.shift()
  context.__fn__ = this
  const result = context.__fn__(...arg)
  delete context.__fn__
  return result
}
// apply
Function.prototype.apply = function(){
  let arg = [...arguments]
  const context = arg.shift()
  context.__fn__ = this
  arg = arg[0]
  let result = null
  if (arg) {
    result = context.__fn__(...arg)
  } else {
    result = context.__fn__()
  }
  delete context.__fn__
  return result
}
//bind
Function.prototype.bind = function(){
  let arg = [...arguments]
  const context = arg.shift()
  const self = this;
  return function(){
    arg.concat([...arguments])
    return self.apply(context, arg)
  }
}
```

## 浅拷贝 和 深拷贝
```
/* 浅拷贝 */

// Object.assign
Object.assign({}, copyObject)

// 展开运算符
{...copyObject}

// 迭代 栗子较多就不多说了

```

<h3 id="question">question</h3>