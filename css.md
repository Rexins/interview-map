## css 篇

### 1.css 盒模型

简介：每个元素都是个盒子（盒模型）。CSS 中盒子模型包括 IE 盒子模型以及 W3C 盒子模型，个人认为只是初始值不一样。

盒子的组成（由内到外）: content、padding、border、margin。

盒子的宽度：contentWidth + 左右padding + 左右border。
盒子的高度：contentHeight + 上下padding + 上下border。

box-sizing（改变 width 的计算规则）: border-box, padding-box, content-box。

### 2.画一条 0.5px 的线 或 修改 font-size 小于 12px 的字体
- 采用 meta viewport
- border-image
- scale

### 3.link 标签和 @import 的差异
- link 属于html标签，@import 是 css 提供。
- 页面被加载时，link 会同时被加载，而 @Import 会等到页面加载结束后加载。？
- link 没有兼容性问题，@Import 需要 IE5 以上才能识别。
- link 方式的样式权重大于 @import。？

### transition 和 animation 的差异
- 兼容性不同。
- transition 是从一个状态过度到另一个状态，需要触发一个状态然后运行。
- animation 可以一帧帧的，不需要触发任何状态。

### BFC

简介： 块级格式化上下文，是一个独立的渲染区域，有自身的一个计算规则。
作用： 用于清除浮动，防止margin叠加。
- BFC区域不会与float box重叠
- BFC是页面上的一个独立容器，子元素不会影响到外面
- 计算BFC的高度时，浮动元素也会参与计算

生成 BFC 因素：
- 根元素
- float 不为 none
- position 为 absolute 和 fixed
- display 为 inline-block、table-cell、table-caption、flex、inline-flex
- overflow 不为 visible

### 关于 JS 动画和 CSS3 动画差异

渲染线程分为main thread和compositor thread，如果css动画只改变transform和opacity，这时整个CSS动画得以在compositor trhead完成（而js动画则会在main thread执行，然后出发compositor thread进行下一步操作），特别注意的是如果改变transform和opacity是不会layout或者paint的。

区别：

- 功能涵盖面，js比css大
- 实现/重构难度不一，CSS3比js更加简单，性能跳优方向固定
- 对帧速表现不好的低版本浏览器，css3可以做到自然降级
- css动画有天然事件支持
- css3有兼容性问题

### visibility=hidden, opacity=0，display:none

- visibility 元素隐藏起来，不会导致页面重绘，点击区域事件不触发
- opcity 元素隐藏起来，不会导致页面重绘，点击区域事件触发
- display 元素隐藏起来，会导致页面重绘，无法点击该区域

### 双边距重叠问题（外边距折叠）

多个相邻（兄弟或者父子关系）普通流的块元素垂直方向marigin会重叠

折叠的结果为：

- 两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
- 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
- 两个外边距一正一负时，折叠结果是两者的相加的和。