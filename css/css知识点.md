### css引入方式：link 和 import
下面介绍一下如何使用link和import引用外部CSS。
#### 1. link 标签引用外部CSS
在网页的```<head></head>```标签对中使用```<link>```标记来引入外部样式表文件。<br>
link语法结构: 
```
<link href="CSSurl路径" rel="stylesheet" type="text/css" />
```
此标签是引入CSS文件link标签，只要设置好路径即可。

#### 2. @import 规则引用外部CSS
CSS ```@import```规则，用于从其他样式表导入样式规则。这些规则必须先于所有其他类型的规则，```@import```不能在条件组的规则中使用。<br>
```@import```语法结构：
```
@import + 空格+ url(CSS文件路径地址);
```
1. 在html中
```
<style type="text/css">
    @import url(CSS文件路径地址);
</style>
```
2. 在css中，直接使用
```
@import url(CSS文件路径地址);
```

### link 和 @import 的区别
1. link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；而@import属于CSS范畴，只能加载CSS。
2. link引用CSS时，在页面载入时同时加载；而@import需要页面网页完全载入以后加载。
3. link是XHTML标签，无兼容问题；而@import是在CSS2.1提出的，低版本的浏览器不支持。
4. link支持使用Javascript控制DOM去改变样式；而@import不支持。
5. @import可以在CSS文件中再次引入其他样式表；而link不支持。

### px,em,rem,%,vw,vh,vm的区别
1. px
> px就是像素，相对于屏幕分辨率，比如常常听到的电脑像素是1024x768的，表示的是水平方向是1024个像素点，垂直方向是768个像素点。

2. em
> em参考物是父元素的font-size，默认字体大小是16px，所以1em不是固定值，因为它会继承父元素的字体大小，父元素16px,1.5em=24px。

3. rem（css3新增）
> rem参考物是相对于根元素，我们在使用时可以在根元素设置一个参考值即可，相对于em使用，减少很大运算工作量，例：html大小为10px，12rem就是120px

4. %
> % 是相对于父元素的大小设定的比率，position:absolute;的元素是相对于已经定位的父元素，position：fixed；的元素是相对可视窗口

5. vw
> vw相对于视口的宽度。视口被均分为100单位，1vw等于视口宽度的1/100
```
h1 {
        font-size: 8vw;
}
```
再举个例子：浏览器宽度1200px, 1 vw = 1200px/100 = 12 px

6. vh
> vh相对于视口的高度。视口被均分为100单位，1vh等于视口高度的1/100
```
h1 {
        font-size: 8vh;
}
```
再举个例子：浏览器高度900px, 1 vh = 900px/100 = 9 px

7. vm
> css3新单位，相对于视口的宽度或高度中较小的那个，其中最小的那个被均分为100单位的vm

   比如：浏览器高度900px，宽度1200px，取最小的浏览器高度，1 vm = 900px/100 = 9 px

   缺点：兼容性差

总结：
1. vw：1vw等于视口宽度的1%
2. vh：1vh等于视口高度的1%
3. vmin和vmax是相对于视口的高度和宽度两者之间的最小值或最大值

### margin和padding分别适合什么场景使用？
margin是用来隔开元素与元素的间距；padding是用来隔开元素与内容的间隔。

margin是用来布局分开元素，使元素与元素互不相干；padding用于元素与内容之间的间隔，让内容与元素之间有一段间距。

### 外边距合并（上下margin重合）
外边距合并指的是，当两个垂直外边距相遇时，他们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中较大者

ie和ff都存在，相邻的两个div的 margin-left 和 margin-right 不会重合，但是margin-top和margin-bottom 却会发生重合。
#### 防止外边距合并
养成良好的代码编写习惯，同时采用margin-top或者同时采用margin-bottom。

### display 和 visibility 的区别
display:none  隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢，就当他从来不存在。

visibility:hidden  隐藏对应的元素，但是在文档布局中仍保留原来的空间。

### 清除浮动的方式
清浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使页面后面的布局不能正常显示。

浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。

浮动的框可以向左或向右移动，直到他的外边缘碰到包含框或另一个浮动框的边框为止。由于浮动框不在文档的普通流中，所以文档的普通流的块框表现得就像浮动框不存在一样。浮动的块框会漂浮在文档普通流的块框上。

浮动元素引起的问题：
1. 父元素的高度无法被撑开，影响与父元素同级的元素
2. 与浮动元素同级的非浮动元素（内联元素）会跟随其后
3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构

清除浮动方式：
1. 父级div定义height
2. 额外标签法：在浮动元素后面添加class为clear的空div元素，给这个div设置样式.clear{clear:both}
  如：```<div style="clear:both;"></div>```
  （缺点：不过这个办法会增加额外的标签使HTML结构看起来不够简洁。）
3. 给父容器添加overflow:hidden 或者 auto 样式（使用zoom:1用于兼容IE）
4. 给父容器添加clearfix的class，用伪类clearfix:after；这个样式来清除浮动
```
.clearfix{
    zoom:1;
}
.clear:after{
    content:'.';
    height:0;
    clear:both;
    display:block;
    visibility:hidden;
}
```

5. 使用after伪类
```
    #parent:after{
        content:".";
        height:0;
        visibility:hidden;
        display:block;
        clear:both;
        }
```

### ```<strong>```，```<em>```和```<b>```，```<i>```标签
1. ```<strong>``` 标签和 ```<em>``` 标签一样，用于强调文本，但它强调的程度更强一些。
em 是 斜体强调标签，更强烈强调，表示内容的强调点。相当于html元素中的 ```<i>...</i>```;
2. ```<b>``` 和 ```<i>``` 是视觉要素，分别表示无意义的加粗，无意义的斜体。
3. ```<em>``` 和 ```<strong>``` 是表达要素(phrase elements)。

### position

#### fixed 固定定位
脱离文档流，相对于浏览器窗口进行定位

#### absolute 绝对定位
脱离文档流，相对于最近一级的有定位设置的父元素进行定位，如果父级元素没有设置定位属性，absolute元素将以body坐标原点进行定位

#### relative 相对定位
不脱离文档流，相对于其正常位置进行定位

#### static  静态    
不脱离文档流，默认值。没有定位，元素出现在正常出现的流中

#### inherit  继承   
规定从父元素继承position属性的值

#### absolute 和 float 的异同
* 共同点：对内联元素设置float和absolute属性，可以让元素脱离文档流，并且可以设置其宽高。
* 不同点：float仍会占据位置，absolute会覆盖文档流中的其他元素。

### display
#### block 块类型
默认宽度为父元素宽度，可设置宽高，换行显示

#### none 不显示
元素不显示，并从文档流中移除

#### inline 行内元素
默认宽度为内容宽度，不可设置宽高，同行显示

#### inline-block 行内块级
默认宽度为内容宽度，可以设置宽高，同行显示

#### list-item  
像块类型元素一样，可以设置宽高，同行显示

#### table 
此元素会作为会计表格来显示

#### inherit         
规定应该从父元素继承display属性的值
