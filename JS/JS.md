### 去除字符串中的空格
##### 1. 正则 replace 方法
* 去除字符串所有空格：
str.replace(/\s*/g, '');
* 去除字符串两头的空格
str.replace(/^\s*|\s*$/g, '');
* 去除字符串头部空格：
str.replace(/^\s*/g, '');
* 去除字符串尾部空格：
str.replace(/\s*$/g, '');

##### 2. trim()
str.trim(); // trim() 方法只能删除字符串两侧代码

##### 3. ES6
```
let str = '  a  b c  ';
let arr = Array.from(new Set(str.split(''))).filter(val => val && val.trim());
str = arr.join('');
```

### 查找字符串中出现次数最多的字符
```
var str = 'asdfssaaasasasasaa';

// 1. 定义一个Object用来存放拆分的字符串，然后遍历字符串。判断obj里面是否出现某一个字符，如果未出现则给obj添加以此字符为键值的属性，并赋值为1。反之则给此属性值++；
var obj = {};
for (var i = 0; i < str.length; i++) {
   if (!obj[str.charAt(i)]) {
       obj[str.charAt(i)] = 1;
   } else {
       obj[str.charAt(i)]++
   }
}

var obj ={
    a: 9,
    d: 1,
    f: 1,
    s: 7  
}

// 2. 定义两个变量用来接收出现最多的属性和值，for..in遍历对象的属性。如果obj[i]>max；重新为max赋值，然后一直重复，直到遍历结束拿到出现次数最多的值。
var max = 0,
    name = "";
for (var i in obj) {
    if (obj[i] > max) {
        max = obj[i];
        name = i;
    }
}
console.log('出现次数最多的是:' + name + '出现了' + max + '次'); //出现次数最多的是:a出现了9次 
```

### 访问函数内部的变量（三种方法）
通过return访问：

```
function bar(value) {
  var testValue = 'inner';
  return testValue + value;
}
console.log(bar('fun'));          // "innerfun"
```

通过 闭包 访问函数内部变量：

```
function bar(value) {
  var testValue = 'inner';
  var rusult = testValue + value;
  function innser() {
     return rusult;
  };
  return innser();
}
console.log(bar('fun'));          // "innerfun"
```

立即执行函数：

```
// 用它来分离全局作用域，形成一个单独的函数作用域。
<script type="text/javascript">
    (function() {
      var testValue = 123;
      var testFunc = function () { console.log('just test'); };
    })();
    console.log(window.testValue);            // undefined
    console.log(window.testFunc);             // undefined
</script>

// 它能够自动执行 (function() { //... })() 里面包裹的内容，能够很好地消除全局变量的影响；
```

### null 与 undefined 的区别
* null是一个表示”无”的对象，转为数值时为0，null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象
* undefined是一个表示”无”的原始值，转为数值时为NaN，当声明的变量还未被初始化时，变量的默认值为undefined。

undefined表示”缺少值”，就是此处应该有一个值，但是还没有定义。典型用法是：
1. 变量被声明了，但没有赋值时，就等于undefined。
2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
3. 对象没有赋值的属性，该属性的值为undefined。
4. 函数没有返回值时，默认返回undefined。

null表示”没有对象”，即该处不应该有值。典型用法是：
1. 作为函数的参数，表示该函数的参数不是对象。
2. 作为对象原型链的终点。

### 数组定制化排序
```
var objs1 = [
{ statusId: 'ABATE', statusName: '已过期' },
{ statusId: 'IS_CLOSED', statusName: '已关闭' },
{ statusId: 'QUOTE', statusName: '已报价' },
{ statusId: 'RETURNED', statusName: '已退回' },
{ statusId: 'UNQUOTE', statusName: '报价中' }
];

objs1.sort((a, b) => {
// order是规则 objs是需要排序的数组
var order = ["UNQUOTE", "QUOTE", "ABATE", "RETURNED", "IS_CLOSED"];
return order.indexOf(a.statusId || '') - order.indexOf(b.statusId || '');
});
console.log('obj: ', objs1);

// const arr = [{ key: 1, value: 'a' }, { key: 2, value: 'b' }, { key: 3, value: 'c' }];
// // 按照 key 分别为 2、3、1的顺序排列
// arr.sort((a, b) => {
// // order
// const order = [2, 3, 1];
// const x = order.indexOf(a.key);
// const y = order.indexOf(b.key);
// console.log('x:', x);
// console.log('y:', y);
// console.log('x-y: ', x - y);
// console.log('arr: ', arr);
// return x - y
// });

// console.log('arr: ', arr);

const arr = [{ key: 1, value: 'a' }, { key: 2, value: 'b' }, { key: 3, value: 'c' }];
arr.sort((a, b) => {
const order = [2, 3, 1];
const x = order.indexOf(a.key);
const y = order.indexOf(b.key);
return x - y
});

console.log('arr: ', arr);
```

