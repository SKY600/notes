### CSS3 字体特效
1. 火焰字：text-shadow:水平偏移量   垂直偏移量  阴影模糊度   阴影颜色;
2. 边框字：-webkit-/-ms-/-moz-/-o-<br>
``` -webkit-text-stroke:1px red; ```<br>
```-webkit-text-fill-color: transparent``` // 镂空字,transparent使得中间填充颜色为透明
3. 渐变字：外部字体处理--font-face
```
@font-face{
    font-family:"myfont";
    src:url("fonts/YGYXSZITI2.0.TTF"),url("");/*字体的存放类型*/ 
}
```

字体类型：1.3.5.要背会：
1. TureType(.ttf)格式 window,和mac
2. OpenType(.otf)格式
3. web open font fotmat(.woff)是web字体中的最佳格式
4. Embedded Open Type(.eot):ie专用字体
5. SVG（.svg）格式  主流画图标  

背景裁切：```-webkit-background-clip: text;```

```@font-face```  制作  web  Icon
```
//Guifx
@font-face {
    font-family: "myicon_guifx";//*图标库*//
    src: url(web_icon/Guifx/guifx_v2_transports-webfont.eot),
    url(web_icon/Guifx/guifx_v2_transports-webfont.svg),
    url(web_icon/Guifx/guifx_v2_transports-webfont.ttf),
    url(web_icon/Guifx/guifx_v2_transports-webfont.woff);
    //*font-weight*//
    //*font-style*//
}
.d1_icon{
    font-family: myicon_guifx;
}
```

