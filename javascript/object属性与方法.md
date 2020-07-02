### Object 构造函数的属性
> Object.length  值为 1。

> Object.prototype   可以为所有 Object 类型的对象添加属性。

### Object 构造函数的方法
    1. Object.assign() —— 通过复制一个或多个对象来创建一个新的对象。
    2. Object.create() —— 通过指定的原型对象和属性创建一个新对象。
    3. Object.defineProperty() —— 给对象添加一个属性并指定该属性的配置。
    4. Object.defineProperties() —— 给对象添加多个属性并分别指定它们的配置。
    5. Object.entries() —— 返回给定对象自身可枚举属性的[key,value]数组。
    6. Object.freeze() —— 冻结对象，其他代码不能删除或更改任何属性。
    7. Object.getOwnPropertyDescriptor() —— 返回对象指定的属性配置。
    8. Object.getOwnPropertyNames() —— 返回一个数组，它包含指定对象所有的可枚举或不可枚举的属性名。
    9. Object.getOwnPropertySymbols() —— 返回一个数组，它包含了指定对象自身所有的符号属性。
    10. Object.getPrototypeOf() —— 返回指定对象的原型对象。
    11. Object.is() —— 比较两个值是否相同。所有NaN值都相等（与==和===不同）
    12. Object.isExtensible() —— 判断对象是否可扩展。
    13. Object.isFrozen() —— 判断对象是否已经冻结。
    14. Object.isSealed() —— 判断对象是否已经密封。
    15. Object.keys() —— 返回一个包含所有给定对象自身可枚举属性名称的数组。
    16. Object.preventExtensios() —— 防止对象的任何扩展。
    17. Object.seal() —— 防止其他代码删除对象的属性。
    18. Object.setPrototypeOf() —— 设置对象的原型（即内部[[prototype]]属性）。
    19. Object.values() —— 返回给定对象自身可枚举值的数组。
    20. Object.fromEntries() —— Object.entries()的逆操作，用于将一个键值对数组转为对象。

#### 1. Object.assign()

Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

<b>语法:</b>
> Object.assign(target, ...sources)。

<b>参数:</b>
<br>target：目标对象。
<br>sources：源对象。

<b>返回值:</b>
<br>目标对象。

<b>demo:</b>
```
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);
console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

<b>描述:</b>

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

Object.assign 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。该方法使用源对象的[[Get]]和目标对象的[[Set]]，所以它会调用相关 getter 和 setter。因此，它分配属性，而不仅仅是复制或定义新的属性。如果合并源包含getter，这可能使其不适合将新属性合并到原型中。为了将属性定义（包括其可枚举性）复制到原型，应使用Object.getOwnPropertyDescriptor()和Object.defineProperty() 。

String类型和 Symbol 类型的属性都会被拷贝。

在出现错误的情况下，例如，如果属性不可写，会引发TypeError，如果在引发错误之前添加了任何属性，则可以更改target对象。

注意，Object.assign 不会在那些source对象值为 null 或 undefined 的时候抛出错误。

<b>示例:</b>

* 数组的处理

```
Object.assign可以用来处理数组，但是会把数组视为对象。

Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
上面代码中，Object.assign把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性4覆盖了目标数组的 0 号属性1。
```

* 复制一个对象
```
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```
* 深拷贝问题
```
// 针对深拷贝，需要使用其他办法，因为 Object.assign()拷贝的是（可枚举）属性值。假如源值是一个对象的引用，它仅仅会复制其引用值。
const log = console.log;
function test() {
    'use strict';
    let obj1 = { a: 0 , b: { c: 0}}; 
    let obj2 = Object.assign({}, obj1); 
    log(JSON.stringify(obj2));
    // { a: 0, b: { c: 0}} 

    obj1.a = 1; 
    log(JSON.stringify(obj1));
    // { a: 1, b: { c: 0}} 
    log(JSON.stringify(obj2));
    // { a: 0, b: { c: 0}} 

    obj2.a = 2; 
    log(JSON.stringify(obj1));
    // { a: 1, b: { c: 0}} 
    log(JSON.stringify(obj2));
    // { a: 2, b: { c: 0}}
 
    obj2.b.c = 3; 
    log(JSON.stringify(obj1));
    // { a: 1, b: { c: 3}} 
    log(JSON.stringify(obj2));
    // { a: 2, b: { c: 3}} 

    // Deep Clone 
    obj1 = { a: 0 , b: { c: 0}}; 
    let obj3 = JSON.parse(JSON.stringify(obj1)); 
    obj1.a = 4; 
    obj1.b.c = 4; 
    log(JSON.stringify(obj3));
    // { a: 0, b: { c: 0}}
}
​​​​​​​test();
```
* 合并对象
```
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };
const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
```

* 合并具有相同属性的对象
```
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };
const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```
属性被后续参数中具有相同属性的其他对象覆盖。

* 拷贝 symbol 类型的属性
```
const o1 = { a: 1 };
const o2 = { [Symbol('foo')]: 2 };
const obj = Object.assign({}, o1, o2);
console.log(obj); // { a : 1, [Symbol("foo")]: 2 } (cf. bug 1207182 on Firefox)
Object.getOwnPropertySymbols(obj); // [Symbol(foo)]
```

* 继承属性和不可枚举属性是不能拷贝的
```
const obj = Object.create({foo: 1}, { // foo 是个继承属性。
    bar: {
        value: 2  // bar 是个不可枚举属性。
    },
    baz: {
        value: 3,
        enumerable: true  // baz 是个自身可枚举属性。
    }
});
const copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }
```

* 原始类型会被包装为对象
```
const v1 = "abc";
const v2 = true;
const v3 = 10;
const v4 = Symbol("foo")
```
```
const obj = Object.assign({}, v1, null, v2, undefined, v3, v4); 
// 原始类型会被包装，null 和 undefined 会被忽略。
// 注意，只有字符串的包装对象才可能有自身可枚举属性。
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

