#### 过渡 transition
```
transition-property:width
transition-duration:1s
transition-timing-function:linear
transition-delay:2s
```
 
#### 动画 animation
animation:动画名称，一个周期花费时间,云顶曲线(默认ease),动画延迟(默认0),动画播放次数(默认1),是否反向播放动画(默认normal),是否暂停动画(默认running)

#### 形状转换 transform
* transform: 使用于2D或3D转换的元素
* transform-origin: 装换元素的位置(围绕哪个点进行装换).默认(x,y,z);

#### 选择器

#### 阴影 box-shadow
box-shadow: 水平阴影的位置 垂直阴影的位置 模糊距离 阴影的大小 阴影的颜色 阴影开始的方向(默认是从里向外，设置inset就是从外往里)

#### 边框图片 border-image
border-image: 设置图片路径 设置边框背景图的分割方式 设置或检索对象的边框厚度 设置或检索对象的边框背景图向外扩展 设置边框图片的平铺方式

#### 边框圆角 border-radius
border-radius: n1 n2 n3 n4;

n1-n4 四个值得顺序是左上角，右上角，右下角，左下角
 
#### 反射(倒影) box-reflect
box-reflect: 方向[above-上|below-下|right-右|left-左],偏移量,遮罩图片
 
#### 文字
#### 换行 word-break
word-break:normal(默认使用浏览器默认的换行规则)|break-all(允许在单词内换行)|keep-all(只能在半角空格或连字符处换行)

#### 超出省略号
```
overflow: hidden;
white-space: nowrap;
text-overflow:ellipsis;
```

#### 多行省略号
```
overflow:hiden;
text-overflow:ellipsis;用省略号"..."隐藏超出范围的文本
display:-webkit-box;  //将对象作为弹性伸缩盒子模型显示
-webkit-line-clamp:2; //用来限制在一个块元素显示的文本的行数
-webkit-box-orient:vertical;设置弹性盒对象的子元素的排列方式
```

#### 文字阴影 text-shadow
text-shadow: 水平阴影 垂直阴影 模糊阴影 阴影颜色

#### 颜色 rgba(rgb颜色值，a为透明度)
#### 渐变 线性渐变和径向渐变
#### filter(滤镜) filter: 滤镜效果(透明度)
#### 弹性布局 弹性布局，就是flex布局
#### 栅格布局 栅格化布局，就是grid布局
#### 盒模型
* border-box 边框和内边距包含在元素的宽高之内
* content-box 边框和内边距不包含在元素的宽高之内