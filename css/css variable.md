CSS变量  是由CSS作者定义的实体，其中包含要在整个文档中重复使用的特定值。使用自定义属性来设置变量名，并使用特定的var()来访问。

### 一、CSS变量的用途
构建大型站点时，在这些网页中，有大量的CSS样式，并且会在许多场合大量重复的使用。比如说：为了保持一种配色方案，在这个配色方案中会有一些颜色经常重复出现在CSS样式表中。如果想要修改配色方案，不论是想单独修改某个颜色或是整套配色，都不是一个简单的问题，而且很容易出错。

好处：
1. CSS可以减轻工作的复杂性，更方便修改和添加。不需要额外的编译。
2. 第二个好处就是变量本身是包含语义的信息。使CSS文件变得更易读和容易理解。main-text-color比单纯出现在文档中的#00ff00要更加容易理解，特别是有相同颜色出现在不同的文件中时。

### 二、什么是CSS变量
CSS变量当前有两种形式：

1. <b>变量</b>，就是拥有合法标识符和合法的值。可以被使用在任意的地方。可以使用var()函数使用函数
var(--main-size)会返回--main-size对应的值。

2. <b>自定义属性</b>，这些属性使用--*where*的特殊格式作为名字
--main-size:30px;  即声明一个CSS语句，意思是：将30px赋值给--main-size变量。

<b>注意：</b>自定义属性和常规属性一样，只作用在当前层级，如果没有特别定义则将从其父元素继承其值。

在之前的标准中，自定义属性以var-作为前缀，后来才改成--前缀。Firefox 31和以后的版本遵循新标准。

### 三、变量的声明
**声明变量之前，变量名之前要加上两个连字符（--）**
```
body{
    --head-color:#ada5f3;
    --foot-color:#da56d4:
}
```
在这个例子中，body选择器中出现了两个变量：--head-color 和 --foot-color。

CSS变量与color、background这些正式属性并没有什么不同，只是没有默认含义。CSS变量其实与自定义的CSS属性用法相同。CSS变量又叫做 CSS自定义属性（CSS custom properties）之所以选用两根连字符（--）表示CSS变量，是因为$foo 被Sass占用了，@foo被Less占用了，为了产生冲突，所以就改用（--）表示CSS变量了

**注意：变量名的大小写敏感，--head不同于--Head，这是两个不同的变量。**

#### 变量的作用域：
一个变量的作用域是其对应的选择器的作用范围，div选择器对应div的范围，由于这个原因，全局的变量通常放在根元素:root里面，确保任何选择器都可以读取它们。

**（一）声明一个局部变量**
```
element {
  --main-bg-color: brown;
}
```
**（二）声明一个全局变量**
```
:root {
  --global-color: #666;
  --pane-padding: 5px 42px;
}
```
**：root  这个伪类选择器匹配的是文档树的根元素。对于HTML来说，：root表示的是<html>根元素，除了优先级更高以外，与html元素选择器相同。**

使用全局变量（在全局中声明以后，其他选择器也可以调用）
```
.demo{
   color: var(--global-color);
}
```

### 四、var()函数

#### （一）var( ) 函数用以读取变量，var（变量名）
```
.box{
    --cor:#ddf;
    background-color: var(--cor);
    width:400px;height:40px;
}
```

#### （二）var()函数还可以使用第二个参数，表示变量的默认值。如果这个变量不存在，就会使用这个默认值
```
.box{
    --cor:#ddf;
    background-color: var(--corlo , red);
    width:400px;height:40px;
}
```

在这个例子里，var()使用了两个参数，变量--corlo和默认值red，由于不存在--corlo这个变量，所以使用red默认值。

**注意：第二个参数不处理内部的逗号或是空格，都视为参数的一部分**
```font-size: var(--sizq,40px 12px);```


#### （三）  var()函数还可以用在变量的声明

    .box{
        --cor:#ddf;
        --bacolor: var(--cor);
    }

**注意：变量只能用作属性值，不能用作属性名；**

    .box{ 
        --size:font-size; 
        var(--size):30px; 
    }


**在这个例子中，变量--size用作属性名，这是无效的**


### 五、变量的类型
#### （一）字符串

如果变量值是一个字符串，可以与其他字符串进行拼串
    --main-text:"hello";
    --text:var(--main-text)"world ! ";

**实例：**（使用两个变量值，使内容显示为hello world）

    span:before{
        --main-text:"hello";
        --text:var(--main-text)"world ! ";
        content:"--text: "var(--text);
        background: skyblue
    }
 
#### （二）数值

**注意：如果变量的值为数值，则在使用的时候不可以与数值单位直接连用**

    .foo {
        --gap: 20;
        /* 无效 */
        margin-top: var(--gap)px;
    }

本例中，使用时数值与单位直接写在一起，这是无效的。必须使用  <b>calc()</b> 函数，将它们连接。

    span{
        background: skyblue;
        --size:30;
        font-size:calc(var(--size)*1px);
    }`

**注意：如果变量赋值的时候带有单位（例如：--size:20px;），就不能写成字符串**


    /* 无效 */
    .foo {
    --foo: '20px';
    font-size: var(--foo);
    }

    /* 有效 */
    .foo {
    --foo: 20px;
    font-size: var(--foo);
    }


### 六、响应式布局
CSS 是动态的，页面的任何变化，都会导致采用的规则变化。

利用这个特点，可以在响应式布局的 media命令里面声明变量，使得不同的屏幕宽度有不同的变量值。

    body {
        --primary: #7F583F;
        --secondary: #F7EFD2;
    }
    a {
        color: var(--primary);
        text-decoration-color: var(--secondary);
    }

    @media screen and (min-width: 768px) {
        body {
            --primary:  #F7EFD2;
            --secondary: #7F583F;
        }
    }
 
### 七、兼容性处理
对于不支持CSS变量的浏览器，可采用下面的写法（为了适应一些不支持CSS变量的浏览器，还可以在样式中另外用CSS样式单独声明以下）

    span{
        --size:30px;
        font-size:var(--size);
                font-size:30px;
    }
可以使用  @supports  检测浏览器是否支持CSS变量

    @supports ( (--a: 0)) {
    /* supported */
    }

    @supports ( not (--a: 0)) {
    /* not supported */
    }

### 八、JavaScript操作
JavaScript也可以检验浏览器是否支持CSS变量

    const isSupported =
        window.CSS &&
        window.CSS.supports &&
        window.CSS.supports('--a', 0);

    if (isSupported) {
        /* supported */
    } else {
        /* not supported */
    }

JavaScript操作CSS变量的写法如下（这表示JavaScript可以实现将任意值存入CSS样式表）：

    // 设置变量
    document.body.style.setProperty('--primary', '#7F583F');

    // 读取变量
    document.body.style.getPropertyValue('--primary').trim();
    // '#7F583F'

    // 删除变量
    document.body.style.removeProperty('--primary');

 

### 关于CSS变量的继承性：
优先级低的继承优先级高的变量，子元素继承父元素的变量。

    <div class="one"></div>
    <div class="two">
        Text  <span class="five">- more text</span>
    </div>
    <input class="three">
    <textarea class="four">Lorem Ipsum</textarea>

定义下面的CSS：

    .two { --test: 10px; }
    .three { --test: 2em; }
在这个例子中，var(--test)的结果是：

    class="two" 对应的节点: 10px
    class="three" 对应的节点: element: 2em
    class="four" 对应的节点: 10px (inherited from its parent)
    class="one" 对应的节点: 无效值, 即此属性值为未被自定义css变量覆盖的默认值