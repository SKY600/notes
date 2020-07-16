### new 操作符
    做了哪些事情：
        1. 创建了一个全新的对象
        2. 会被执行[[Prototype]]（也就是 \_\_proto\_\_ ）链接（链接到原型，将构造函数的 prototype 赋值给新对象的 \_\_proto\_\_）
        3. 使this指向新创建的对象
        // 返回对象（即下面的 4 5）
        4. 通过new 创建的每个对象将最终被[[Prototype]]链接到这个函数的prototype对象上
        5. 如果函数没有返回对象类型Object(包含Function,Array,Date,RegExp,Error)，那么new表达式中的函数调用将返回该对象引用

```
function name(name, age) {
    this.name = name;
    this.age = age;
    this.say = function () {
        console.log('name is: ', name, '; age is: ', age);
    }
}
function objectFactory() {
    const obj = new Object(); // {} 创建一个新对象
    const Constructor = [].shift.call(arguments); // 获得构造函数

    obj.__proto__ = Constructor.prototype; // 链接到原型（给obj这个新生对象的原型指向它的构造函数的原型）
    const ret = Constructor.apply(obj, arguments); // 绑定this
    return typeof ret === "object" ? ret : obj; // 确保new出来的是一个对象
}
objectFactory(name, 'cxk', '18').say();
console.log(new name(name, 'cxk', '18'));
```

### 实现new

    realizeNew() 方法需要传入的参数是：构造函数 + 属性
    1.首先我们创建一个新对象。
    2.然后通过 arguments 类数组我们可以知道参数中包含了构造函数以及我们调用 create 时传入的其他参数，接下来就是要想如何得到其中这个构
    造函数和其他的参数，由于 arguments 是类数组，没有直接的方法可以供其使用，我们可以有以下两种方法:
        1).Array.from(arguments).shift(); 转换成数组 使用数组的方法 shift 将第一项弹出。
        2).[].shift().call(arguments); 通过 call() 让 arguments 能够借用shift()方法。
     3.绑定this的时候需要注意：
        1).给构造函数传入属性，注意构造函数的this属性。
        2).参数传进 Con 对 obj 的属性赋值，this要指向 obj 对象。
        3).在 Con 内部手动指定函数执行时的this 使用call、apply实现。
    4.最后我们需要返回一个对象。

```
function realizeNew() {
    // 创建一个新对象
    let obj = {};
    // 获得构造函数
    let Con = [].shift.call(argument);
    // 链接到原型（给obj这个新生对象的原型指向它的构造函数的原型）
    obj.__proto__ = Con.prototype;
    // 绑定this
    let result = Con.apply(obj, arguments);
    // 确保new出来的是一个对象
    return typeof result === "object" ? result : obj;
}

function Person(name, age) {
    this.name = name;
    this.age = age;
    this.say = function() {
        console.log("I am " + this.name);
    }
}

//通过realize()方法创造实例
let person2 = realizeNew(Person, "Curry", 18);
console.log(person2.name); //"Curry"
console.log(person2.age); //18
person2.say(); //"I am Curry'

// 通过 new 创建构造实例
let person1 = new Person("Curry, 18); 
console.log(person1.name); //"Curry"
console.log(person1.age); //18
person1.say(); //"I am Curry'

```