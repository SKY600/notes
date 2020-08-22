##### 你觉得jQuery源码有哪些写的好的地方?
1. jQuery源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当jQuery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找undefined时的作用域链。

```
    (function( window, undefined ) {
         //用一个函数域包起来，就是所谓的沙箱
         //在这里边var定义的变量，属于这个函数域内的局部变量，避免污染全局
         //把当前沙箱需要的外部变量通过函数参数引入进来
         //只要保证参数对内提供的接口的一致性，你还可以随意替换传进来的这个参数
        window.jQuery = window.$ = jQuery;
    })( window );
```
2. jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法。
3. 有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度。
4. jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率。

#### 使用jquery的原因?
jquery是轻量级的框架，大小不到30kb，它有强大的选择器。

#### jquery能做什么？
1. 获取页面的元素
2. 修改页面的外观
3. 改变页面大的内容
4. 响应用户的页面操作
5. 为页面添加动态效果
6. 无需刷新页面，即可以从服务器获取信息
7. 简化常见的javascript任务

#### Jquery选择器
基本选择器，内容选择器，可见性选择器，属性选择器，子元素选择器，表单选择器

#### jquery和css 选择器的区别
jQuery选择器支持CSS里的选择器，jQuery选择器可用来添加样式和添加相应的行为。CSS 中的选择器是只能添加相应的样式。

#### jquery中选择器的优势
简单的写法 $('ID') 来代替 document.getElementById()函数。支持CSS1 到CSS3 选择器完善的处理机制(就算写错了id也不会报错)。

#### 使用jquery选择器要注意的地方
* 选择器中含有".","#","[" 等特殊字符的时候需要进行转译
* 属性选择器的引号问题
* 选择器中含有空格的注意事项

#### jquery对象和dom对象的转换
jquery转DOM对象:jQuery 对象是一个数组对象，可以通过[index]的丰富得到相应的DOM对象，还可以通过get[index]去得到相应的DOM对象。DOM对象转jQuery对象:$(DOM对象)

#### jquery中$.get()提交和$.post()提交有区别吗？
本质就是问get和post请求的区别

#### jquery中如何操作样式
* addClass() 来追加样式 
* removeClass() 来删除样式
* toggle() 来切换样式

#### jquery中的动画怎样使用
* hide() 和 show() 同时修改多个样式属性。像高度，宽度，不透明度。 
* fadeIn() 和fadeOut() fadeTo() 只改变不透明度。
* slideUp() 和 slideDown() slideToggle() 只改变高度。
* animate() 属于自定义动画的方法。

#### jquery中引入css的方式
四种 行内式，内嵌式，导入式，链接式

#### jquery中插入节点的方法以及区别
1. append(),appendTo()
2. prepend(),prependTo()
3. after(),insertAfter()
4. before(),insertBefore() 
 
大致可以分为 内部追加 和 外部追加。
* append() 表式向每个元素内部追加内容。
* appendTo()表示 讲所有的元素追加到指定的元素中。
例$(A)appendTo(B) 是将A追加到B中下面的方法解释类似。

#### 包裹节点的方法 以及 好处
wrapAll(),wrap(), wrapInner() 需要在文档中插入额外的结构化标记的时候，可以使用这些包裹的方法，因为它不会帛画原始文档的语义。

#### jquery中如何来获取或和设置属性？
* attr()方法来获取和设置元素属性
* removeAttr() 方法来删除元素属性

14. 如何来设置和获取HTML 和文本的值？
答：html()方法 类似于innerHTML属性 可以用来读取或者设置某个元素中的HTML内容
注意：html() 可以用于xhtml文档 不能用于xml文档text() 类似于innerText属性 可以用来读取或设置某个元素中文本内容。val() 可以用来设置和获取元素的值

15. 你jquery中有哪些方法可以遍历节点？
答 ：children() 取得匹配元素的子元素集合,只考虑子元素不考虑后代元素 next() 取得匹配元素后面紧邻的同辈元素
prev() 取得匹配元素前面紧邻的同辈元素
siblings() 取得匹配元素前后的所有同辈元素
closest() 取得最近的匹配元素
find() 取得匹配元素中的元素集合 包括子代和后代
:first 查询第一个，:last 查询最后一个，:odd查询奇数但是索引从0开始:even 查询偶数，:eq(index)查询相等的 ,:gt(index)查询大于index的 ,:lt查询小于index:header 选取所有的标题等