* 异常会打断后续拷贝任务
```
const target = Object.defineProperty({}, "foo", {
    value: 1,
    writable: false
}); // target 的 foo 属性是个只读属性。
Object.assign(target, {bar: 2}, {foo2: 3, foo: 3, foo3: 3}, {baz: 4});
// TypeError: "foo" is read-only
// 注意这个异常是在拷贝第二个源对象的第二个属性时发生的。
console.log(target.bar);  // 2，说明第一个源对象拷贝成功了。
console.log(target.foo2); // 3，说明第二个源对象的第一个属性也拷贝成功了。
console.log(target.foo);  // 1，只读属性不能被覆盖，所以第二个源对象的第二个属性拷贝失败了。
console.log(target.foo3); // undefined，异常之后 assign 方法就退出了，第三个属性是不会被拷贝到的。
console.log(target.baz);  // undefined，第三个源对象更是不会被拷贝到的。
拷贝访问器
const obj = {
  foo: 1,
  get bar() {
    return 2;
  }
};
let copy = Object.assign({}, obj); 
console.log(copy); // { foo: 1, bar: 2 } copy.bar的值来自obj.bar的getter函数的返回值
// 下面这个函数会拷贝所有自有属性的属性描述符
function completeAssign(target, ...sources) {
  sources.forEach(source => {
    let descriptors = Object.keys(source).reduce((descriptors, key) => {
      descriptors[key] = Object.getOwnPropertyDescriptor(source, key);
      return descriptors;
    }, {});

    // Object.assign 默认也会拷贝可枚举的Symbols
    Object.getOwnPropertySymbols(source).forEach(sym => {
      let descriptor = Object.getOwnPropertyDescriptor(source, sym);
      if (descriptor.enumerable) {
        descriptors[sym] = descriptor;
      }
    });
    Object.defineProperties(target, descriptors);
  });
  return target;
}
copy = completeAssign({}, obj);
console.log(copy);
// { foo:1, get bar() { return 2 } }
``` 

#### 2. Object.create() 

Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 （请打开浏览器控制台以查看运行结果。）

<b>语法:</b>
> Object.create(proto, [propertiesObject])

<b>参数:</b>
<br>proto：必须。表示新建对象的原型对象，即该参数会被赋值到目标对象(即新对象，或说是最后返回的对象)的原型上。该参数可以是null， 对象， 函数的prototype属性 （创建空的对象时需传null , 否则会抛出TypeError异常）。

propertiesObject : 可选。 添加到新创建对象的可枚举属性（即其自身的属性，而不是原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应Object.defineProperties()的第二个参数。

<b>返回值:</b>
<br>一个新对象，带着指定的原型对象和属性。

注： 如果propertiesObject参数是 null 或非原始包装对象，则抛出一个 TypeError 异常。

<b>demo:</b>
```
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};
const me = Object.create(person);
me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten
me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```
更多示例：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create

#### 3. Object.defineProperty() 

Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

备注：应当直接在 Object 构造器对象上调用此方法，而不是在任意一个 Object 类型的实例上调用。

<b>语法:</b>
> Object.defineProperty(obj, prop, descriptor)

<b>参数:</b>
<br>obj：要定义属性的对象。
<br>prop：要定义或修改的属性的名称或 Symbol 。
<br>descriptor：要定义或修改的属性描述符。

<b>返回值:</b>
<br>被传递给函数的对象。

在ES6中，由于 Symbol类型的特殊性，用Symbol类型的值来做对象的key与常规的定义或修改不同，而Object.defineProperty 是定义key为Symbol的属性的方法之一。

<b>描述:</b>

