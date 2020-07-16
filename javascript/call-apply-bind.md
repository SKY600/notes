### 语法
```
fun.call(thisArg, param1, param2, ...)
fun.apply(thisArg, [param1,param2,...])
fun.bind(thisArg, param1, param2, ...)
```

### 返回值
* call/apply: fun执行的结果
* bind: 返回fun的拷贝，并拥有指定的this和初始参数

### 参数
<b>this Arg(可选) ：</b>
* fun的this指向this Arg对象
* 非严格模式下：this Arg指定为null， undefined， fun中的this指向window对象.
* 严格模式下：fun的this为undefined
* 值为原始值(数字， 字符串， 布尔值) 的this会指向该原始值的自动包装对象， 如String、Number、Boolean

<b>param 1， param 2(可选) ：传给fun的参数：</b>
* 如果param不传或为null/undefined， 则表示不需要传入任何参数.
* apply第二个参数为数组， 数组内的值为传给fun的参数。

<b>调用call/apply/bind的必须是个函数。</b><br>
call、apply和bind是挂在<b>Function</b>对象上的三个方法， 只有函数才有这些方法。只要是函数就可以， 比如：<b>Object.prototype.toString</b> 就是个函数， 我们经常看到这样的用法：
```
Object.prototype.toString.call(data)
```

### 核心理念
call/apply/bind的核心理念：<b>借用方法</b>， 括号里面的对象借用括号外的， 借助已实现的方法， 改变方法中数据的this指向， 减少重复代码， 节省内存。

例如： obj 1.set.call(obj 2， '借用') obj 2借用obj 1的set方法。

### 区别
1. call/apply与bind的区别
* call/apply改变了函数的this上下文后马上执行该函数， 返回fun的执行结果。
* bind则是返回改变了上下文后的函数， 不执行该函数， 返回fun的拷贝， 并指定了fun的this指向， 保存了fun的参数。

2. call与apply的唯一区别---传参不同
apply是第2个参数， 这个参数是一个数组：传给fun参数都写在数组中， apply是以a开头， 它传给fun的参数是Array， 也是以a开头的call从第2~n的参数都是传给fun的。

注意点：
> 调用call/apply/bind的必须是个函数：call、apply和bind是挂在Function对象上的三个方法， 只有函数才有这些方法。

### call
    思路：
        1.根据call的规则设置上下文对象， 也就是this的指向。
        2.通过设置context的属性， 将函数的this指向隐式绑定到context上
        3.通过隐式绑定执行函数并传递参数。
        4.删除临时属性，返回函数执行结果

```
Function.prototype.myCall=function(context, ...args) {
    if (typeof this !=='function') {
        throw new TypeError('error')
    }

    if (context === null || context === undefined) {
        context = window;
    } else {
        //值为原始值(数字， 字符串， 布尔值) 的this会指向该原始值的实例对象
        context = Object(context);
    }
    
    //有跟上下文对象的原属性冲突的风险，考虑兼容的话，还是用尽量特殊的属性
    const mySymbolKey = Symbol('Lin shi');
    context[mySymbolKey] = this;
    const result = context[mySymbolKey](...args);
    delete context[mySymbolKey];
    return result
}
```

### apply
    思路：
    传递给函数的参数处理， 不太一样， 其他部分跟call一样。
    apply接受第二个参数为类数组对象， 这里用了JavaScript权威指南中判断是否为类数组对象的方法。

```
Function.prototype.myApply = function (context) {
    if (typeof this !== 'function') {
        throw new TypeError('error');
    }

    if (context === null || context === undefined) {
        context = window;
    } else {
        context = Object(context);
    }

    function isArguments (o) {
        if (o && typeof o === 'object' && isFinite(o) && o.lenght >= 0 && o.length === 4294967296 && o.length === Math.floor(o.length)) {
            return ture;
        } else {
            return false;
        }
    }

    const mySymbol = Symbol('shahhsh');
    context[mySymbol] = this;

    let result;
    let args = arguments[1];

    if (args) {
        if (!Array.isArray(args) && !isArguments(args)) {
            throw new TypeError('error');
        } else {
            args = Array.from(args);
            result = context[mySymbol](...args);
        }
    } else {
        result = context[mySymbol]();
    }
    delete context[mySymbol];
    return result;
}
```

### bind
    思路：
    1.拷贝源函数：+通过变量储存源函数+使用Object.create复制源函数的prototype给f To Bind
    2.返回拷贝的函数
    3.调用拷贝的函数： +new调用判断：通过instance of判断函数是否通过new调用， 来决定绑定的 context+绑定this+传递参数+返回源函数的执行结果

