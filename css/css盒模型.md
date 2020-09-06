### 标准模型
#### 由四部分组成：
* 内容区域: 可以放置元素的区域如文本,图像等，一般设置宽高指的是这个内容的宽高
* 内边距的区域：内容与边框之间的距离
* 边框区域: 边框
* 外边框区域：由外边框限制，用空白区域扩展边框区域，开分开相邻的元素


#### 模型区分：
* 标准表型指的是设置 box-sizing 为 content-box 的盒子模型，一般width,height指的是content的宽高。
* IE盒模型指的是 box-sizing 为 border-box 的盒子。宽高的计算是 content + padding + border;

#### box-sizing属性
box-sizing属性主要用来控制元素的盒模型的解析模式。默认值是content-box。

* content-box：让元素维持W3C的标准盒模型。元素的宽度/高度由border + padding + content的宽度/高度决定，设置width/height属性指的是content部分的宽/高
* border-box：让元素维持IE传统盒模型（IE6以下版本和IE6~7的怪异模式）。设置width/height属性指的是border + padding + content

标准浏览器下，按照W3C规范对盒模型解析，一旦修改了元素的边框或内距，就会影响元素的盒子尺寸，就不得不重新计算元素的盒子尺寸，从而影响整个页面的布局。