该方法允许精确地添加或修改对象的属性。通过赋值操作添加的普通属性是可枚举的，在枚举对象属性时会被枚举到（for...in 或 Object.keys 方法），可以改变这些属性的值，也可以删除这些属性。这个方法允许修改默认的额外选项（或配置）。默认情况下，使用 Object.defineProperty() 添加的属性值是不可修改（immutable）的。

对象里目前存在的属性描述符有两种主要形式：数据描述符和存取描述符。数据描述符是一个具有值的属性，该值可以是可写的，也可以是不可写的。存取描述符是由 getter 函数和 setter 函数所描述的属性。一个描述符只能是这两者其中之一；不能同时是两者。

这两种描述符都是对象。它们共享以下可选键值（默认值是指在使用 Object.defineProperty() 定义属性时的默认值）：

* configurable：当且仅当该属性的 configurable 键值为 true 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。
* enumerable：当且仅当该属性的 enumerable 键值为 true 时，该属性才会出现在对象的枚举属性中。默认为 false。

数据描述符还具有以下可选键值：

* value：该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
* writable：当且仅当该属性的 writable 键值为 true 时，属性的值，也就是上面的 value，才能被赋值运算符改变。默认为 false。

存取描述符还具有以下可选键值：

* get：属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。默认为 undefined。
* set：属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。
默认为 undefined。

描述符默认值汇总：
- 拥有布尔值的键 configurable、enumerable 和 writable 的默认值都是 false。
- 属性值和函数的键 value、get 和 set 字段的默认值为 undefined。

描述符可拥有的键值：

| |     configurable| enumerable| value |	writable	|get|	set
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
数据描述符|	   可以|    可以|     可以|     可以|	不可以|	   不可以|
存取描述符|	   可以|	可以|	不可以|	   不可以|     可以|     可以|

<b>demo:</b>
```
const object1 = {};
Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});
object1.property1 = 77;
// throws an error in strict mode
console.log(object1.property1);
// expected output: 42
```
更多示例：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

#### 4. Object.defineProperties()

Object.defineProperties() 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

<b>语法:</b>
> Object.defineProperties(obj, props)

<b>参数:</b>
<br>obj：在其上定义或修改属性的对象。

props: 要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅Object.defineProperty()）。描述符具有以下键：

+ configurable：true 当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。默认为 false
+ enumerable：true 当且仅当在枚举相应对象上的属性时该属性显现。默认为 false
+ value：与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。默认为 undefined.
+ writable：true当且仅当与该属性相关联的值可以用assignment operator改变时。默认为 false
+ get：作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。默认为 undefined
+ set：作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。默认为 undefined

<b>返回值:</b>
<br>传递给函数的对象。

<b>描述:</b>
<br>Object.defineProperties本质上定义了obj 对象上props的可枚举属性相对应的所有属性。

</b>demo:</b>
```
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
// {property1: true, property2: "Hello"}
```

#### 5. Object.entries() 

Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。

<b>语法:</b>
<br>Object.entries(obj)

<b>参数:</b>
<br>obj: 可以返回其可枚举属性的键值对的对象。

<b>返回值:</b>
<br>给定对象自身可枚举属性的键值对数组。

<b>描述:</b>
<br>Object.entries()返回一个数组，其元素是与直接在object上找到的可枚举属性键值对相对应的数组。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

<b>demo:</b>
```
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
```

```
// array like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]
```

```
// array like object with random key ordering
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
```

```
// getFoo is property which isn't enumerable
const myObj = Object.create({}, { getFoo: { value() { return this.foo; } } });
myObj.foo = 'bar';
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]
```

```
// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]

// iterate through key-value gracefully
const obj = { a: 5, b: 7, c: 9 };
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}
```

```
// Or, using array extras
Object.entries(obj).forEach(([key, value]) => {
console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
});
```
* 将Object转换为Map

new Map() 构造函数接受一个可迭代的entries。借助Object.entries方法你可以很容易的将Object转换为Map:
```
var obj = { foo: "bar", baz: 42 }; 
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }
```

#### 6. Object.freeze() 

Object.freeze() 方法可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。freeze() 返回和传入的参数相同的对象。

<b>语法:</b>
<br>Object.freeze(obj)

<b>参数:</b>
<br>obj: 要被冻结的对象。

<b>返回值:</b>
<br>被冻结的对象。

<b>描述:</b>

被冻结对象自身的所有属性都不可能以任何方式被修改。任何修改尝试都会失败，无论是静默地还是通过抛出TypeError异常（最常见但不仅限于strict mode）。

数据属性的值不可更改，访问器属性（有getter和setter）也同样（但由于是函数调用，给人的错觉是还是可以修改这个属性）。如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。数组作为一种对象，被冻结，其元素不能被修改。没有数组元素可以被添加或移除。

这个方法返回传递的对象，而不是创建一个被冻结的副本。

<b>demo:</b>
```
const obj = {
  prop: 42
};
Object.freeze(obj);
obj.prop = 33;
// Throws an error in strict mode
console.log(obj.prop);
// expected output: 42
```

