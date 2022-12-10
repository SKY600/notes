### css 选择器
假设分为A、B、C 三类

##### A类 权重100
* id选择器(#myid)

##### B类 权重10
* 选择器(.myclassName)
* 属性选择器(a[rel="external"])
* 伪类选择器(a:hover,li:nth-child)

##### C类 权重1
* 标签选择器(div,h1,p)
* 伪元素选择器 (::before)

##### 其它
* 相邻选择器（h1 + p）
* 子代选择器(ul>li)
* 后代选择器(li a)
* 通配符选择器(*)


> 选择器之前可以使用逗号『,』分组，也可以使用空格，加号『+』，右尖括号『>』，波浪号『~』进行组合。


#### 优先级为:
* 同权重： 内联样式(标签内部)> 嵌入样式表(当前文件中)>外部样式(外部文件中)
* !important > id > class > tag
* !important 比内联优先级高，但内联比 id 要高

1. 优先级就近原则，同权重情况下样式定义最近这位准
2. 载入样式以最后载入的定位为准

#### 可继承的样式： 
* font-size 
* font-family 
* color

#### 不可继承的样式： 
* border 
* padding 
* margin 
* height 
* width