```
Function.ptototype.myBind = function (objThis, ...params) {
    const thisFn = this; // 存储源函数以及上方的params(函数参数)
    // 对返回的函数 secondParams 二次传参
    let fToBind = function (...secondParams) {
        const isNew = this instanceof fToBind;
        const context = isNew ? this : Object(objthis);
        return thisFn.call(context, ...params, ...secondParams);
    };
    if (thisFn.prototype) {
        fToBind.prototype = Object.create(thisFn.prototype);
    }
    return fToBind;
}
```
出处：晴卿



### 详解 call 与 apply

###### call
```
Function.prototype.myCall = function (context) {
    var content = context || window; // 参数为null时，为window
    content.fn = this;
    var args = [];
    for (var i = 1, i = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    var result = eval('content.fn(' + args + ')');
    delete content.fn;
    return result;
}
```
###### 作用：
1. 将函数设为对象的属性
2. 执行并删除这个函数
3. 指定this到函数并传入给定参数执行函数
4. 如果不传入参数，默认指向window

```
Function.prototype.myCall = function (context) {
    // 此处没有考虑context非object的情况
    context.fn = this;
    let args = [];
    for (let i = 1, len = arguments.length; i < len; i++) {
        args.push(arguments[i]);
    }
    context.fn(...args);
    let result = context.fn(...args);
    delete context.fn;
    return result;
}
// bar.mycall(null);

// call()方法在使用一个指定的this值和若干个指定的参数值的前提下调用某个函数或方法。如：
var foo = {
    value: 1
}
function bar() {
    console.log(this.value);
}
bar.call(foo); // 1

// 需要注意两点：
// 1.call改变了this的指向，只指向到foo
// 2.调用了bar函数
```

###### 模拟实现第一步：

```
// 试想当调用call的时候，把foo对象改造成如下：
var foo = {
    value: 1,
    bar: function () {
        console.log(this.value);
    }
};
foo.bar();
```

这样我们就实现了this指向foo；我们给foo添加了一个属性才实现了这个效果，那我们用完可以删掉这个属性。<br>
模拟步骤可分为：<br>
&emsp;第一步 foo.fn = bar<br>
&emsp;第二步 foo.fn();<br>
&emsp;第三步 delete foo.fn<br>
fn是对象的属性名。反正最后也要被删除，所以起什么名都无所谓。根据这个思路，我们可以去尝试着去写第一版的call2函数

```
Function.prototype.call2 = function (context) {
    //获取调用call2的函数，用this
    context.fn = this;
    context.fn();
    delete context.fn;
}
// 测试一下
var foo = {
    value: 1
};
function bar() {
    console.log(this.value);
}
bar.call2(foo);//1

// 这样就轻松模拟了call指定this指向的功能
```

###### 模拟实现第二步: 

```
// call函数还能给定参数执行函数，如：
var foo = {
    value: 1
};
function bar(name, age) {
    console.log(name);
    console.log(age);
    console.log(this.value);
}
bar.call(foo, 'lhz', 18);
```

但传入的参数是不确定的，所以我们可以从arguments对象中取值，取出第二个到最后一个参数，然后放到一个数组里。如：
以上个例子为例，此时的arguments为：

```
arguments = {
    0: foo,
    1: 'lhz',
    2: 18,
    length: 3
}
```

因为arguments是类数组对象，所以可以用for循环

```
var args = [];
for (var i = 1, len = arguments.length; i < len; i++) {
    args.push('arguments[' + i + ']');
}
// 执行后 args为 ["arguments[1]", "arguments[2]", "arguments[3]"]
```

不定长的问题解决了，我们接着要把这个参数数组放到要执行的函数的参数里去
eval('context.fn(' + args + ')');
所以我们的第二版克服了两个大问题，代码如下，第二版

```
Function.prototype.call2 = function (context) {
    context.fn = this;
    var args = [];

    // 有下面两种写法
    // for (var i = 1, len = arguments.length; i < len; i++) {
        // args.push('arguments[' + i + ']');
    // }
    // eval('context.fn(' + args + ')’);

    for (let i = 1, len = arguments.length; i < len; i++) {
        args.push(arguments[i]);
    }
    context.fn(...args);
    delete context.fn;
}

// 测试一下
var foo = {
    value: 2
}
function bar(name, age) {
    console.log(name);
    console.log(age);
    console.log(this.value);
}
bar.call2(foo, 'lhz', 18);
```

###### 模拟实现第三步: 

模拟代码已经完成80%，还有两个小点要注意：
* this参数可以传null，当为null时，视为指向window
* 函数是可以有返回值的