更多示例：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze

#### 7. Object.getOwnPropertyDescriptor() 

Object.getOwnPropertyDescriptor() 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

<b>语法:</b>
> Object.getOwnPropertyDescriptor(obj, prop)

<b>参数:</b>
<br>obj: 需要查找的目标对象
<br>prop: 目标对象内属性名称

<b>返回值:</b>
<br>如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。

<b>描述:</b>

该方法允许对一个属性的描述进行检索。在 Javascript 中， 属性 由一个字符串类型的“名字”（name）和一个“属性描述符”（property descriptor）对象构成。更多关于属性描述符类型以及他们属性的信息可以查看：Object.defineProperty.

一个属性描述符是一个记录，由下面属性当中的某些组成的：
* value: 该属性的值(仅针对数据属性描述符有效)
* writable: 当且仅当属性的值可以被改变时为true。(仅针对数据属性描述有效)
* get: 获取该属性的访问器函数（getter）。如果没有访问器， 该值为undefined。(仅针对包含访问器或设置器的属性描述有效)
* set: 获取该属性的设置器函数（setter）。 如果没有设置器， 该值为undefined。(仅针对包含访问器或设置器的属性描述有效)
* configurable: 当且仅当指定对象的属性描述可以被改变或者属性可被删除时，为true。
* enumerable: 当且仅当指定对象的属性可以被枚举出时，为 true。

<b>demo:</b>
```
var o, d;
o = { get foo() { return 17; } };
d = Object.getOwnPropertyDescriptor(o, "foo");
// d {
//   configurable: true,
//   enumerable: true,
//   get: /*the getter function*/,
//   set: undefined
// }
o = { bar: 42 };
d = Object.getOwnPropertyDescriptor(o, "bar");
// d {
//   configurable: true,
//   enumerable: true,
//   value: 42,
//   writable: true
// }
o = {};
Object.defineProperty(o, "baz", {
  value: 8675309,
  writable: false,
  enumerable: false
});
d = Object.getOwnPropertyDescriptor(o, "baz");
// d {
//   value: 8675309,
//   writable: false,
//   enumerable: false,
//   configurable: false
// }
```

#### 8. Object.getOwnPropertyNames() 

Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。

<b>语法:</b>
> Object.getOwnPropertyNames(obj)

<b>参数:</b>
<br>obj: 一个对象，其自身的可枚举和不可枚举属性的名称被返回。

<b>返回值:</b>
<br>在给定对象上找到的自身属性对应的字符串数组。

<b>描述:</b>
<br>Object.getOwnPropertyNames() 返回一个数组，该数组对元素是 obj自身拥有的枚举或不可枚举属性名称字符串。 数组中枚举属性的顺序与通过 for...in 循环（或 Object.keys）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义。

<b>demo:</b>
```
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]
// 类数组对象
var obj = { 0: "a", 1: "b", 2: "c"};
console.log(Object.getOwnPropertyNames(obj).sort()); // ["0", "1", "2"]
// 使用Array.forEach输出属性名和属性值
Object.getOwnPropertyNames(obj).forEach(function(val, idx, array) {
  console.log(val + " -> " + obj[val]);
});
// 输出
// 0 -> a
// 1 -> b
// 2 -> c
//不可枚举属性
var my_obj = Object.create({}, {
  getFoo: {
    value: function() { return this.foo; },
    enumerable: false
  }
});
my_obj.foo = 1;
console.log(Object.getOwnPropertyNames(my_obj).sort()); // ["foo", "getFoo"]
```

#### 9. Object.getOwnPropertySymbols() 

Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。

<b>语法:</b>
> Object.getOwnPropertySymbols(obj)

<b>参数:</b>
<br>obj: 要返回 Symbol 属性的对象。

<b>返回值:</b>
<br>在给定对象自身上找到的所有 Symbol 属性的数组。

<b>描述:</b>

与Object.getOwnPropertyNames()类似，您可以将给定对象的所有符号属性作为 Symbol 数组获取。 请注意，Object.getOwnPropertyNames()本身不包含对象的 Symbol 属性，只包含字符串属性。

因为所有的对象在初始化的时候不会包含任何的 Symbol，除非你在对象上赋值了 Symbol 否则Object.getOwnPropertySymbols()只会返回一个空的数组。

<b>demo:</b>
```
var obj = {};
var a = Symbol("a");
var b = Symbol.for("b");
obj[a] = "localSymbol";
obj[b] = "globalSymbol";
var objectSymbols = Object.getOwnPropertySymbols(obj);
console.log(objectSymbols.length); // 2
console.log(objectSymbols)         // [Symbol(a), Symbol(b)]
console.log(objectSymbols[0])      // Symbol(a)
```

#### 10. Object.getPrototypeOf() 