### iframe的优缺点：
#### 优点：
* iframe能够原封不动的把嵌入的网页展现出来。
* 如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。
* 网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。
* 如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

#### 缺点：
* 会产生很多页面，不容易管理。
* iframe框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。
* 代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化。
* 很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。
* iframe框架页面会增加服务器的http请求，对于大型网站是不可取的。

分析了这么多，现在基本上都是用Ajax来代替iframe，所以iframe已经渐渐的退出了前端开发。

### WEB应用从服务器主动推送Data到客户端有那些方式？
#### Javascript数据推送
* Commet：基于HTTP长连接的服务器推送技术
* 基于WebSocket的推送方案
* SSE（Server-Send Event）：服务器推送数据新方式

### 截取字符串abcdace的ace
alert('abcdace'.substring(4));

### 规避javascript多人开发函数重名问题
* 命名空间
* 封闭空间
* js模块化mvc（数据层、表现层、控制层）
* seajs
* 变量转换成对象的属性
* 对象化

### javascript面向对象中继承实现

```
function Person(name){
        this.name = name;
}

Person.prototype.showName = function(){
        alert(this.name);
}

function Worker(name, job){
        Person.apply(this,arguments)
        this.job = job;
}
for(var i in Person.prototype){
        Worker.prototype = Person.prototype;
}
new Worker('sl', 'coders').showName();
```

### 判断一个字符串中出现次数最多的字符，统计这个次数
```
var str = 'asdfssaaasasasasaa';
var json = {};

for (var i = 0; i < str.length; i++) {
        if(!json[str.charAt(i)]){
                json[str.charAt(i)] = 1;
        }else{
                json[str.charAt(i)]++;
        }
};
var iMax = 0;
var iIndex = '';
for(var i in json){
        if(json[i]>iMax){
                iMax = json[i];
                iIndex = i;
        }
}
alert('出现次数最多的是:'+iIndex+'出现'+iMax+'次');
```

### 求一个字符串的字节长度;
```
//假设一个中文占两个字节
var str = '22两是';

alert(getStrlen(str))

function getStrlen(str){
        var json = {len:0};
        var re = /[\u4e00-\u9fa5]/;
        for (var i = 0; i < str.length; i++) {
                if(re.test(str.charAt(i))){
                        json['len']++;
                }
        };
        return json['len']+str.length;
}
```

### 数组去重
```
var arr = [1,2,3,1,43,12,12,1];
var json = {};
var arr2 = [];
for (var i = 0; i < arr.length; i++) {
        if(!json[arr[i]]){
                json[arr[i]] = true;
        }else{
                json[arr[i]] = false;
        }

        if(json[arr[i]]){
                arr2.push(arr[i]);
        }
};

for (var i = 0; i < arr.length; i++) {
        if(!aa(arr[i], arr2)){
                arr2.push(arr[i])
        }
};
function aa(obj, arr){
        for (var i = 0; i < arr.length; i++) {
                if(arr[i] == obj) return true;
                else return false;
        };
}
alert(arr2)
```

### 写出3个使用this的典型应用
1. 事件： 如onclick  this->发生事件的对象
2. 构造函数          this->new 出来的object
3. call/apply        改变this

### 如何深度克隆
```
var arr = [1,2,43];
var json = {a:6,b:4,c:[1,2,3]};
var str = 'sdfsdf';

var json2 = clone(json);

alert(json['c'])
function clone(obj){
        var oNew = new obj.constructor(obj.valueOf());
        if(obj.constructor == Object){
                for(var i in obj){
                        oNew[i] = obj[i];
                        if(typeof(oNew[i]) == 'object'){
                                clone(oNew[i]);
                        }
                }
        }
        return oNew;
}
```

### JS中如何检测变量是String类型
```
 typeof(obj) == 'string'
 obj.constructor == String;
```

### 网页中实现一个计算当年还剩多少时间的倒数计时程序，要求网页上实时动态显示“××年还剩××天××时××分××秒”
```
var oDate = new Date();
var oYear = oDate.getFullYear();

var oNewDate = new Date();
oNewDate.setFullYear(oYear, 11, 31, 23, 59, 59);
var iTime = oNewDate.getTime()-oDate.getTime();

var iS = iTime/1000;
var iM = oNewDate.getMonth()-oDate.getMonth();
var iDate =iS
```

