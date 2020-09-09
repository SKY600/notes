### CSS3新增伪类
1. ```p:first-of-type``` 选择属于其父元素的首个 ```<p>``` 元素的每个 ```<p>``` 元素。
2. ```p:last-of-type```  选择属于其父元素的最后 ```<p>``` 元素的每个 ```<p>``` 元素。
3. ```p:only-of-type``` 选择属于其父元素唯一的 ```<p>``` 元素的每个 ```<p>``` 元素。
4. ```p:only-child```    选择属于其父元素的唯一子元素的每个 ```<p>``` 元素。
5. ```p:nth-child(2)```  选择属于其父元素的第二个子元素的每个 ```<p>``` 元素。
6. ```:enabled```  :disabled 控制表单控件的禁用状态。
7. ```:checked```  单选框或复选框被选中。


### CSS3新特性
1. CSS3实现圆角（```border-radius```），阴影（```box-shadow```），
2. 对文字加特效（```text-shadow```），线性渐变（```gradient```），旋转（```transform```）
3. ```transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg)```;//旋转,缩放,定位,倾斜
4. 增加了更多的CSS选择器  多背景 ```rgba```
5. 在CSS3中唯一引入的伪元素是```::selection```
6. 媒体查询，多栏布局
7. ```border-image```

### CSS3新增盒模型：box-sizing
盒模型默认的值是```content-box```, 新增的值是```padding-box``` 和 ```border-box```，几种盒模型计算元素宽高的区别如下：

1. content-box（默认）：
> 布局所占宽度 Width：Width = width + padding-left + padding-right + border-left + border-right<br>
> 布局所占高度 Height：Height = height + padding-top + padding-bottom + border-top + border-bottom

2. padding-box
> 布局所占宽度 Width：Width = width(包含padding-left + padding-right) + border-top + border-bottom<br>
> 布局所占高度 Height: Height = height(包含padding-top + padding-bottom) + border-top + border-bottom

3. border-box
> 布局所占宽度 Width：Width = width(包含padding-left + padding-right + border-left + border-right)<br>
> 布局所占高度 Height：Height = height(包含padding-top + padding-bottom + border-top + border-bottom)