### 渐变
1. 线性渐变 color-stop(过渡颜色位置，过渡颜色，起始点与终止点：left top,left bottom
30 40,60 90
20% 30%,40% 50%

2. 径向渐变 background:-webkit-gradient
* 80 80,50 内圆坐标  内圆半径
* 90 90,100 外圆坐标  外圆半径

```
background:-webkit-gradient(radial,40 40,20,90 90,100,
    from(yellow),
    to(green),
    color-stop(0.2,orange));
```

3. 重复渐变
-webkit-repeating-linear-gradient(left,起始渐变颜色 默认0开始,过渡渐变 宽度,过渡渐变 宽度,终止颜色 整体渐变宽度)

最后一个数：渐变宽

### CSS3颜色
opacity: 0.5; 对子元素产生影响
1. RGBA(23,34,45,0.8)    A:透明度  0-1  透明度不影响子元素
2. HSL色彩模式---（工业界颜色标准）
* H：hue :色调 360 --  -360
    60：黄  120：绿   180：青   360：洋红
* S：staturation :饱和度   0%---100%   表示该颜色被用了多少
	0%：灰度,表示没用  100%  饱和度最高，颜色最鲜艳
* L：lightness :亮度   0%--100%   
	0%: 黑色   50%：均色  100%：白色，最亮
3. HSLA（120,50%，50%，0.5）

### CSS3 边框
* -moz-border-radius: 100px;火狐早期
* border-top-left-radius: 100px;
* border-radius: 130px;
* border-radius: 100px 200px;  100px:左上及右下 200px:右上及左下
* border-radius: 100px 200px 250px 300px;从左上顺时针
* border-radius: 0 150px 150px 150px/0 150px 150px 150px;水平半径，垂直半径 

#### CSS3边框背景
1. border-radius 的值 <= border:20px 厚度时，内部边框不具有圆角效果
2. border-radius 的值 > border:20px 厚度时，内部边框有圆角效果

边框颜色 前面的颜色各占1px，最后一个占所有
```
-moz-border-left-colors: yellow blue green; // 谷歌不支持，火狐支持!

border-image: url(images/1.png) 20 30 40 50/20px repeat stretch; // 同下
border-image: url("images/1.png");

// 切割引入的图片  无单位
border-image-slice: 20 20 20 20;  // 上 右 下 左

// 边框宽度
border-image-width: 40px 50px 60px 70px; // 上右下左

//图片排列方式
border-image-repeat: 水平方向 垂直方向;
repeat ：重复
stretch ：拉伸
round : 平铺
border-image-repeat:
    如果省略该参数，默认 strstch
    如果只取一个值，水平和垂直相同

border-image-repeat: repeat stretch;
```

### 盒子阴影   
* 默认--外阴影
    * box-shadow:水平偏移量 垂直偏移量 模糊度 阴影扩展半径 阴影颜色;
    * box-shadow:10px 40px 30px 30px gray;
    * inset-  内阴影
    * box-shadow: inset 10px 10px 10px 10px gray;

* 多重阴影
    * box-shadow: 0 0 50px yellow, inset 0 0 15px blue;

css3  变形 旋转  ```transform: rotate(-5deg);```

### CSS3 背景
1. background-size
* ```background-size: 200px 200px;``` // 背景图片宽度，高度
* ```background-size: 80% 100%;``` // 装载背景图片的元素百分比计算  宽度：200px * 80% =160px 高度：200 * 100% =200 px 
* ```background-size: 80% 80px;```
* ```background-repeat: no-repeat;```
* ```background-size: cover;``` // 将背景图片放大到适合容器的尺寸
* ```background-size: contain;``` // 缩小到适合容器的尺寸，不改变图片的比例

2. background-clip // 背景裁剪--控制元素显示区域
* ```background-clip: border-box;``` // 默认，超出border部分的将被截掉
* ```background-clip: padding-box;``` // 超出padding部分的将被截掉
* ```background-clip: content-box;``` // 超出content部分的将被截掉
* ```background-clip: text;```

3. background-origin  // 设置背景起始点，决定了background-position的起始位置
* ```background-origin: border-box;``` // 从border的外边缘开始显示图片，决定了background-position的起始位置
* ```background-origin: padding-box;``` // 默认，从 border的内边缘，padding的外边缘开始显示图片，决定了background-position的起始位置
* ```background-origin: content-box;``` // 从content的外边缘,padding的内边缘，开始显示图片，决定了background-position的起始位置

#### 多重背景
* ```background-image: url(images/1.png),url(images/5.png);```
* ```background-position: 10px 10px,center center;```
* ```background: url(images/1.png) 10px 10px no-repeat,url(images/5.png) center center no-repeat;```

### CSS3 随机多背景--蝉原则
蝉原则：让事物重复出现的规律符合自然界的随机性原则即，以质数作为循环周期来增加“自然随机性”的策略，称为蝉原则

### CSS3 选择器
E~F 表示E元素后的所有F同级元素

属性选择符：
* E[attr ^= value]  :表示属性attr的值，以value开头的元素
* E[attr $= value]  :表示属性attr的值，以value结尾的元素
* E[attr *= value]  :表示属性attr的值，以包含value字符串的元素
* E:nth-child(3n+1) :查找子元素  所有子元素一起排列
* E：nth-of-type()  :选择父元素下第n个位置的子元素，所有匹配子元素,元素被提取出来，非E元素不参与排序
* E:last-child  :选择位于其父元素中最后一个位置且匹配E子元素
* E:last-of-type  : 选择位于其父元素匹配E的最后一个子元素
* E:only-child  :表示其父元素只包含一个子元素且该子元素匹配E
* E:only-of-type  :表示其父元素只包含同类子元素且该子元素匹配E
* E:empty  :匹配一个任何不包括子元素的元素
* E:not(:last-child)--E：not(s): 不含有s匹配符的E元素

### disabled="disabled"
<button disabled="disabled">确定</button> 
```<button disabled="disabled">确定</button>```//使得按钮变灰色，并无法点击

### 镜像倒影
-webkit-box-reflect :方位  偏移量  遮罩图片

方位：  above  below   left  right

### box-sizing
border-box --盒子采用“怪异盒子”模型方式计算，IE6以下版本

content-box --盒子采用“标准盒子”模型方式计算

-webkit-box-reflect: right 10px -webkit-linear-gradient(transparent,white);

-webkit-box-reflect: below 10px url(images/1.png);

### 多列multi-columns:  主要用在文本的多列布局方面以及报纸布局
* column-count: 列数：正整数，无单位  auto（默认），相当于不写
* column-width：列宽：column-width=(width-(n-1)*font-size)/n  n>=2
* column-gap: 列间距
* column-rule：列与列之间的边宽样式，不占任何空间位置
* column-span: 表示元素横跨列   none默认  all所有

### CSS3 转换 transform
rotate(): 正数  顺时针 负数  逆时针<br>
scale():X水平方向  Y垂直方向 0-1缩小  大于1放大<br>
translate: 平移  （水平方向，垂直方向）  正数 负数  小数<br>
skew: 倾斜（水平度数，垂直度数）<br>

```
transform: rotate(30deg);
transform: rotate(-30deg);
transform: rotateX(30deg);
transform: rotateY(30deg);
transform: rotateZ(30deg);
transform: scale(2);
transform: scale(-2);
transform: scale(-2,-2);
transform: scaleX(2);
transform: scaleY(2);
transform: scaleY(0.5);
transform: scale(-1);旋转180
transform: scale(-1,-1);旋转180
transform: scale(-1,1);水平翻转
transform: scale(1,-1);//*垂直翻转*//
transform: translate(30px,60px);
transform: translateX(30px);
transform: translateY(30px);
transform: translate(30.5px);
transform: translate(-30px);
transform: skew(30deg,45deg);
transform: skewX(30deg);
transform: skewY(45deg);
transform: skew(-30deg,45deg);
/*设置元素的基准点
        top  left  center   right
*/
transform-origin: top left;
transform-origin: 100px 100px;
transform-origin: 10% 50%;
transform: scale(2);
```

### CSS3 过渡 
transition: 通过将元素的某个属性，从一个属性值在一个时间内平滑过渡到另一个属性值，来完成过渡

transition: 过渡属性  过渡时间  过渡方式  过渡延迟

或  transition:    all    过渡时间  过渡方式  过渡延迟

过渡方式: 
* linear  匀速
* ease-in   由慢到快
* ease-out   由快到慢
* ease-in-out  慢-快-慢
* cubic-bezier   贝塞尔曲线

```transition: all 2s ease-in;```

### CSS3 Animation(动画)
animation: 属性名  持续时间  播放方式  循环次数  延迟时间  播放方向  播放状态
1. 动画播放方式：linear（线性过渡）  ease-in  ease-out  ease-in-out ease(平滑过渡)
2. 动画循环次数：infinite: 无限循环，可用数字表示
3. 动画延迟（动画开始时间）
4. 动画播放方向：指定元素动画播放方向，默认，动画每次循环向前播放
   * alternate:  偶数次时向前播放，奇数次反方向播放
5. 动画播放状态: 
   * running 默认，将暂停的动画重新播放
   * paused  正在播放的动画暂停
6. animation-fill-mode:
   * none:      默认值，不会对动画等待，动画完成时进行样式改变
   * forwards:  动画结束后，元素的样式设置为动画最后一帧的格式，防止立马跳回卡顿情况
   * backwards: 在动画等待的时间内，元素的样式设置为动画第一帧的样式
   * both:      同时运用他们属性

 <style>
        /* 设置 帧*/
        @keyframes myframes1 {
            from{background-color: #026873;width: 10px}
            /*走到30%时，width到100px*/
            30%{background-color: #026873;width: 100px}
            /*走到60%时，width回到10px*/
            60%{background-color: #026873;width: 10px}
            to{background-color: red;width: 200px}
        }/*谁用谁引*/
        @keyframes myframes2 {
            from{background-color: #026873;width: 10px}
            to{background-color: red;width: 200px}
        }

        .d1{
            width: 200px;
            height: 200px;
            /*background-color: red;*/
            animation: myframes1 4s ease infinite 1s alternate;
            /*animation-name: myframes1  /!*动画名*!/*/
            /*animation-duration: 4s  /!*动画持续时间*!/*/
            animation-timing-function : ease;
            animation-iteration-count: 2;
            animation-delay: 3s;
            animation-direction: alternate;
            animation-play-state: running;

            /*多重动画*/
            animation: myframes1 2s ease infinite,
                         myframes2 4s ease-in infinite;

        }
        .d1:hover{
            animation-play-state: paused;
        }
    </style>
</head>
<body>
    <div class="d1"></div>
</body>

```
或者：定义一个不存在的属性，然后动态加入
.ani{
    animation: myboxframes 5s linear infinite;
}
document.getElementsByClassName("box")[0].setAttribute("class","box ani");
```

### CSS3 3D
```
父盒子 {
	透视--景深
   	perspective: 1200px;
}
子盒子{
	开启三维效果
	transform-style: preserve-3d;默认2维
	平移
	translated3d(20px,20px,20px)
	translated3d(X,Y,Z)
	旋转
	rotate3d(1,0,0,45deg)
	rotate3d(X,Y,Z,45deg)
	rotateX  rotateY  rotateZ
	缩放
	scale3d(x,y,z)
	scaleX  scaleY  scaleZ
}
```

### CSS3 响应式设计
1. 视区：浏览器内部的可视区域大小
2. 视口：content="width=device-width"：视窗的宽度等于设备的宽度
    * winitial-scale=1.0：缩放比例为1
    * user-scaleable=no：禁止用户进行缩放操作
    * minimum-scale=1.0：最小比例
    * maximum-scale=1.0：最大比例
```
<meta charset="UTF-8">
<meta name="viewport" 
    content="width=device-width,initial-scale=1.0,user-scaleable=no" >
<title></title>
```

#### @media all and (max-width: 800px){  }

响应式：responsive wed-design 自适应设计，是识别屏幕尺寸做出页面的响应调整

@media
* all所有媒体类型
* print：打印机
* screen:计算机显示器
* handheld:手持设备
* projection:投影仪

* max-width://最大宽度：浏览器小于最大宽度时响应的样式
* max-height:
* min-width://最小宽度：大于最小宽度时响应的样式
* min-height:

```
<!-- 引入响应式代码方法-->
<!-- 方法2-->
<style media="screen and (max-width:600px)">
    @import url("css/res_css.css");
</style>
<!-- 方法3-->
<link href="css/res_css.css" rel="stylesheet"
      media="screen and (max-width:600px)">
```

#### 字体大小设置
em  范围：0-100  

需求：屏幕越大，文字大小也越大，元素的尺寸也能等比放大

#### 适合响应式的字体单位
* vw  视区宽度百分比
* vh  视区高度百分比
* vmin   vw或vh，取小的
* vmax   vw或vh，取大的

#### 浏览器宽度在600px-1000px之间变化时，
html根元素的字体大小在18px-22px之间对应变化

calc()动态计算长度值
1. 支持 + - * /
2. 任何长度值都可用该函数
```
width:calc(100% - 150px)
font-size: calc(18px + 4 * (100vw - 600px) / 400)
```

### Css3 flex 弹性布局（流布局）
```
父盒子{
    /*开启弹性布局*/
    display: flex;
}
子盒子{
    flex-grow: 1;
}
```

1. flex-direction:流布局方向 
* row; 默认，横向从左向右排列，（左对齐）让它内部的子盒子排成
* row-reverse; 反转横向排列，右对齐
* column; 纵向排列
* column-reverse; 反转纵向排列，右对齐

2. align-items; 垂直对齐方式
* flex-start; 垂直顶对齐
* flex-end; 垂直底对齐
* center; 中心点对齐
* stretch; 拉伸（默认） 指高度未设或设为auto将拉伸到整个容器高度
* baseline; 以项目的第一行文字为基线对齐

3. justify-content: 水平对齐方式
* flex-start; 左对齐（默认）
* flex-end;
* center;
* pace_between; 两端对齐，项目之间的间隔都相等
* space_around; 每个项目两侧的间隔相等

4. flex-wrap:内容超出一行时是否换行
* wrap; 换行
* nowrap; 不换行
* wrap-reverse; 换行倒置

5. order：元素的排列顺序，数值越小，排列越靠前

6. flex-grow:元素的放大比例
* 如元素没给宽度，会自动按grow给定的比例值计算
* 如指定宽度，会按比例分配剩余空间

7. flex-shrink：定义了元素的缩小比例

8. flex-basis：分配剩余空间前，元素占用的空间

flex：flex-grow flex-shrink  flex-basis  简写

flex-flow是flex-direction、flex-wrap的简写 ----flex-flow: column wrap;

### @charset "utf-8"; 分号不可省略


### CSS3换行
word-wrap   word-break   white-space

word-wrap: 换行
* break-word：英文单词和中文自动折行，超长英文大于盒子宽度时，其碰到盒子宽度时，会折行显示，该属性在table中无效
* break-all：强行拆词，达到词内换行效果  早期火狐，欧朋不支持
* keep-all：不允许汉字换行
* white-space：nowrap 文本不会换行，包括英文和中文
* overflow-x: hidden/scroll
* overflow-y: hidden/scroll

### resize: 自动缩放某个浏览器元素的大小
* none:不调整
* both
* horizontal:水平调整
* vertical:垂直调整

### (浏览器支持率低)
* nav-index: 导航序列号（相当于按tab键），设置浏览器获取焦点的顺序
* nav-up： 控制向上方向键
* nav-right：
* nav-down：
* nav-left：

### zoom：设置缩放比例，不支持度数
* text-decoration-color: #19ff0b;
* text-decoration-style: dashed;虚线
* text-decoration-style: double;双线
* text-decoration-style: wavy;波浪线
* text-decoration-style: dotted;点线
* text-decoration-style: solid;
* text-decoration-skip: spaces;
    * spaces：装饰条略过哪些，略过空白  
    * object：略过图片或内联块

### 等宽字体
1. 有的英文字体等宽
2. 等宽英文字体有：Consolas Monaco  monospace
3. ch：设置字宽  1ch表示一个字宽

font-size: 30px;<br>
width: 8ch; 个ch占一个字符<br>
font-family: Consolas; 有的字体<br>

### CSS3 条件判断
@supports   可以在浏览器不支持css3样式的情况下设置单独的属性
```
@supports ( border-radius) {
        .d2{
            border-radius: 100px;
        }
    }
```

### CSS3 变量
var(变量名)

var(--*)  *：自定义变量名称
```
:root{
    --a:#f23;    /*定义变量*/
    --你好:30px
}
body{
    background-color: var(--a);
}
div{
    --12:#345;
    font-size: var(--你好);
    color: var(--12);
}
```
