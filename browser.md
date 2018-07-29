Browser
------------

- 事件
  - [事件触发三阶段](#事件触发三阶段)
  - [注册事件](#注册事件)
  - [事件代理](#事件代理)
- 存储
  - cookie、localStorage、sessionStorage、indexDB
  - Service Worker

## 事件触发三阶段
事件触发有三个阶段。  
- 捕获阶段  
  - document 往事件触发处传播，遇到注册的捕获事件会触发。 
- 目标阶段
  - 触发注册事件。
- 冒泡阶段
  - 从事件触发处往 document 传播，遇到注册的冒泡事件会触发。

tips:  
  
  - 基于事件冒泡，就有了事件委托。通过判断事件发生元素DOM的类型，来做出不同的响应。

<!-- <h4 id="registerEvent">注册事件</h4> -->
## 注册事件
EventTarget.addEventListener 的第三个参数来决定该函数是注册在捕获阶段还是冒泡冒泡阶段。  
第三个参数 可以为 Boolean(useCapture) 或 Object，默认为 false 表示注册冒泡事件。对于对象参数来说，可以为一下几个属性

- capture: Boolean 决定了注册的事件是捕获事件还是冒泡事件。
- once: Boolean 表示只调用一次，调用后会移除监听。
- passive: Boolean 表示永远不会调用 preventDefault

Event.stopPropagation 阻止了事件的传播，传播过的元素注册事件依旧会执行。  
Event.stopImmediatePropagation 解决了上个问题。

```
node.addEventListener('click',(event) =>{
	event.stopImmediatePropagation()
	console.log('冒泡')
},false);
// 点击 node 只会执行上面的函数，该函数不会执行
node.addEventListener('click',(event) => {
	console.log('捕获 ')
},true)
```

## 事件代理
如果节点的子节点是动态生成的或有大量的子节点，子节点做的是同一件事。那么子节点的注册事件应该注册在父节点上

```
<ul id="ul">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
</ul>
<script>
  let ul = document.querySelector('#ul')
  ul.addEventListener('click', (event) => {
    console.log(event.target);
  })
</script>
```

事件代理给直接在目标元素绑定，有以下有点
- 动态节点绑定事件
- 节省内存
- 不需要给子节点注销事件

## cookie、localStorage、sessionStorage、indexDB

|    特性     |                 cookie                  |      localStorage      | sessionStorage |       indexDB       |
| :---------: | :-------------------------------------: | :--------------------: | :------------: | :-----------------: |
| 数据生命周期 |     一般为服务器生成，可以设置过期事件     | 除非被清理，否则一直存在 |  页面关闭，清除 | 除非被清除否则一直存在 |
| 数据存储大小 |                   4k                    |           5M           |      5M        |         无限         |
| 与服务器通信 | 每次都会携带在 header 中，对于请求性能影响 |         不参与         |     不参与      |        不参与         |

对于 `cookie`，我们还需要注意安全性。

|   属性    |                             作用                             |
| :-------: | :----------------------------------------------------------: |
|   value   | 如果用于保存用户登录态，应该将该值加密，不能使用明文的用户标识 |
| http-only |            不能通过 JS 访问 Cookie，减少 XSS 攻击            |
|  secure   |               只能在协议为 HTTPS 的请求中携带                |
| same-site |    规定浏览器不能在跨域请求中携带 Cookie，减少 CSRF 攻击     |