Object.getPrototypeOf() 方法返回指定对象的原型（内部[[Prototype]]属性的值）。

<b>语法:</b>
> Object.getPrototypeOf(object)

<b>参数:</b>
<br>obj: 要返回其原型的对象。

<b>返回值:</b>
<br>给定对象的原型。如果没有继承属性，则返回 null 。

<b>demo:</b>
```
const prototype1 = {};
const object1 = Object.create(prototype1);
console.log(Object.getPrototypeOf(object1) === prototype1);
// expected output: true
```
```
var proto = {};
var obj = Object.create(proto);
Object.getPrototypeOf(obj) === proto; // true
var reg = /a/;
Object.getPrototypeOf(reg) === RegExp.prototype; // true
```

    Object.getPrototypeOf(Object)  不是  Object.prototype

    JavaScript中的 Object 是构造函数（创建对象的包装器）。
    一般用法是：
        var obj = new Object();

    所以：
        Object.getPrototypeOf( Object );               // ƒ () { [native code] }
        Object.getPrototypeOf( Function );             // ƒ () { [native code] }

        Object.getPrototypeOf( Object ) === Function.prototype;        // true

        Object.getPrototypeOf( Object )是把Object这一构造函数看作对象，
    返回的当然是函数对象的原型，也就是 Function.prototype。

    正确的方法是，Object.prototype是构造出来的对象的原型。
        var obj = new Object();
        Object.prototype === Object.getPrototypeOf( obj );              // true

        Object.prototype === Object.getPrototypeOf( {} );               // true

#### 11. Object.is() 

Object.is() 方法判断两个值是否是相同的值。

<b>语法:</b>
> Object.is(value1, value2);

<b>参数:</b>
<br>value1: 第一个需要比较的值。
<br>value2: 第二个需要比较的值。

<b>返回值:</b>
<br>表示两个参数是否相同的布尔值 。

<b>描述:</b>

Object.is() 判断两个值是否相同。如果下列任何一项成立，则两个值相同：

- 两个值都是 undefined
- 两个值都是 null
- 两个值都是 true 或者都是 false
- 两个值是由相同个数的字符按照相同的顺序组成的字符串
- 两个值指向同一个对象
- 两个值都是数字并且
    - 都是正零 +0
    - 都是负零 -0
    - 都是 NaN
    - 都是除零和 NaN 外的其它同一个数字

这种相等性判断逻辑和传统的 == 运算不同，== 运算符会对它两边的操作数做隐式类型转换（如果它们类型不同），然后才进行相等性比较，（所以才会有类似 "" == false 等于 true 的现象），但 Object.is 不会做这种类型转换。

这与 === 运算符的判定方式也不一样。=== 运算符（和== 运算符）将数字值 -0 和 +0 视为相等，并认为 Number.NaN 不等于 NaN。

<b>demo:</b>
```
Object.is('foo', 'foo');     // true
Object.is(window, window);   // true
Object.is('foo', 'bar');     // false
Object.is([], []);           // false
```
```
var foo = { a: 1 };
var bar = { a: 1 };
Object.is(foo, foo);         // true
Object.is(foo, bar);         // false
Object.is(null, null);       // true
```
```
// 特例
Object.is(0, -0);            // false
Object.is(0, +0);            // true
Object.is(-0, -0);           // true
Object.is(NaN, 0/0);         // true
```

#### 12. Object.isExtensible() 

Object.isExtensible() 方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。

<b>语法:</b>
> Object.isExtensible(obj)

<b>参数:</b>
<br>obj: 需要检测的对象

<b>返回值:</b>
<br>表示给定对象是否可扩展的一个Boolean 。

<b>描述:</b>

默认情况下，对象是可扩展的：即可以为他们添加新的属性。以及它们的 __proto__ 属性可以被更改。Object.preventExtensions，Object.seal 或 Object.freeze 方法都可以标记一个对象为不可扩展（non-extensible）。

<b>demo:</b>
```
// 新对象默认是可扩展的.
var empty = {};
Object.isExtensible(empty); // === true
```
```
// ...可以变的不可扩展.
Object.preventExtensions(empty);
Object.isExtensible(empty); // === false
```
```
// 密封对象是不可扩展的.
var sealed = Object.seal({});
Object.isExtensible(sealed); // === false
```
```
// 冻结对象也是不可扩展.
var frozen = Object.freeze({});
Object.isExtensible(frozen); // === false
```

#### 13. Object.isFrozen()

Object.isFrozen()方法判断一个对象是否被冻结。

<b>语法:</b>
> Object.isFrozen(obj)

<b>参数:</b>
<br>obj: 被检测的对象。

<b>返回值:</b>
<br>表示给定对象是否被冻结的Boolean。

<b>描述:</b>

一个对象是冻结的是指它不可扩展，所有属性都是不可配置的，且所有数据属性（即没有getter或setter组件的访问器的属性）都是不可写的。