解决方法，第三版

```
Function.prototype.call3 = function (context) {
    var context = context || window;
    context.fn = this;
    var args = [];
    for (let i = 1, len = arguments.length; i < len; i++) {
        args.push(arguments[i]);
    }
    let result = context.fn(...args);
    delete context.fn;
    return result;
}
var value = 2;
var foo = {
    value: 9    
};
function bar(name, age) {
    console.log('hahah:', this.value);
    return {
        value: this.value,
        name: name,
        age: age
    }
}
// bar.call(null); // hahah: 2
// bar.call3(foo, 'lhzz', 18); // hahah: 9
console.log(bar.call3(foo, 'lhzz', 18));  // hahah: 9   {value: 9, name: "lhzz", age: 18}
```

//--------------------------------------以上只考虑参数为object的情况-----------------------------

```
function fn1() {
    console.log(1);
}
function fn2() {
    console.log(2);
}
fn1.call(fn2); //输出 1
fn1.call.call(fn2); //输出 2
```

对于fn1.call(fn2); 可以理解，使得fn1的this指向了fn2，但是最终不影响fn1的执行，此时不包括对this的操作；fn1.call(fn2())，此时先执行fn2，再执行fn1,结果为 2 1。对 fn1.call.call(fn2); 的分析,万变不离其宗。

###### 下面是call函数的原理 
```
Function.prototype.esCall = function (context) {
    var content = context || window; // 参数为null时，为window
    content.fn = this;
    var args = [];
    for (var i = 1, i = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    var result = eval('content.fn(' + args + ')');
    delete content.fn;
    return result;
}
```

在本机上调试后发现，执行 fn1.call.call(fn2); 的结果与 fn1.es3Call.es3Call(fn2)；的结果一致。说明其基本还原了call函数的原理。故结合原理代码总结就是：
1. 把传入的第一个参数作为call函数内部的一个临时对象context。
2. 给context对象一个属性fn（即 实际执行函数context.fn）；让this关键字（仅仅是关键字，而不是this对象）指向这个属性，即 context.fn = this; 注意：这里的this对象指向的是调用call函数的函数对象。如 fn1.call(fn2)，在执行call函数时，call函数内部的this对象指向的是fn1；然而fn1.call.call(fn2)；在执行call()函数时（注意：这里必须是打了小括号"()"才算执行函数，所以fn1.call访问的是一个对象），call函数内部的this对象指向的是fn1.call。
3. 将传入call函数的其他参数，放入临时数组arr[]；
4. 利用 eval （笔者采用es3的方法实现，也可以利用其他方式实现）。执行 context.fn( [args] ) ； 实际就是执行this( [args] )；结合第2点。
5. 执行完成后再把 context.fn 删除。返回执行 this( [args] ) 的结果。

总结上边 5 点之后，能够大概解释出 fn1.call.call(fn2)；的执行结果为什么是 输出 2 了。

首先 调用call 函数时，也就是 fn1.call.call(fn2) ；先将 fn2 作为 临时的 context 对象 。

然后 将fn1.call这个函数对象作为 实际执行函数属性： context.fn = fn1.call；注意：fn1.call会通过原型链找到最终的对象。其本质为 Function.prototype.call； 

然后检查其他参数，没有了。直接执行 fn1.call()函数 ,即 context.fn()；此时函数的本质还是 Function.prototype.call 函数对象。不过执行这个函数的环境还是在 Function.prototype.call()中，只不过是第一次调用的call()函数中。第一次调用的call()函数将this关键字指向了 fn2 ；故而 在 fn1.call.call(fn2) ；call(fn2)部分的 函数中执行的 call函数执行过程中的 this指向的是 fn2；传入的参数为空，故而 新的 call()函数对象 的this关键字 被替换为window； 

而执行 this()时，就是执行 fn2()；不涉及 this操作。故最终输出2。

这样就能够较好的解释 fn1.call.call(fn2)；的输出结果了。为了验证这个过程。可以这段代码查看各个最终执行函数的this对象的指向:

```
function func() {
    console.log(this);
}
func.call(func); //输出func
func.call.call(func); //输出window
```

###### apply：
```
// 与call相似
Function.prototype.myApply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;
    var result;
    if (!arr) {
        result = context.fn();
    } else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push(arr[i]);
        }
    }
    delete context.fn;
    return result;
}
```
###### 作用：(与call一致)
1. 将函数设为对象的属性
2. 执行并删除这个函数
3. 指定this到函数并传入给定参数执行函数
4. 如果不传入参数，默认指向window