### instanceof的原理及实现：
```
function instance_of(L, R) {
    var O = R.prototype; // 取R的显示原型
    L = L.__proto__;
    while (true) {
        if (L === null) return false;
        if (O === L)
        // 这里重点：当O严格等于L时，返回true
        return true;
        L = L.__proto__;
    }
}
```

### 作用：
示例： a instanceof B

> 检测a的原型链（__proto__）上是否有B.prototype，若有返回true，否则false。

### 和typeof的区别：
* typeof只能返回 number string boolean object function undefined，但是对于对象{}、数组[]、null都会返回object
* 为了弥补这一点，instanceof从原型的角度，来判断某引用属于哪个构造函数，从而判定它的数据类型

### 使用
```
function Fn() {
    // 每个函数function都有一个prototype，即显式原型（属性），默认指向一个空的object对象。
    console.log('Fn.prototype: ', Fn.prototype);
}
Fn();
```
```
//每个实例对象都有一个__ptoro__，称为隐式原型。
var fn = new Fn();
console.log('fn.__proto__: ', fn.__proto__);
//对象的隐式原型的值为其对应构造函数显式原型的值。
console.log(Fn.prototype === fn.__proto__); //true

console.log('测试：', instance_of(fn, Fn)); // true
console.log('测试 1：', fn instanceof Fn); // true

function fn1() {

}
console.log('测试：', instance_of(fn1, Fn));//false

console.log('typeof ("123"): ', typeof ('123')); // string
console.log('typeof (123): ', typeof (123)); // number
console.log('typeof (0): ', typeof (0)); // number
console.log('typeof (true): ', typeof (true)); // boolean
console.log('typeof (f2): ', typeof (f2)); // function
console.log('typeof (undefined): ', typeof (undefined)); // undefined
console.log('typeof (null): ', typeof (null)); // object
console.log('typeof ({ a: "1" }): ', typeof ({ a: '1' })); // object
console.log('typeof ([1, 2, 3]): ', typeof ([1, 2, 3])); // object
console.log('null instanceof Object: ', null instanceof Object); // false
console.log('{ a: "1" } instanceof Object: ', { a: '1' } instanceof Object); // true
console.log('[1, 2, 3] instanceof Array: ', [1, 2, 3] instanceof Array); // true
console.log('[1, 2, 3] instanceof Object: ', [1, 2, 3] instanceof Object); // true
console.log('instance_of([1, 2, 3], Array): ', instance_of([1, 2, 3], Array)); // true
```