<b>demo:</b>
```
// 一个对象默认是可扩展的,所以它也是非冻结的.
Object.isFrozen({}); // === false
```
```
// 一个不可扩展的空对象同时也是一个冻结对象.
var vacuouslyFrozen = Object.preventExtensions({});
Object.isFrozen(vacuouslyFrozen) //=== true;
```
```
// 一个非空对象默认也是非冻结的.
var oneProp = { p: 42 };
Object.isFrozen(oneProp) //=== false
```
```
// 让这个对象变的不可扩展,并不意味着这个对象变成了冻结对象,
// 因为p属性仍然是可以配置的(而且可写的).
Object.preventExtensions(oneProp);
Object.isFrozen(oneProp) //=== false
```
```
// 此时,如果删除了这个属性,则它会成为一个冻结对象.
delete oneProp.p;
Object.isFrozen(oneProp) //=== true
```
```
// 一个不可扩展的对象,拥有一个不可写但可配置的属性,则它仍然是非冻结的.
var nonWritable = { e: "plep" };
Object.preventExtensions(nonWritable);
Object.defineProperty(nonWritable, "e", { writable: false }); // 变得不可写
Object.isFrozen(nonWritable) //=== false
```
```
// 把这个属性改为不可配置,会让这个对象成为冻结对象.
Object.defineProperty(nonWritable, "e", { configurable: false }); // 变得不可配置
Object.isFrozen(nonWritable) //=== true
```
```
// 一个不可扩展的对象,拥有一个不可配置但可写的属性,则它仍然是非冻结的.
var nonConfigurable = { release: "the kraken!" };
Object.preventExtensions(nonConfigurable);
Object.defineProperty(nonConfigurable, "release", { configurable: false });
Object.isFrozen(nonConfigurable) //=== false
```
```
// 把这个属性改为不可写,会让这个对象成为冻结对象.
Object.defineProperty(nonConfigurable, "release", { writable: false });
Object.isFrozen(nonConfigurable) //=== true
```
```
// 一个不可扩展的对象,值拥有一个访问器属性,则它仍然是非冻结的.
var accessor = { get food() { return "yum"; } };
Object.preventExtensions(accessor);
Object.isFrozen(accessor) //=== false
```
```
// ...但把这个属性改为不可配置,会让这个对象成为冻结对象.
Object.defineProperty(accessor, "food", { configurable: false });
Object.isFrozen(accessor) //=== true
```
```
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true
```
```
// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen) //=== true
```
```
// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen) //=== false
```

#### 14. Object.isSealed()

Object.isSealed() 方法判断一个对象是否被密封。

<b>语法:</b>
> Object.isSealed(obj)

<b>参数:</b>
<br>obj: 要被检查的对象。

<b>返回值:</b>
<br>表示给定对象是否被密封的一个Boolean 。

<b>描述:</b>

如果这个对象是密封的，则返回 true，否则返回 false。密封对象是指那些不可 扩展 的，且所有自身属性都不可配置且因此不可删除（但不一定是不可写）的对象。

<b>demo:</b>
```
// 新建的对象默认不是密封的.
var empty = {};
Object.isSealed(empty); // === false
```
```
// 如果你把一个空对象变的不可扩展,则它同时也会变成个密封对象.
Object.preventExtensions(empty);
Object.isSealed(empty); // === true
```
```
// 但如果这个对象不是空对象,则它不会变成密封对象,因为密封对象的所有自身属性必须是不可配置的.
var hasProp = { fee: "fie foe fum" };
Object.preventExtensions(hasProp);
Object.isSealed(hasProp); // === false
```
```
// 如果把这个属性变的不可配置,则这个对象也就成了密封对象.
Object.defineProperty(hasProp, "fee", { configurable: false });
Object.isSealed(hasProp); // === true
```
```
// 最简单的方法来生成一个密封对象,当然是使用Object.seal.
var sealed = {};
Object.seal(sealed);
Object.isSealed(sealed); // === true
```
```
// 一个密封对象同时也是不可扩展的.
Object.isExtensible(sealed); // === false
```
```
// 一个密封对象也可以是一个冻结对象,但不是必须的.
Object.isFrozen(sealed); // === true ，所有的属性都是不可写的
var s2 = Object.seal({ p: 3 });
Object.isFrozen(s2); // === false， 属性"p"可写
```
```
var s3 = Object.seal({ get p() { return 0; } });
Object.isFrozen(s3); // === true ，访问器属性不考虑可写不可写,只考虑是否可配置
```

#### 15. Object.keys() 

Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

<b>语法:</b>
> Object.keys(obj)

<b>参数:</b>
<br>obj: 要返回其枚举自身属性的对象。

<b>返回值:</b>
<br>一个表示给定对象的所有可枚举属性的字符串数组。

