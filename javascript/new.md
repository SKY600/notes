### new 操作符
1. 创建了一个全新的对象
2. 会被执行[[Prototype]]（也就是 \_\_proto\_\_ ）链接（链接到原型，将构造函数的 prototype 赋值给新对象的 \_\_proto\_\_）
3. 使this指向新创建的对象
<br> // 返回对象（即下面的 4 5）<br>
4. 通过new 创建的每个对象将最终被[[Prototype]]链接到这个函数的prototype对象上
5. 如果函数没有返回对象类型Object(包含Function,Array,Date,RegExp,Error)，那么new表达式中的函数调用将返回该对象引用