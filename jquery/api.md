#### DOM操作：
1. .clone() 深度克隆，和append()搭配使用，避免克隆带有id属性的元素，克隆一组数据时不保证其顺序
2. .wrap() 给指定的元素包裹一层结构，可以传递DOM结构，可以传递函数$(this)指向元素本身
3. .wrapAll() 给集合中所有匹配元素包裹一层结构
4. .wrapInner() 给指定的内容包裹一层结构，可以传递DOM结构，可以传递函数$(this)指向元素本身
5. .append() 在每一个匹配的元素里面的末尾处插入参数内容，选择器表达式在函数的前面，参数是要加入的内容，支持DOM元素，Jquery对象，HTML字符串，DOM元素数组。
```$('.inner').append('<p>Test</p>');```
6. .appendTo() 在每一个匹配的元素里面的末尾处插入参数内容，内容在方法前面。
```$('<p>Test</p>').appendTo('.inner');```
7. .html() 来获取任意一个元素的内容。.html() 来设置元素的内容，这些元素中的任何内容会完全被新的内容取代。
8. .prepend() 在每一个匹配的元素里面的前面处插入参数内容，选择器表达式在函数的前面，参数是要加入的内容，支持DOM元素，Jquery对象，HTML字符串，DOM元素数组
9. .prependTo() 在每一个匹配的元素里面的前面处插入参数内容，内容在方法前面
10. .text() 无参数：返回所选参数的文本，包含它的后代文本。
    ```
    .text(x): 替换所选参数的文本
    .text(function() {})
    .text() 可以用在xml和html文当中，不能用在input上以及script元素上
    .html()不可以用在xml文档中
    ```
11. val() 用在input
12. .after() 匹配元素集合中的每一个元素后面插入参数指定的内容，作为兄弟节点
13. .insertAfter()  匹配元素集合中的每一个元素后面插入参数指定的内容，作为兄弟节点
```
区别：内容和目标的位置不同
$(‘inner’).after(‘<div>aaaaaaaaaa</div>’)
$(‘<div>aaaaaaaaaa</div>’).insertAfter(‘inner’)
```
14. .before() 匹配元素集合中的每一个元素前面插入参数指定的内容，作为兄弟节点
15. .insertBefore()  匹配元素集合中的每一个元素前面插入参数指定的内容，作为兄弟节点。
> 区别：内容和目标的位置不同
16. .detach() 从DOM中去掉匹配的所有元素
17. remove()从DOM中去掉匹配的所有元素
> 区别： .detach()保存了所有Jquery数据和被移走的元素相关联
18. .empty() 从DOM中移除集合中所匹配的所有的子节点，无参数，文本也被移除
19. .remove()  从DOM中移除集合中所匹配的所有的子节点，包含自身，无参数，文本也被移除，同时移除元素上的事件以及JQuery数据
20. .unwrap() 把匹配元素集合中的元素的父级删掉，其本身以及兄弟元素都还在原位上


#### 属性操作
1. .attr()  获得第一个匹配元素的属性值或者设置属性值
2. .prop() 获得第一个匹配元素的属性值或者设置属性值
> 区别：attr()只返回attribute prop返回property
3. removeAttr() 为匹配的元素集合中的每一个元素中移除一个属性
4. removeProp() 为匹配的元素集合中的每一个元素中移除一个属性，删除内联的onclick事件处理使用removeProp
5. .val() 获得表单的值，input select textarea 空集合调用返回undefined 选项未被选中返回空数组，radio checkbox select利用check属性进行获取值，input option的value值与所传数组值相同时被选中


#### CSS操作
1. .addClass(className) 为每一个匹配的元素添加指定的样式类名
2. .css() 获得匹配元素集合中第一个元素的样式属性的计算值以及设置每一个匹配元素的一个或者多个css属性
3. .hasClass() 确定任何一个匹配的元素是否具有被分配给定的样式
4. .removeClass() 移除集合上每一个匹配的一个或者多个样式
5. .toggleClass()  根据state状态添加或者删除一个或者多个样式类  state===true 添加
6. .height() 第一个元素计算高度，返回容器的高度，不管box-sizing的值
> .height(value) 设置值，根据box-sizing来定的，为border-box会变成修改outerHeight值。
区别：css(‘height’) 返回一个400px，.heiht()返回400。
7. .innerHeight() 内部高度 height + padding 不适用window和document对象
8. .innerWidth()
9. .Width()
10. .outerHeight() height + padding + margin
11. .outerWidth()
12. .offset() 第一个元素的当前坐标，相对于文档，返回top left对象
> .offset({top: 10,left:20})设置坐标，相对于文档
13. offsetParent() 取得最近的被定位过的祖先元素
14. .position() 当前元素（包含margin）相对于被定位过的父级元素的位移，不包括magin+border
15. .scrollLeft()取得或者设置当前水平滚动条的位置
16. .scrollTop()

#### 数据操作
1. .data(key ,value) 在匹配元素上存储任意相关数据
> .data()获得元素的值，为了避免内存泄漏先设置之后在获取
2. .JQuery.hasData()确定任意一个元素都有与之相关的jquery数据
3. .removeData()

#### 事件
1. .resize()根据浏览器窗口改变出发的效果，移除使用.off(“resize”)
2. .srcoll() 用户在元素内执行了滚动操作，出发scroll事件，适用于window对象
3. .holdReady()延迟ready事件
4. .bind()绑定事件之前Dom必须已经被加载完成。已经存在，用.on()替代
5. .unbind()移除事件监听用off()替代
6. .trigger()自动触发绑定在元素上所有的事件以及自定义事件，还可以以数组的传递参数，事件冒泡
7. .triggerHandler() 不会触发事件的默认行为，不冒泡
8. 表单事件 .submit() .blur() .change() .select() .focus()
9. 键盘事件 .keydown() .keypress() .keyup()
10. 鼠标事件 .click()  .dblclick() .focusin .focusout .contextmenu() . hover()
.mousedown() .mouseover() .mouseenter() .mouseleave() .mosemove() .mouseout() .mouseup()
11. 事件对象
	1. event.currentTarget === this   事件冒泡过程中的当前的DOM元素function(event)
    2. e