<b>描述:</b>
<br>Object.keys 返回一个所有元素为字符串的数组，其元素来自于从给定的object上面可直接枚举的属性。这些属性的顺序与手动遍历该对象属性时的一致。

<b>demo:</b>
```
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']
```
```
// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']
```
```
// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']
```
```
// getFoo is a property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  } 
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```
如果你想获取一个对象的所有属性,，甚至包括不可枚举的，请查看Object.getOwnPropertyNames。

#### 16. Object.preventExtensios() 

Object.preventExtensions()方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。

<b>语法:</b>
> Object.preventExtensions(obj)

<b>参数:</b>
<br>obj: 将要变得不可扩展的对象。

<b>返回值:</b>
<br>已经不可扩展的对象。

<b>描述:</b>

如果一个对象可以添加新的属性，则这个对象是可扩展的。Object.preventExtensions()将对象标记为不再可扩展，这样它将永远不会具有它被标记为不可扩展时持有的属性之外的属性。注意，一般来说，不可扩展对象的属性可能仍然可被删除。尝试将新属性添加到不可扩展对象将静默失败或抛出TypeError（最常见的情况是strict mode中，但不排除其他情况）。

Object.preventExtensions()仅阻止添加自身的属性。但其对象类型的原型依然可以添加新的属性。

该方法使得目标对象的 [[prototype]]  不可变；任何重新赋值 [[prototype]] 操作都会抛出 TypeError 。这种行为只针对内部的 [[prototype]] 属性， 目标对象的其它属性将保持可变。

一旦将对象变为不可扩展的对象，就再也不能使其可扩展。

<b>demo:</b>
```
const object1 = {};
Object.preventExtensions(object1);
try {
  Object.defineProperty(object1, 'property1', {
    value: 42
  });
} catch (e) {
  console.log(e);
  // Expected output: TypeError: Cannot define property property1, object is not extensible
}
```
```
// Object.preventExtensions将原对象变的不可扩展,并且返回原对象.
var obj = {};
var obj2 = Object.preventExtensions(obj);
obj === obj2;  // true
```
```
// 字面量方式定义的对象默认是可扩展的.
var empty = {};
Object.isExtensible(empty) //=== true
// ...但可以改变.
Object.preventExtensions(empty);
Object.isExtensible(empty) //=== false
```
```
// 使用Object.defineProperty方法为一个不可扩展的对象添加新属性会抛出异常.
var nonExtensible = { removable: true };
Object.preventExtensions(nonExtensible);
Object.defineProperty(nonExtensible, "new", { value: 8675309 }); // 抛出TypeError异常
```
```
// 在严格模式中,为一个不可扩展对象的新属性赋值会抛出TypeError异常.
function fail()
{
  "use strict";
  nonExtensible.newProperty = "FAIL"; // throws a TypeError
}
fail();
```
不可扩展对象的原型是不可变的：
```
var fixed = Object.preventExtensions({}); 
// throws a 'TypeError'.
fixed.__proto__ = { oh: 'hai' };
```

#### 17. Object.seal() 

Object.seal()方法封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要原来是可写的就可以改变。

<b>语法:</b>
> Object.seal(obj)

<b>参数:</b>
<br>obj: 将要被密封的对象。

<b>返回值:</b>
<br>被密封的对象。

<b>描述:</b>

通常，一个对象是可扩展的（可以添加新的属性）。密封一个对象会让这个对象变的不能添加新属性，且所有已有属性会变的不可配置。属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但属性的值仍然可以修改。尝试删除一个密封对象的属性或者将某个密封对象的属性从数据属性转换成访问器属性，结果会静默失败或抛出TypeError（在严格模式 中最常见的，但不唯一）。

不会影响从原型链上继承的属性。但 __proto__ (  ) 属性的值也会不能修改。

返回被密封对象的引用。

<b>demo:</b>
```
const object1 = {
  property1: 42
};
Object.seal(object1);
object1.property1 = 33;
console.log(object1.property1);
// expected output: 33
delete object1.property1; // cannot delete when sealed
console.log(object1.property1);
// expected output: 33
```

更多示例：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/seal

#### 18. Object.setPrototypeOf() 

