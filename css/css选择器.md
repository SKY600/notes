#### 选择器
1. id选择器(#myid)
2. 类选择器(.myclassName)
3. 标签选择器(div,h1,p)
4. 相邻选择器（h1 + p）
5. 子代选择器(ul>li)
6. 后代选择器(li a)
7. 通配符选择器(*)
8. 属性选择器(a[rel="external"])
9. 伪类选择器(a:hover,li:nth-child)

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