16. 子元素选择器 和后代选择器元素有什么区别？
答:子代元素是找子节点下的所有元素,后代元素是找子节点或子节点的子节点中的元素
 17. 在jquery中可以替换节点吗？
答：可以 在jQuery中有两者替换节点的方式 replaceWith() 和 replaceAll()例如在<p title="hao are you">hao are you</p>替换成<strong>I am fine<strong>$('p').replaceWith('<strong>I am fine</strong>'); replaceAll 与replaceWith的用法前后调换一下即可。
	* 
DOM操作的封装
	* 
事件处理机制
	* 
ajax(它的ajax封装的非常的好，不需要考虑复杂浏览器的兼容性和XMLHttpRequest                         对象的创建和使用的问题。)
	* 
出色的浏览器的兼容性。 
	* 
而且支持链式操作，隐式迭。
	* 
行为层和结构层的分离，还支持丰富的插件，jquery的文档也非常的丰富。


1.  你觉得beforeSend方法有什么用？
答：发送请求前可以修改XMLHttpRequest对象的函数，在beforeSend中如果返回false 可以取消本次的Ajax请求。XMLHttpRequest对象是唯一的参数所以在这个方法里可以做验证2. siblings() 方法 和 $('prev~div')选择器是一样的嘛？
答: $('prev~div') 只能选择'#prev'元素后面的同辈<div>元素而siblings()方法与前后的文职无关，只要是同辈节点就都能匹配。20 你在ajax中使用过JSON吗，你是如何用的？
答:使用过，在$.getJSON() 方法的时候就是。
因为 $.getJSON() 就是用于加载JSON文件的21 nextAll() 能 替代$('prev~siblindgs')选择器吗？
答:能。 使用nextAll() 和使用$('prev~siblindgs') 是一样的22 jQuery中有几种方法可以来设置和获取样式
答 ：addClass() 方法，attr() 方法23 $(document).ready()方法和window.onload有什么区别？
答: 两个方法有相似的功能，但是在实行时机方面是有区别的。
1. window.onload方法要在所有的关联资源（包括图像、音频）加载完毕后才会调用。用户就会感受到定义在 window.onload 事件上的代码在执行时有明显的延迟。$(document).ready() 方法可以在DOM载入就绪时就对其进行操纵，并调用执行绑定的函数。适用于所有浏览器，jQuery帮你解决了跨浏览器的难题
2. 我们可以在页面中使用多个document.ready()，但只能使用一次onload()。24 jQuery是如何处理缓存的？
答 ：要处理缓存就是禁用缓存.
1 通过$.post() 方法来获取数据，那么默认就是禁用缓存的。
2 通过$.get()方法 来获取数据，可以通过设置时间戳来避免缓存。可以在URL后面加上+(+new Date)例 $.get('ajax.xml?'+(+new Date),function () { //内容 }); 3 通过$.ajax 方法来获取数据，只要设置cache:false即可。25 $.getScript()方法 和 $.getJson() 方法有什么区别？
答: 1 $.getScript() 方法可以直接加载.js文件，并且不需要对javascript文件进行处理，javascript文件会自动执行。
2 $.getJson() 是用于加载JSON 文件的 ，用法和$.getScript()26 radio单选组的第二个元素为当前选中值，该怎么去取？
答 : $('input[name=items]').get(1).checked = true;27 选择器中 id，class有什么区别？
答：在网页中 每个id名称只能用一次，class可以允许重复使用28 你使用过哪些数据格式，它们各有什么特点？
答: HTML格式 ,JSON格式,javascript格式,XML格式
    1 HTML片段提供外部数据一般来说是最简单的。
    2 如果数据需要重用，而且其他应用程序也可能一次受到影响，那么在性能和文件大小方面具有优势的JSON通常是不错的选择。
    3 而当远程应用程序未知时，XML则能够为良好的互操作性提供最可靠的保证。29 在ajax中data主要有几种方式？
答 ： 三种，html拼接的，json数组，form表单经serialize()序列化的。30 jQuery中的hover()和toggle()有什么区别？
答 hover()和toggle()都是jQuery中两个合成事件。
hover()方法用于模拟光标悬停事件。 toggle()方法是连续点击事件。31 你知道jQuery中的事件冒泡吗，它是怎么执行的，何如来停止冒泡事件？
答 : 知道,事件冒泡是从里面的往外面开始触发。在jQuery中提供了stopPropagation()方法可以停止冒泡。32 例如 单击超链接后会自动跳转，单击"提交"按钮后表单会提交等，有时候我想阻止这些默认的行为，该怎么办？
答: 可以用 event.preventDefault()或在事件处理函数中返回false，即 return false;33 jquery表单提交前有几种校验方法？分别为？？
a) formData:返回一个数组，可以通过循环调用来校验
b) jaForm：返回一个jQuery对象，所有需要先转换成dom对象
c) fieldValue：返回一个数组beforeSend()
34怎样用jQuery编码和解码URL encodeURIComponent(url)   decodeURIComponent(url)
35 如何用jQuery禁用浏览器的前进后退按钮？
Window.history.forward(1)
Window.history.forward(-1)
36 jQuery中的Delegate()函数有什么作用？
	* 
