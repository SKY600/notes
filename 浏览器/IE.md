### 几种IE6 BUG的解决方法
1. 双边距BUG float引起的 使用display
2. 3像素问题 使用float引起的 使用dislpay:inline -3px
3. 超链接hover 点击后失效 使用正确的书写顺序 link visited hover active
4. z-index问题 给父级添加position:relative
5. Png 透明 使用js代码 改
6. Min-height 最小高度 ！Important 解决’
7. select 在ie6下遮盖 使用iframe嵌套
8. 为什么没有办法定义1px左右的宽度容器（IE6默认的行高造成的，使用over:hidden,zoom:0.08 line-height:1px）

### ie各版本和chrome可以并行下载多少个资源？
IE6 两个并发，iE7升级之后的6个并发，之后版本也是6个；Firefox，chrome也是6个

### 事件、IE与火狐的事件机制有什么区别？ 如何阻止冒泡？
1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
2. 事件处理机制：IE是事件冒泡、firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件。
3. `ev.stopPropagation()`;
注意旧ie的方法 `ev.cancelBubble = true`;

### 列举IE 与其他浏览器不一样的特性？
1. IE支持currentStyle，FIrefox使用getComputStyle
2. IE 使用innerText，Firefox使用textContent
3. 滤镜方面：`IE:filter:alpha(opacity= num)；Firefox：-moz-opacity:num`
4. 事件方面：IE：attachEvent：火狐是addEventListener
5. 鼠标位置：IE是event.clientX；火狐是event.pageX
6. IE使用event.srcElement；Firefox使用event.target
7. IE中消除list的原点仅需margin:0即可达到最终效果；FIrefox需要设置margin:0;padding:0以及list-style:none
8. CSS圆角：ie7以下不支持圆角