Object.setPrototypeOf() 方法设置一个指定的对象的原型 ( 即, 内部[[Prototype]]属性）到另一个对象或  null。

    警告: 由于现代 JavaScript 引擎优化属性访问所带来的特性的关系，更改对象的 [[Prototype]]在各个浏览器和 JavaScript 引擎上都是一个很慢的操作。其在更改继承的性能上的影响是微妙而又广泛的，这不仅仅限于 obj.__proto__ = ... 语句上的时间花费，而且可能会延伸到任何代码，那些可以访问任何[[Prototype]]已被更改的对象的代码。如果你关心性能，你应该避免设置一个对象的 [[Prototype]]。相反，你应该使用 Object.create()来创建带有你想要的[[Prototype]]的新对象。

<b>语法:</b>
> Object.setPrototypeOf(obj, prototype)

<b>参数:</b>
<br>obj: 要设置其原型的对象。

<b>返回值:</b>
<br>该对象的新原型(一个对象 或 null).

<b>描述:</b>

如果对象的[[Prototype]]被修改成不可扩展(通过 Object.isExtensible()查看)，就会抛出 TypeError异常。如果prototype参数不是一个对象或者null(例如，数字，字符串，boolean，或者 undefined)，则什么都不做。否则，该方法将obj的[[Prototype]]修改为新的值。

Object.setPrototypeOf()是ECMAScript 6最新草案中的方法，相对于 Object.prototype.__proto__ ，它被认为是修改对象原型更合适的方法

<b>demo:</b>
```
var dict = Object.setPrototypeOf({}, null);
```

<b>附加原型链</b>
<br>通过  Object.getPrototypeOf() 和 Object.prototype.__proto__ 的组合允许将一个原型链完整的附加到一个新的原型对象上：

```
Object.appendChain = function(oChain, oProto) {
  if (arguments.length < 2) { 
    throw new TypeError('Object.appendChain - Not enough arguments');
  }
  if (typeof oProto === 'number' || typeof oProto === 'boolean') {
    throw new TypeError('second argument to Object.appendChain must be an object or a string');
  }

  var oNewProto = oProto,
      oReturn, 
      o2nd, 
      oLast;
      
  oReturn = o2nd = oLast = oChain instanceof this ? oChain : new oChain.constructor(oChain);

  for (var o1st = this.getPrototypeOf(o2nd);
    o1st !== Object.prototype && o1st !== Function.prototype;
    o1st = this.getPrototypeOf(o2nd)
  ) {
    o2nd = o1st;
  }

  if (oProto.constructor === String) {
    oNewProto = Function.prototype;
    oReturn = Function.apply(null, Array.prototype.slice.call(arguments, 1));
    this.setPrototypeOf(oReturn, oLast);
  }

  this.setPrototypeOf(o2nd, oNewProto);
  return oReturn;
}
```

例子一：向一个原型附加一个链
```
function Mammal() {
  this.isMammal = 'yes';
}

function MammalSpecies(sMammalSpecies) {
  this.species = sMammalSpecies;
}

MammalSpecies.prototype = new Mammal();
MammalSpecies.prototype.constructor = MammalSpecies;

var oCat = new MammalSpecies('Felis');

console.log(oCat.isMammal); 
// 'yes'

function Animal() {
  this.breathing = 'yes';
}

Object.appendChain(oCat, new Animal());

console.log(oCat.breathing); 
// 'yes'
```

例子二：将一个基本类型转化为对应的对象类型并添加到原型链上
```
function Symbol() {
  this.isSymbol = 'yes';
}

var nPrime = 17;

console.log(typeof nPrime); // 'number'

var oPrime = Object.appendChain(nPrime, new Symbol());

console.log(oPrime); // '17'
console.log(oPrime.isSymbol); // 'yes'
console.log(typeof oPrime); // 'object'
```

例子三：给函数类型的对象添加一个链，并添加一个新的方法到那个链上
```
function Person(sName) {
  this.identity = sName;
}

var george = Object.appendChain(new Person('George'), 'console.log("Hello guys!!");');

console.log(george.identity); // 'George'
george(); // 'Hello guys!!'
```

#### 19. Object.values() 

Object.values()方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

<b>语法:</b>
> Object.values(obj)

<b>参数:</b>
<br>obj: 被返回可枚举属性值的对象。

<b>返回值:</b>
<br>一个包含对象自身的所有可枚举属性值的数组。

<b>描述:</b>
<br> Object.values()返回一个数组，其元素是在对象上找到的可枚举属性值。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

<b>demo:</b>
```
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]
```
```
// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']
```
```
// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']
```
```
// getFoo is property which isn't enumerable
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']
```
```
// non-object argument will be coerced to an object
console.log(Object.values('foo')); // ['f', 'o', 'o']
```

#### 20. Object.fromEntries() 

Object.fromEntries()方法是Object.entries()的逆操作，用于将一个键值对数组转为对象。

<b>语法:</b>
> Object.values(obj)

<b>参数:</b>
<br>obj: 键值对数组。

<b>返回值:</b>
<br>对象。

<b>描述:</b>

+ 将键值对的数据结构还原为对象，因此特别适合将 Map 结构转为对象。
+ 配合URLSearchParams对象，将查询字符串转为对象。

<b>demo:</b>

```
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
])
// { foo: "bar", baz: 42 }
```

```
// 例一
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

Object.fromEntries(entries)
// { foo: "bar", baz: 42 }

// 例二
const map = new Map().set('foo', true).set('bar', false);
Object.fromEntries(map)
// { foo: true, bar: false }
```

```
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```