如果你有一个父元素，需要给其下的子元素添加事件，这时你可以使用delegate()了，代码如下
	* 
、当元素在当前页面中不可用时，可以使用delegate()

37.  jQuery 里的 each() 是什么函数？你是如何使用它的？
each() 函数就像是 Java 里的一个 Iterator，它允许你遍历一个元素集合。你可以传一个函数给 each() 方法，被调用的 jQuery 对象会在其每个元素上执行传入的函数。有时这个问题会紧接着上面一个问题，举个例子，如何在 alert 框里显示所有选中项。我们可以用上面的选择器代码找出所有选中项，然后我们在 alert 框中用 each() 方法来一个个打印它们，代码如下：



$('[name=NameOfSelectedTag] :selected').each(function(selected) {
    alert($(selected).text());
});

其中 text() 方法返回选项的文本。38 . $(this) 和 this 关键字在 jQuery 中有何不同？
这对于很多 jQuery 初学者来说是一个棘手的问题，其实是个简单的问题。$(this) 返回一个 jQuery 对象，你可以对它调用多个 jQuery 方法，比如用 text() 获取文本，用val() 获取值等等。而 this 代表当前元素，它是 JavaScript 关键词中的一个，表示上下文中的当前 DOM 元素。你不能对它调用 jQuery 方法，直到它被 $() 函数包裹，例如 $(this)。38. jQuery中 detach() 和 remove() 方法的区别是什么?
尽管 detach() 和 remove() 方法都被用来移除一个DOM元素, 两者之间的主要不同在于 detach() 会保持对过去被解除元素的跟踪, 因此它可以被取消解除, 而 remove() 方法则会保持过去被移除对象的引用. 你也还可以看看 用来向DOM中添加元素的 appe
	1. 
jQuery 中的方法链是什么？使用方法链有什么好处？


