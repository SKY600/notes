### ES6新特性：
#### 1. let和const
ES5中是没有块级作用域的，并且var有变量提升，在let中，使用的变量一定要进行声明

#### 2. 模板字符串
模板字符串是增强版的字符串，用反引号（`）标识，可以当作普通字符串使用，也可以用来定义多行字符串

#### 3. 箭头函数
ES6中的函数定义不再使用关键字function()，而是利用了()=>来进行定义

#### 4. 解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值

#### 5. ...操作符
可以将数组或对象里面的值展开；还可以将多个值收集为一个变量

#### 6. class、extends、super
#### 7. Promise
Promise是异步编程的一种解决方案，比传统的解决方案（回调函数和事件）更合理、强大

#### 8. Set
Set数据结构，类似数组。所有的数据都是唯一的，没有重复的值。它本身是一个构造函数

#### 9. import 和 export
ES6标准中，Js原生支持模块(module)。将JS代码分割成不同功能的小块进行模块化，将不同功能的代码分别写在不同文件中，各模块只需导出公共接口部分，然后通过模块的导入的方式可以在其他地方使用

#### 10. Symbol
Symbol是一种基本类型。Symbol 通过调用symbol函数产生，它接收一个可选的名字参数，该函数返回的symbol是唯一的

* 用途：
        1. 消除魔术串
        2. 遍历属性名使用自己的方式Object.getOwnPropertySymbols(),返回所有Symbol值
* 声明的方法：
        1. Symbol.for() 声明在全局环境中，可以被搜索
        2. Symbol() 不可搜索，每一次调用都产生一个新的Symbol值
        3. let a = Symbol('str')


#### 11. Proxy代理
使用代理（Proxy）监听对象的操作，然后可以做一些相应事情

#### 12. async、await
使用 async/await, 搭配promise,可以通过编写形似同步的代码来处理异步流程, 提高代码的简洁性和可读性

async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成

#### 13. 修饰器 @
decorator是一个函数，用来修改类甚至于是方法的行为。修饰器本质就是编译时执行的函数

### JS 与 ES6
#### 声明变量：
* ES5: var function命令
* ES6: var let const class @import function命令

#### 作用域：
* ES5: 全局作用域、函数作用域
* ES6: 块级作用域

#### 数据类型：
* ES5: 基本数据类型5种：String, Number, Boolean, Null, Undefined  复杂数据类型：Object
* ES6: 基本数据类型6种：String, Number, Boolean, Null, Undefined, <b>Symbol</b> （新增，用于声明一种独一无二的值，常常用来作为函数的属性名）

#### 数组
##### 1. 定义数组
* ES5: （1）构造函数 ```new Array('red')``` （2）数组字面量 ```var a = []``` （3）改变数组长度
* ES6:  定义一个不含重复元素的数组 ```let array = new Set([1,2,2,3,4,5])```

##### 2. 检测数组的方法
* ES5： ```array instanceof Array```,  ```Array.isArray(arr1)```

##### 3. 数组变字符串输出
* ES5: ```array.toString()```,  ```array.join()```
* ES6：```...[array]```
	 
##### 4. pop()、push()、shift()、unshift()
* Pop()---- 返回被删除元素，原数组改变
* push()----返回被添加元素，原数组改变
* shift() 
* unshift()，
	
##### 5. 复制数组, 合并数组
* ES5：```concat()```，并将参数添加到数组中，生成了<b>新的数组</b> a.concat(), <b>浅复制</b>
* ES6：```[...arr1,...arr2浅拷贝]```, ```var arr2 = [...arr1]``` <b>深拷贝</b>生成数组
；数组扩展运算符与解构赋值结合 ```var [first, ...second] = [1,3,3,4,5,6]```
 
##### 6. 倒叙输出数组元素 reverse() ，原数组被覆盖

##### 7. 原数组排序  array.sort()  
sort()方法根据字母在字符编码表中的顺序进行排序，将数组转换成字符串进行比较，null，undefined排在最后""在最前面

##### 8. 删除数组中的元素  
1. ```slice()``` 返回被删除位置元素，原素组不变
2. ```splice()``` 返回被删除位置元素，原素组改变
 
##### 9. 数组位置方法
* ES5: ```array.indexOf('2')```--- 有元素就返回元素位置，没有返回-1，使用全等运算符进行比较；```Array.lastIndexOf('3')```
* ES6: ```[1,2,3].includes(1)``` //true   ```NAN==NAN```
 
##### 10. 数组迭代方法：
* arr.every(function(item) {}) 数组中每一项都运行给定函数该函数对每一项都true返回true
* arr.some(function(item){}) 数组中每一项都运行给定函数该函数对任一项都true返回true
* arr.filter(function(item){}) 返回true项的结果集
* arr.map() 返回每一项调用后的结果集
* arr.forEach() 返回数组中每一项运行传入的函数
 
##### 11. 数组归并方法
* ES5: ```Array.reduce(function(prev,cur,index,array) {}) ```，数组遍历 ```Array.educeRight()```
* ES6: ```...[array]```

##### 12. 求数组最大的值
* ES5: ```Math.max.apply(null,[1,2,3])```
* ES6: ```Math.max(...[1,2,3])```
	
##### 13. 添加数组到数组中
* ES5：```Array.prototype.push.apply(arr1,arr2)``` arr2添加到arr1中，返回数组长度
* ES6：```arr1.push(...[arr2])```
 
##### 14. ES6扩展运算符的应用

##### 15. 对象转成数组
1. ```Array.from``` 方法用于将两类对象转为真正的数组：<b>类似数组的对象（array-like object）</b> 和 <b>可遍历（iterable）的对象</b>（包括 ES6 新增的数据结构 Set 和 Map）。只要有length属性就可以进行转换。
2. ```const args = [...arguments]``` 遍历器接口（Symbol.iterator），如果一个对象没有部署这个接口，就无法转换
3. 还没有部署该方法的浏览器，可以用 ```Array.prototype.slice``` 方法<b>替代</b>```Array.from```，还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

##### 16. Array.of()
```Array.of``` 方法用于将一组值转换为数组。是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。如：var a = new Array(3)此时a = 1代表长度

##### 17. 数组实例的copyWithin()
在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会<b>修改当前数组</b>。

```Array.property.copyWithin(target,start,end)```

##### 18. 数组实例的find()
用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined

```array.find(function(value,index,arr))```

##### 19. 数组实例的findIndex()
用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

##### 20. 数组实例的 fill() 
fill方法使用给定值，填充一个数组。```[a,b,c].fill(7,start,end)```
 
##### 21. 数组实例的 entries()，keys() 和 values() 可以使用for...of遍历

##### 22. Array.prototype.includes()
返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似```[1,2,3].includes(1)``` //true

##### 23. 多维数组变成一维数组 
```[1,[2,3]].flat()``` //[1,2,3]   

```[1,[[2],3]].flat(2)``` //[1,2,3]

```flat()``` 方法会跳过空位 ```flatMap()``` 方法对原数组的每个成员执行一个函数（相当于执行```Array.prototype.map()```），然后对返回值组成的数组执行 ```flat()``` 方法。该方法返回一个新数组，不改变原数组。

##### 24. 空位的处理
* ES5大多数情况下会忽略空位forEach(), filter(), reduce(), every() 和some()都会跳过空位。
  map()会跳过空位，但会保留这个值join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。
* ES6 则是明确将空位转为undefined。