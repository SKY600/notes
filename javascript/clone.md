实现一个函数clone，可以对JavaScript中的5种主要的数据类型（包括Number、String、Object、Array、Boolean）进行值复制。
```
Object.prototype.clone = function () {
    var o = this.constructor === Array ? [] : {};
    for (var e in this) {
        o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
    }
    return o;
}
```
## 深拷贝与浅拷贝
### 原理
深拷贝：在堆中重新分配内存，拥有不同的地址，但值是一样的，复制后的对象与原来的对象是完全隔离，互不影响。

浅拷贝：仅仅复制了引用（地址），即复制了之后，原来的变量和新的变量指向同一个东西，彼此之间的操作会互相影响。

### 区别
复制的是引用(地址)还是复制的是实例。

深拷贝本身只针对较为复杂的object类型数据。

### 方法
###### 浅拷贝：slice、concat、extend

Array 的 slice 和 concat 方法 和 jQuery 中的 extend 复制方法，他们都会复制第一层的值，对于 第一层 的值都是 深拷贝，而到 第二层 的时候 Array 的 slice 和 concat 方法就是 复制引用 ，jQuery 中的 extend 复制方法 则 取决于 你的 第一个参数， 也就是是否进行递归复制。所谓第一层 就是 key 所对应的 value 值是基本数据类型。

```
const oldObj = { a: 1, b: 2, c: 3 };
const newObj = oldObj;
console.log(newObj); // { a: 1, b: 2, c: 3 }
oldObj.c = 4;
console.log(oldObj); // { a: 1, b: 2, c: 4 }
console.log(newObj); // { a: 1, b: 2, c: 4 }
```

###### 深拷贝（简单版）：JSON.parse()、JSON.stringify()

```
// 1.无法实现对函数、RegExp等特殊对象的克隆
// 2.会抛弃对象的constructor，所有的构造函数会指向Object
// 3.对象有循环引用会报错
const oldObj1 = { a: 1, b: 2, c: 3 };
const newObj1 = JSON.parse(JSON.stringify(oldObj1));
oldObj1.c = 4;
console.log(oldObj1); // { a: 1, b: 2, c: 4 }
console.log(newObj1); // { a: 1, b: 2, c: 3 }
```

###### 深拷贝（面试版）
```
const clone = parent => {
    // 判断类型
    const isType = (obj, type) => {
        if (obj !== "object") return false;
        const typeString = Object.prototype.toString.call(obj);
        let flag;
        switch (type) {
            case "Array":
                flag = typeString === "[object Array]";
            break;
            case "Date":
                flag = typeString === "[object Date]";
            break;
            case "RegExp":
                flag = typeString === "[object RegExp]";
            break;
            default:
                flag = false;
        }
        return flag;
    }

    // 处理正则 相当于getRegExp(re)
    const getRegExp = re => {
        var flags = "";
        if (re.global) flags += "g";
        if (re.ignoreCase) flags += "i";
        if (re.multiline) flags += "m";
        return flags;
    };

    // 维护两个储存循环引用的数组
    const parents = [];
    const children = [];

        // 处理正则 相当于getRegExp(re)
    const getRegExp = re => {
        var flags = "";
        if (re.global) flags += "g";
        if (re.ignoreCase) flags += "i";
        if (re.multiline) flags += "m";
        return flags;
    };
    // 维护两个储存循环引用的数组
    const parents = [];
    const children = [];

    const _clone = parent => {
        if (parent === null) return null;
        if (typeof parent !== "object") return parent;

        let child, proto;    

        if (isType(parent, "Array")) {
            // 对数组做特殊处理
            child = [];
        } else if (isType(parent, "RegExp")) {
            // 对正则对象做特殊处理
            child = new RegExp(parent.source, getRegExp(parent));
            if (parent.lastIndex) child.lastIndex = parent.lastIndex;
        } else if (isType(parent, "Date")) {
            // 对Date对象做特殊处理
            child = new Date(parent.getTime());
        } else {
            // 处理对象原型
            proto = Object.getPrototypeOf(parent);
            // 利用Object.create切断原型链
            child = Object.create(proto);
        }

        // 处理循环引用
        const index = parents.indexOf(parent);

        if (index != -1) {
            // 如果父数组存在本对象,说明之前已经被引用过,直接返回此对象
            return children[index];
        }
        parents.push(parent);
        children.push(child);

        for (let i in parent) {
            // 递归
            child[i] = _clone(parent[i]);
        }

        return child;
    };
    return _clone(parent);
}

const regStr = /runoob/i;
const newRegStr = clone(regStr);
console.log(clone(regStr).toSource());
```