方法链是对一个方法返回的结果调用另一个方法，这使得代码简洁明了，同时由于只对 DOM 进行了一轮查找，性能方面更加出色。40.  event.preventDefault() 和 event.stopPropagation()
event.stopPropagation() 方法在事件传播链中阻止事件冒泡，而 event.preventDefault() 只是在事件发生时阻断浏览器的默认响应，但事件仍然会向上传递。41. 返回 false
在下面三种情况下返回 false 会出现什么样的结果？（a）jQuery 事件处理， （b） 超链接标签的原生 Javascript onclick 事件处理，（c）非超链接标签（例如： div，button等）的原生 Javascript onclick 事件处理。
答案：（a）在 jQuery 事件处理中返回 false 相当于 jQuery 的 event 对象连续调用了 preventDefault() 和 stopPropagation() 方法。
（b） 在超链接标签的原生 Javascript onclick 事件处理中返回 false 会阻止浏览器默认的地址导航，并且阻止了DOM事件的冒泡传递。
（c） 在非超链接标签（例如： div，button等）的原生 Javascript onclick 事件处理中返回 false 不会起任何作用。42. clone() 方法
jQuery 提供了一个非常有用的 .clone() 方法用于对指定元素进行深拷贝。
回答下面的问题
1. 这里的“深拷贝”是什么意思?
答案： .clone() 方法为一组元素执行深拷贝，意味着拷贝这些元素的同时，对其所有子元素和 text 节点也进行拷贝。
2. 深拷贝有哪些例外情况？如何去控制这些情况？
答案： 一般来说：
对象和数组中的元素数据不会被拷贝，克隆元素和原始元素共享同一份数据。要深拷贝所有数据，你需要“手动”操作每一个元素。任何绑定原始元素的事件处理也不会被拷贝到克隆元素上。可以将 withDataAndEvents 参数设置为 true 来强制所有的事件处理都会被拷贝到新元素上。
在 jQuery 1.4 中，所有元素所包含的数据（通过.data()方法绑定）同样会拷贝到新元素上。
到了 jQuery 1.5， withDataAndEvents 连同增强的 deepWithDataAndEvents 参数可以为所有子元素拷贝事件及数据。
3. 使用 clone()方法有什么潜在的陷阱？（提示：元素有什么属性，是你不愿克隆的？）
答案：使用 .clone() 方法会有一个潜在的问题，就是产生了一堆 id 重复的元素，但 id 应该是唯一的。所以，如果克隆的元素含有 id 属性，务必要记住在 DOM插入这个克隆元素前修改其 id 。
 
 
Jquery api
DOM操作：
	1. 
clone() 深度克隆，和append()搭配使用，避免克隆带有id属性的元素，克隆一组数据时不保证其顺序
	2. 
.wrap() 给指定的元素包裹一层结构，可以传递DOM结构，可以传递函数$(this)指向元素本身
	3. 
.wrapAll() 给集合中所有匹配元素包裹一层结构
	4. 
.wrapInner() 给指定的内容包裹一层结构，可以传递DOM结构，可以传递函数$(this)指向元素本身
	5. 
.append() 在每一个匹配的元素里面的末尾处插入参数内容，选择器表达式在函数的前面，参数是要加入的内容，支持DOM元素，Jquery对象，HTML字符串，DOM元素数组


$('.inner').append('<p>Test</p>');
	1. 
.appendTo() 在每一个匹配的元素里面的末尾处插入参数内容，内容在方法前面


$('<p>Test</p>').appendTo('.inner');
	1. 
.html() 来获取任意一个元素的内容
   .html() 来设置元素的内容，这些元素中的任何内容会完全被新的内容取代
	2. 
.prepend() 在每一个匹配的元素里面的前面处插入参数内容，选择器表达式在函数的前面，参数是要加入的内容，支持DOM元素，Jquery对象，HTML字符串，DOM元素数组
	3. 
.prependTo() 在每一个匹配的元素里面的前面处插入参数内容，内容在方法前面
	4. 
.text() 无参数：返回所选参数的文本，包含它的后代文本


         .text(x): 替换所选参数的文本
         .text(function() {})
.text() 可以用在xml和html文当中，不能用在input上以及script元素上
.html()不可以用在xml文档中
.val() 用在input
	1. 
.after() 匹配元素集合中的每一个元素后面插入参数指定的内容，作为兄弟节点


.insertAfter()  匹配元素集合中的每一个元素后面插入参数指定的内容，作为兄弟节点
区别：内容和目标的位置不同
$(‘inner’).after(‘<div>aaaaaaaaaa</div>’)
$(‘<div>aaaaaaaaaa</div>’).insertAfter(‘inner’)
	1. 
.before() 匹配元素集合中的每一个元素前面插入参数指定的内容，作为兄弟节点


.insertBefore()  匹配元素集合中的每一个元素前面插入参数指定的内容，作为兄弟节点
区别：内容和目标的位置不同
	1. 
