### 语法
```
fun.call(thisArg, param1, param2, ...)
fun.apply(thisArg, [param1,param2,...])
fun.bind(thisArg, param1, param2, ...)
```

### 返回值
* call/apply: fun执行的结果
* bind: 返回fun的拷贝，并拥有指定的this和初始参数

### call 与 apply 作用：
1. 将函数设为对象的属性
2. 执行并删除这个函数
3. 指定this到函数并传入给定参数执行函数
4. 如果不传入参数，默认指向window

### call 原理：
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

### apply 原理：
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

### bind
(待定)

### 详解 call
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

##### 模拟实现第一步：

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

##### 模拟实现第二步: 

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

##### 模拟实现第三步: 

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

##### 下面是call函数的原理 
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