### 什么是语义化的HTML
内容使用特定标签，通过标签就能大概了解整体页面的布局分布

### 为什么利用多个域名来存储网站资源会更有效？
确保用户在不同地区能用最快的速度打开网站，其中某个域名崩溃用户也能通过其他郁闷访问网站

### 减低页面加载时间的方法
1. 压缩css、js文件
2. 合并js、css文件，减少http请求
3. 外部js、css文件放在最底下
4. 减少dom操作，尽可能用变量替代不必要的dom操作

### 什么是FOUC？你如何来避免FOUC？
* 由于css引入使用了@import 或者存在多个style标签以及css文件在页面底部引入使得css文件加载在html之后导致页面闪烁、花屏
* 用link加载css文件，放在head标签里面

### 文档类型的作用是什么？你知道多少种文档类型？
影响浏览器对html代码的编译渲染
* html2.0
* xHtml
* html5

### 浏览器标准模式和怪异模式之间的区别
盒模型解释不同

### 闭包
子函数能被外部调用到，则该作用连上的所有变量都会被保存下来。

### 什么是Javascript的模块模式，并举出实用实例。
js模块化mvc（数据层、表现层、控制层）
* seajs
* 命名空间

### 你如何组织自己的代码？是使用模块模式，还是使用经典继承的方法？
对内：模块模式
对外：继承

### 你如何优化自己的代码？
* 代码重用
* 避免全局变量（命名空间，封闭空间，模块化mvc..）
* 拆分函数避免函数过于臃肿
* 注释

### 你能解释一下JavaScript中的继承是如何工作的吗？
* 子构造函数中执行父构造函数，并用call\apply改变this
* 克隆父构造函数原型上的方法

### 解释AJAX的工作原理
1. 创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）
2. 判断数据传输方式(GET/POST)
3. 打开链接 open()
4. 发送 send()
5. 当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数
 
### 阻止事件冒泡和默认事件
* e.stopPropagation()
* e.preventDefault()
* return false

### 如何判断一个对象是否为数组：
1. obj instanceof Array  // true 或 false
2. Object.prototype.toString.call(obj) === '[object Array]';   // true 或 false
3. Array.isArray(obj)  // true 或 false
4. arr.constructor === Array  // true 或 false

注意： 不能用typeof(obj)  // object

### ”==”和“===”的不同：
前者会自动转换类型，后者不会

eval的作用：eval() 可以解析、解释并返回js对象的数组

### js中什么是伪数组？如何将伪数组转化为标准数组？    
伪数组（类数组）：无法直接调用数组方法或期望length属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。

典型的是函数的argument参数，还有像调用getElementsByTagName,document.childNodes之类的,它们都返回NodeList对象都属于伪数组。    

可以使用 Array.prototype.slice.call(fakeArray) 将数组转化为真正的Array对象。

### Argument的特性
arguments对象不能显式创建，arguments对象只有函数开始时才可用。函数的 arguments 对象并不是一个数组，访问单个参数的方式与访问数组元素的方式相同。索引 n 实际上是 arguments 对象的 0…n 属性的其中一个参数。

### 拖拽事件：
* dragstart: 开始拖拽
* dragover: 拖放过程中鼠标经过的元素
* drop: 被拖放元素被放下触发的事件

### 渐进增强和优雅降级
* 渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
* 优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

### DOM操作——添加、移除、移动、复制、创建和查找节点
1. 创建新节点      
* createDocumentFragment()    //创建一个DOM片段      
* createElement()   //创建一个具体的元素      
* createTextNode()   //创建一个文本节点
2. 添加、移除、替换、插入      
* appendChild()      
* removeChild()      
* replaceChild()      
* insertBefore() //并没有insertAfter()
3. 查找      
* getElementsByTagName()    //通过标签名称      
* getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)      
* getElementById()    //通过元素Id，唯一性

### 说说你对作用域链的理解
作用域链的作用：保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。

### 作用域链（实例属性的查找顺序）：
1. 实例内部
2. 实例所在对象的实例属性(即构造函数内的属性)
3. 实例所在对象的原型属性
4. 查找继承对象的实例属性
5. 查找继承对象的原型属性

### document.write()的用法
document.write()方法可以用在两个方面：
* 页面载入过程中用实时脚本创建页面内容
* 用延时脚本创建本窗口或新窗口的内容

document.write只能重绘整个页面。innerHTML可以重绘页面的一部分