.detach() 从DOM中去掉匹配的所有元素


.remove()从DOM中去掉匹配的所有元素
区别： .detach()保存了所有Jquery数据和被移走的元素相关联
	1. 
.empty() 从DOM中移除集合中所匹配的所有的子节点，无参数，文本也被移除


.remove()  从DOM中移除集合中所匹配的所有的子节点，包含自身，无参数，文本也被移除，同时移除元素上的事件以及JQuery数据
	1. 
.unwrap() 把匹配元素集合中的元素的父级删掉，其本身以及兄弟元素都还在原位上


属性操作
	1. 
.attr()  获得第一个匹配元素的属性值或者设置属性值


.prop() 获得第一个匹配元素的属性值或者设置属性值
区别：attr()只返回attribute prop返回property
	1. 
removeAttr() 为匹配的元素集合中的每一个元素中移除一个属性


removeProp() 为匹配的元素集合中的每一个元素中移除一个属性，删除内联的onclick事件处理使用removeProp
	1. 
.val() 获得表单的值，input select textarea 空集合调用返回undefined 选项未被选中返回空数组，radio checkbox select利用check属性进行获取值，input option的value值与所传数组值相同时被选中


CSS操作
	1. 
.addClass(className) 为每一个匹配的元素添加指定的样式类名
	2. 
.css() 获得匹配元素集合中第一个元素的样式属性的计算值以及设置每一个匹配元素的一个或者多个css属性
	3. 
.hasClass() 确定任何一个匹配的元素是否具有被分配给定的样式
	4. 
.removeClass() 移除集合上每一个匹配的一个或者多个样式
	5. 
.toggleClass()  根据state状态添加或者删除一个或者多个样式类  state===true 添加
	6. 
.height() 第一个元素计算高度，返回容器的高度，不管box-sizing的值


.height(value) 设置值，根据box-sizing来定的，为border-box会变成修改outerHeight值
  区别css(‘height’) 返回一个400px，.heiht()返回400
	1. 
.innerHeight() 内部高度 height + padding 不适用window和document对象
	2. 
innerWidth()
	3. 
Width()
	4. 
.outerHeight() height + padding + margin
	5. 
.outerWidth()
	6. 
.offset() 第一个元素的当前坐标，相对于文档，返回top left对象


  .offset({top: 10,left:20})设置坐标，相对于文档
	1. 
offsetParent() 取得最近的被定位过的祖先元素
	2. 
.position() 当前元素（包含margin）相对于被定位过的父级元素的位移，不包括magin+border
	3. 
.scrollLeft()取得或者设置当前水平滚动条的位置
	4. 
.scrollTop()


数据操作
	1. 
.data(key ,value) 在匹配元素上存储任意相关数据


  .data()获得元素的值
 为了避免内存泄漏先设置之后在获取
	1. 
.JQuery.hasData()确定任意一个元素都有与之相关的jquery数据
	2. 
.removeData()


事件
	1. 
.resize()根据浏览器窗口改变出发的效果，移除使用.off(“resize”)
	2. 
.srcoll() 用户在元素内执行了滚动操作，出发scroll事件，适用于window对象
	3. 
.holdReady()延迟ready事件
	4. 
.bind()绑定事件之前Dom必须已经被加载完成。已经存在，用.on()替代
	5. 
.unbind()移除事件监听用off()替代
	6. 
.trigger()自动触发绑定在元素上所有的事件以及自定义事件，还可以以数组的传递参数，事件冒泡
	7. 
.triggerHandler() 不会触发事件的默认行为，不冒泡
	8. 
表单事件


 .submit() .blur() .change() .select() .focus()
	1. 
键盘事件


 .keydown() .keypress() .keyup()
	1. 
鼠标事件


.click()  .dblclick() .focusin .focusout .contextmenu() . hover()
.mousedown() .mouseover() .mouseenter() .mouseleave() .mosemove() .mouseout() .mouseup()
事件对象
	1. 
event.currentTarget === this   事件冒泡过程中的当前的DOM元素function(event)


e
