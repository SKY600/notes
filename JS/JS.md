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