### tabindex: 
点击tab键让窗体中的控件获得焦点
```
<!--<div tabindex="3">h3h</div>-->
<!--<div tabindex="2">h2hh</div>-->
<!--<div tabindex="4">h4hh</div>-->
<!--<div tabindex="-1">h-1</div>-->
```

### BOM常用对象：
* Window
* history 
* document
* location
* navgiator
* screen
* event

### 拖拽事件：
* draggable="true"  必须放到被拖拽元素中
* dragstart  拖放开始
* dragover  拖放过程中鼠标经过得到元素
* drop  被拖放元素放下触发的事件

### 3种强制类型转换和2种隐式类型转换?
* 强制（parseInt, parseFloat, number）
* 隐式（== – ===）

### 前端页面有哪三层构成：
* 结构层 HTM
* 表示层 CSS
* 行为层js

### Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？  
hasOwnProperty

### 简述一下src与href的区别       
* href是指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，用于超链接。      

* src是指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素。当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部。

### 间歇调用与超时调用
* setInterval()间歇调用
* setTimeout()超时调用，后边的时间为毫秒

### js单线程单线程的原因：
若js同时有两个线程，一个线程在一个dom节点上加内容，一个线程删除这个节点，浏览器应为那个为主呢？由于js主要用于用户交互及操作dom，决定了它只能是单线程。

### 检测浏览器所在客户端的平台
```
var v1 = navigator.platform;
var a = v1.toUpperCase();
if(v1.indexOf('WIN')){    
    alert("Windows操作系统");
} else if (v1.indexOf("MAC")) {    
    alert("Macintosh操作系统");
} else if (v1.indexOf("LINUX")) {    
    alert("Linux操作系统");
}
```

### css sprites的使用
Css 精灵 把一堆小的图片整合到一张大的图片上，减轻服务器对图片的请求数量。

### call和apply的区别
* Object.call(this,obj1,obj2,obj3)
* Object.apply(this,arguments)

### 行内元素有哪些？块级元素有哪些？
* css的和模型块级元素：div p h1 h2 h3 h4 h5 form url
* 行内元素：a b br i span input select

### Css盒模型：
内容 border margin padding 

### 下面浏览器的内核
* Ie（Ie内核）  
* 火狐（Gecko）  
* 谷歌（webkit）  
* opear（Presto）

### 栈和队列的区别?
* 栈的插入和删除操作都是在一端进行的，而队列的操作却是在两端进行的。
* 队列先进先出，栈先进后出。
* 栈只允许在表尾一端进行插入和删除，而队列只允许在表尾一端进行插入，在表头一端进行删除

### 栈和堆的区别？
* 栈区（stack）—   由编译器自动分配释放   ，存放函数的参数值，局部变量的值等。
* 堆区（heap）   —   一般由程序员分配释放，   若程序员不释放，程序结束时可能由OS回收。
* 堆（数据结构）：堆可以被看成是一棵树，如：堆排序；
* 栈（数据结构）：一种先进后出的数据结构。

### 事件代理
#### 定义：
（Event Delegation），又称之为事件委托。是 JavaScript 中常用绑定事件的常用技巧。顾名思义，“事件代理”即是把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务。

#### 原理：
DOM元素的事件冒泡。使用事件代理的好处是可以提高性能。

### attribute 和 property 的区别
* attribute是dom元素在文档中作为html标签拥有的属性；
* property就是dom元素在js中作为对象拥有的属性。

所以：对于html的标准属性来说，attribute和property是同步的，是会自动更新的，但是对于自定义的属性来说，他们是不同步的。

### HTML与 XHTML 的区别
1. HTML对于各大浏览器兼容性较差(pc端浏览器、手机端浏览器、PAD)，对于网页页面编写技巧要求比较高，现在web前端开发的静态网页，一般都是html4.0，HTML5就另当别论了。

2. XHTML可以很好处理各大浏览器的兼容(pc端浏览器、手机端浏览器、PAD)，看起来与HTML有些相象但是和HTML有不少的区别，XHTML的语法较为严谨，习惯松散结构的HTML编写者刚开始接触XHTML有些不习惯。XHTML结合了部分XML的强大功能及大多数HTML的简单特性。

3. HTML标签不区分大小写XHTML所有标签都必须小写。

4. XHTML标签必须成双成对。

5. html对标签顺序要求不严格，XHTML标签顺序必须正确。

6. html元素必须正确嵌套，不能乱。

### XML 和 JSON 的区别
1. 数据体积方面。JSON相对于XML来讲，数据的体积小，传递的速度更快些。
2. 数据交互方面。JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。
3. 数据描述方面。JSON对数据的描述性比XML较差。
4. 传输速度方面。JSON的速度要远远快于XML。