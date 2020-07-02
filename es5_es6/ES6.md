### ES6常用方法

#### 数值的扩展方法
    Number.isFinite(), Number.isNaN()
    Number.parseInt(), Number.parseFloat()
    Number.isInteger()
    Number.EPSILON
    安全整数和 Number.isSafeInteger()
    Math 对象的扩展: 
        Math.trunc() 
        Math.sign() 
        Math.cbrt() 
        Math.clz32() 
        Math.imul() 
        Math.fround() 
        Math.hypot()
    对数方法:
        Math.expm1()
        Math.log1p()
        Math.log10()
        Math.log2()
    双曲函数方法: 
        Math.sinh(x) 
        Math.cosh(x) 
        Math.tanh(x) 
        Math.asinh(x) 
        Math.acosh(x) 
        Math.atanh(x) 
    指数运算符
    BigInt


#### 字符串的新增方法
    String.fromCodePoint()
    String.raw()
    实例方法：codePointAt()
    实例方法：normalize()
    实例方法：includes(), startsWith(), endsWith()
    实例方法：repeat()
    实例方法：padStart()，padEnd()
    实例方法：trimStart()，trimEnd()
    实例方法：matchAll()

#### 数组的扩展
    扩展运算符
    Array.from()
    Array.of()
    数组实例的 copyWithin()
    数组实例的 find() 和 findIndex()
    数组实例的 fill()
    数组实例的 entries()，keys() 和 values()
    数组实例的 includes()
    数组实例的 flat()，flatMap()
    数组的空位
    Array.prototype.sort() 的排序稳定性

#### Number.isFinite(), Number.isNaN() 
> Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。

注意，如果参数类型不是数值，Number.isFinite一律返回false。

```
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
```

> Number.isNaN()用来检查一个值是否为NaN。

```
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9/NaN) // true
Number.isNaN('true' / 0) // true
Number.isNaN('true' / 'true') // true
```

传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。

#### Number.parseInt(), Number.parseFloat()
ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。

```
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

```
Number.parseInt === parseInt // true
Number.parseFloat === parseFloat // true
```

#### Number.isInteger()
> Number.isInteger()用来判断一个数值是否为整数。

```
Number.isInteger(25) // true
Number.isInteger(25.1) // false
```

JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。

```
Number.isInteger(25) // true
Number.isInteger(25.0) // true
```

如果参数不是数值，Number.isInteger返回false。

```
Number.isInteger() // false
Number.isInteger(null) // false
Number.isInteger('15') // false
Number.isInteger(true) // false
```

误判的情况：
```
Number.isInteger(3.0000000000000002) // true  
// 小数的精度达到了小数点后16个十进制位，转成二进制位超过了53个二进制位，导致最后的那个2被丢弃了。

Number.isInteger(5E-324) // false
// 如果一个数值的绝对值小于Number.MIN_VALUE（5E-324），即小于 JavaScript 能够分辨的最小值，会被自动转为 0。这时，Number.isInteger也会误判。

Number.isInteger(5E-325) // true
// 5E-325由于值太小，会被自动转为0，因此返回true。
```

总之，如果对数据精度的要求较高，不建议使用Number.isInteger()判断一个数值是否为整数。


#### Number.EPSILON
> 表示 1 与大于 1 的最小浮点数之间的差。

#### Number.isSafeInteger()
> Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。

#### Math.trunc() 
> 用于去除一个数的小数部分，返回整数部分。

#### Math.sign() 
> 用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

#### Math.cbrt() 
> 用于计算一个数的立方根。

#### Math.clz32() 
> 将参数转为 32 位无符号整数的形式，然后返回这个 32 位值里面有多少个前导 0。

#### Math.imul() 
> 返回两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数。

#### Math.fround() 
> 返回一个数的32位单精度浮点数形式。
- 主要作用，是将64位双精度浮点数转为32位单精度浮点数。如果小数的精度超过24个二进制位，返回值就会不同于原值，否则返回值不变（即与64位双精度值一致）。
- 对于 NaN 和 Infinity，此方法返回原值。对于其它类型的非数值，Math.fround 方法会先将其转为数值，再返回单精度浮点数。

#### Math.hypot() 
> 返回所有参数的平方和的平方根。
- Math.hypot(3, 4);

#### Math.expm1()
> Math.expm1(x)返回 ex - 1，即Math.exp(x) - 1。

#### Math.log1p()
> Math.log1p(x)方法返回1 + x的自然对数，即Math.log(1 + x)。如果x小于-1，返回NaN。

#### Math.log10()
> Math.log10(x)返回以 10 为底的x的对数。如果x小于 0，则返回 NaN

#### Math.log2()
> Math.log2(x)返回以 2 为底的x的对数。如果x小于 0，则返回 NaN。

#### Math.sinh(x) 
> 返回x的双曲正弦（hyperbolic sine）

#### Math.cosh(x) 
> 返回x的双曲余弦（hyperbolic cosine）

#### Math.tanh(x) 
> 返回x的双曲正切（hyperbolic tangent）

#### Math.asinh(x) 
> 返回x的反双曲正弦（inverse hyperbolic sine）

#### Math.acosh(x) 
> 返回x的反双曲余弦（inverse hyperbolic cosine）

#### Math.atanh(x) 
> 返回x的反双曲正切（inverse hyperbolic tangent）

#### 指数运算符（**）

```
2 ** 2 // 4
2 ** 3 // 8

// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2 // 512

let a = 1.5;
a **= 2;
// 等同于 a = a * a;

let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```

#### BigInt 数据类型

```
// 超过 53 个二进制位的数值，无法保持精度
Math.pow(2, 53) === Math.pow(2, 53) + 1 // true

// 超过 2 的 1024 次方的数值，无法表示
Math.pow(2, 1024) // Infinity
```

ES2020 引入了一种新的数据类型 BigInt（大整数），来解决这个问题。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

```
const a = 2172141653n;
const b = 15346349309n;

// BigInt 可以保持精度
a * b // 33334444555566667777n

// 普通整数无法保持精度
Number(a) * Number(b) // 33334444555566670000
```

为了与 Number 类型区别，BigInt 类型的数据必须添加后缀n。

```
1234 // 普通整数
1234n // BigInt

// BigInt 的运算
1n + 2n // 3n
```

BigInt 同样可以使用各种进制表示，都要加上后缀n。

```
0b1101n // 二进制
0o777n // 八进制
0xFFn // 十六进制
```

BigInt 与普通整数是两种值，它们之间并不相等。

```
42n === 42 // false
```

typeof运算符对于 BigInt 类型的数据返回bigint。

```
typeof 123n // 'bigint'
```

BigInt 可以使用负号（-），但是不能使用正号（+），因为会与 asm.js 冲突。

```
-42n // 正确
+42n // 报错
```

JavaScript 以前不能计算70的阶乘（即70!），因为超出了可以表示的精度。

```
let p = 1;
for (let i = 1; i <= 70; i++) {
  p *= i;
}
console.log(p); // 1.197857166996989e+100
```

现在支持大整数了，就可以算了，浏览器的开发者工具运行下面代码，就OK。

```
let p = 1n;
for (let i = 1n; i <= 70n; i++) {
  p *= i;
}
console.log(p); // 11978571...00000000n
```
#### BigInt 对象
用作构造函数生成 BigInt 类型的数值。转换规则基本与Number()一致，将其他类型的值转为 BigInt。

```
BigInt(123) // 123n
BigInt('123') // 123n
BigInt(false) // 0n
BigInt(true) // 1n
```

BigInt()构造函数必须有参数，而且参数必须可以正常转为数值，下面的用法都会报错。

```
new BigInt() // TypeError
BigInt(undefined) //TypeError
BigInt(null) // TypeError
BigInt('123n') // SyntaxError
BigInt('abc') // SyntaxError
// 上面代码中，尤其值得注意字符串123n无法解析成 Number 类型，所以会报错。

// 参数如果是小数，也会报错。
BigInt(1.5) // RangeError
BigInt('1.5') // SyntaxError
```

+ 继承了 Object 对象的两个实例方法。
    - BigInt.prototype.toString()
    - BigInt.prototype.valueOf()

+ 继承了 Number 对象的一个实例方法。
    - BigInt.prototype.toLocaleString()

+ 还提供了三个静态方法。
    - BigInt.asUintN(width, BigInt)： 给定的 BigInt 转为 0 到 2width - 1 之间对应的值。
    - BigInt.asIntN(width, BigInt)：给定的 BigInt 转为 -2width - 1 到 2width - 1 - 1 之间对应的值。
    - BigInt.parseInt(string[, radix])：近似于Number.parseInt()，将一个字符串转换成指定进制的 BigInt。

转换规则： 可以使用Boolean()、Number()和String()这三个方法，将 BigInt 可以转为布尔值、数值和字符串类型。
```
Boolean(0n) // false
Boolean(1n) // true
Number(1n)  // 1
String(1n)  // "1"
```

上面代码中，注意最后一个例子，转为字符串时后缀n会消失。

另外，取反运算符（!）也可以将 BigInt 转为布尔值。

```
!0n // true
!1n // false
```

#### String.fromCodePoint()
用于从 Unicode 码点返回对应字符，可以识别大于0xFFFF的字符，弥补了String.fromCharCode()方法的不足。在作用上，正好与下面的codePointAt()方法相反。
<br> String.fromCharCode() -- ES5
<br> 如果String.fromCodePoint方法有多个参数，则它们会被合并成一个字符串返回。
<br> 注意，fromCodePoint方法定义在String对象上，而codePointAt方法定义在字符串的实例对象上。
```
String.fromCodePoint(0x20BB7)
// "𠮷"
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
// true
```

#### String.raw()
该方法返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，往往用于模板字符串的处理方法。
```
String.raw`Hi\n${2+3}!`
// 实际返回 "Hi\\n5!"，显示的是转义后的结果 "Hi\n5!"

String.raw`Hi\u000A!`;
// 实际返回 "Hi\\u000A!"，显示的是转义后的结果 "Hi\u000A!"
```
如果原字符串的斜杠已经转义，那么String.raw()会进行再次转义。
```
String.raw`Hi\\n`
// 返回 "Hi\\\\n"

String.raw`Hi\\n` === "Hi\\\\n" // true
```
String.raw()方法可以作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。

String.raw()本质上是一个正常的函数，只是专用于模板字符串的标签函数。如果写成正常函数的形式，它的第一个参数，应该是一个具有raw属性的对象，且raw属性的值应该是一个数组，对应模板字符串解析后的值。
```
// `foo${1 + 2}bar`
// 等同于
String.raw({ raw: ['foo', 'bar'] }, 1 + 2) // "foo3bar"
```
上面代码中，String.raw()方法的第一个参数是一个对象，它的raw属性等同于原始的模板字符串解析后得到的数组。

作为函数，String.raw()的代码实现基本如下。
```
String.raw = function (strings, ...values) {
  let output = '';
  let index;
  for (index = 0; index < values.length; index++) {
    output += strings.raw[index] + values[index];
  }

  output += strings.raw[index]
  return output;
}
```

#### codePointAt()
处理 4 个字节储存的字符，返回一个字符的码点。
<br>返回 32 位的 UTF-16 字符的码点。对于那些两个字节储存的常规字符，它的返回结果与charCodeAt()方法相同。
<br>codePointAt()方法返回的是码点的十进制值，如果想要十六进制的值，可以使用toString()方法转换一下。

```
let s = '𠮷a';

s.codePointAt(0) // 134071
s.codePointAt(1) // 57271

s.codePointAt(2) // 97

s.codePointAt(0).toString(16) // "20bb7"
s.codePointAt(2).toString(16) // "61"
```
你可能注意到了，codePointAt()方法的参数，仍然是不正确的。比如，上面代码中，字符a在字符串s的正确位置序号应该是 1，但是必须向codePointAt()方法传入 2。解决这个问题的一个办法是使用for...of循环，因为它会正确识别 32 位的 UTF-16 字符。
```
let s = '𠮷a';
for (let ch of s) {
  console.log(ch.codePointAt(0).toString(16));
}
// 20bb7
// 61
```
另一种方法也可以，使用扩展运算符（...）进行展开运算。
```
let arr = [...'𠮷a']; // arr.length === 2
arr.forEach(
  ch => console.log(ch.codePointAt(0).toString(16))
);
// 20bb7
// 61
```
codePointAt()方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。
```
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}

is32Bit("𠮷") // true
is32Bit("a") // false
```

#### normalize()
将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。
```
'\u01D1'.normalize() === '\u004F\u030C'.normalize()
// true
```
normalize方法可以接受一个参数来指定normalize的方式，参数的四个可选值如下:
- NFC，默认参数，表示“标准等价合成”（Normalization Form Canonical Composition），返回多个简单字符的合成字符。所谓“标准等价”指的是视觉和语义上的等价。
- NFD，表示“标准等价分解”（Normalization Form Canonical Decomposition），即在标准等价的前提下，返回合成字符分解的多个简单字符。
- NFKC，表示“兼容等价合成”（Normalization Form Compatibility Composition），返回合成字符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，比如“囍”和“喜喜”。（这只是用来举例，normalize方法不能识别中文。）
- NFKD，表示“兼容等价分解”（Normalization Form Compatibility Decomposition），即在兼容等价的前提下，返回合成字符分解的多个简单字符。

```
'\u004F\u030C'.normalize('NFC').length // 1
'\u004F\u030C'.normalize('NFD').length // 2
```
上面代码表示，NFC参数返回字符的合成形式，NFD参数返回字符的分解形式。
<br>不过，normalize方法目前不能识别三个或三个以上字符的合成。这种情况下，还是只能使用正则表达式，通过 Unicode 编号区间判断。

#### includes(), startsWith(), endsWith()
用来确定一个字符串是否包含在另一个字符串中。
- includes()：返回布尔值，表示是否找到了参数字符串。
- startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
- endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```
let s = 'Hello world!';
s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

这三个方法都支持第二个参数，表示开始搜索的位置。

```
let s = 'Hello world!';
s.startsWith('world', 6) // true 从第n个位置直到字符串结束
s.endsWith('Hello', 5) // true  针对前n个字符
s.includes('Hello', 6) // false 从第n个位置直到字符串结束
```

#### repeat()
repeat方法返回一个新字符串，表示将原字符串重复n次。
```
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```
参数如果是小数，会被取整。
```
'na'.repeat(2.9) // "nana"
```
如果repeat的参数是负数或者Infinity，会报错。
```
'na'.repeat(Infinity) // RangeError
'na'.repeat(-1) // RangeError
```
但是，如果参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。
```
'na'.repeat(-0.9) // ""
```
参数NaN等同于 0。
```
'na'.repeat(NaN) // ""
```
如果repeat的参数是字符串，则会先转换成数字。
```
'na'.repeat('na') // ""
'na'.repeat('3') // "nanana"
```

#### padStart()，padEnd()
如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。

```
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'

'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'

'abc'.padStart(10, '0123456789') // '0123456abc'

'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```

另一个用途是提示字符串格式。

```
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```

#### trimStart()，trimEnd()
行为与trim()一致，trimStart()消除字符串头部的空格，trimEnd()消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。
除了空格键，这两个方法对字符串头部（或尾部）的 tab 键、换行符等不可见的空白符号也有效。

```
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

#### matchAll()

返回一个正则表达式在当前字符串的所有匹配。
```
let regexp = /t(e)(st(\d?))/g;
let str = 'test1test2';

let array = [...str.matchAll(regexp)];

console.log(array[0]);
// expected output: Array ["test1", "e", "st1", "1"]

console.log(array[1]);
// expected output: Array ["test2", "e", "st2", "2"]
```

#### 数组扩展运算符 ...
```
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5
```
替代函数的 apply 方法：

```
// ES5 的写法
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6的写法
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```

下面是扩展运算符取代apply方法的一个实际的例子，应用Math.max方法，简化求出一个数组最大元素的写法。

```
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

上面代码中，由于 JavaScript 不提供求数组最大元素的函数，所以只能套用Math.max函数，将数组转为一个参数序列，然后求最大值。有了扩展运算符以后，就可以直接用Math.max了。

另一个例子是通过push函数，将一个数组添加到另一个数组的尾部。

```
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```

上面代码的 ES5 写法中，push方法的参数不能是数组，所以只好通过apply方法变通使用push方法。有了扩展运算符，就可以直接将数组传入push方法。

下面是另外一个例子。

```
// ES5
new (Date.bind.apply(Date, [null, 2015, 1, 1]))
// ES6
new Date(...[2015, 1, 1]);
```

<b>应用:</b>

1. 复制数组

数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

```
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]
```

上面代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化。

ES5 只能用变通方法来复制数组。

```
const a1 = [1, 2];
const a2 = a1.concat();

a2[0] = 2;
a1 // [1, 2]
```

上面代码中，a1会返回原数组的克隆，再修改a2就不会对a1产生影响。

扩展运算符提供了复制数组的简便写法。

```
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
上面的两种写法，a2都是a1的克隆。
```

2. 合并数组

扩展运算符提供了数组合并的新写法。

```
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3); // [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3] // [ 'a', 'b', 'c', 'd', 'e' ]

// 不过，这两种方法都是浅拷贝，使用的时候需要注意。

const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

a3[0] === a1[0] // true
a4[0] === a1[0] // true

//上面代码中，a3和a4是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了引用指向的值，会同步反映到新数组。
```

3. 与解构赋值结合

扩展运算符可以与解构赋值结合起来，用于生成数组。

```
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
```

下面是另外一些例子。

```
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

```
const [...butLast, last] = [1, 2, 3, 4, 5]; // 报错
const [first, ...middle, last] = [1, 2, 3, 4, 5]; // 报错
```

4. 字符串

扩展运算符还可以将字符串转为真正的数组。

```
[...'hello']
// [ "h", "e", "l", "l", "o" ]
// 上面的写法，有一个重要的好处，那就是能够正确识别四个字节的 Unicode 字符。

'x\uD83D\uDE80y'.length // 4
[...'x\uD83D\uDE80y'].length // 3
// 上面代码的第一种写法，JavaScript 会将四个字节的 Unicode 字符，识别为 2 个字符，采用扩展运算符就没有这个问题。因此，正确返回字符串长度的函数，可以像下面这样写。

function length(str) {
  return [...str].length;
}

length('x\uD83D\uDE80y') // 3
// 凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写。
```

```
let str = 'x\uD83D\uDE80y';

str.split('').reverse().join('') // 'y\uDE80\uD83Dx'
[...str].reverse().join('') // 'y\uD83D\uDE80x'
//上面代码中，如果不用扩展运算符，字符串的reverse操作就不正确。
```

5. 实现了 Iterator 接口的对象

任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。

```
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];
// 上面代码中，querySelectorAll方法返回的是一个NodeList对象。它不是数组，而是一个类似数组的对象。这时，扩展运算符可以将其转为真正的数组，原因就在于NodeList对象实现了 Iterator 。
```

```
Number.prototype[Symbol.iterator] = function*() {
  let i = 0;
  let num = this.valueOf();
  while (i < num) {
    yield i++;
  }
}

console.log([...5]) // [0, 1, 2, 3, 4]
// 上面代码中，先定义了Number对象的遍历器接口，扩展运算符将5自动转成Number实例以后，就会调用这个接口，就会返回自定义的结果。

// 对于那些没有部署 Iterator 接口的类似数组的对象，扩展运算符就无法将其转为真正的数组。
let arrayLike = {
  '0': 'a',
  '1': 'b',
  '2': 'c',
  length: 3
};

// TypeError: Cannot spread non-iterable object.
let arr = [...arrayLike];
// 上面代码中，arrayLike是一个类似数组的对象，但是没有部署 Iterator 接口，扩展运算符就会报错。这时，可以改为使用Array.from方法将arrayLike转为真正的数组。
```

6. Map 和 Set 结构，Generator 函数

扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

```
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```

Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。

```
const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

上面代码中，变量go是一个 Generator 函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。

如果对没有 Iterator 接口的对象，使用扩展运算符，将会报错。

```
const obj = {a: 1, b: 2};
let arr = [...obj]; // TypeError: Cannot spread non-iterable object
```

#### Array.from()
> Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

下面是一个类似数组的对象，Array.from将它转为真正的数组。

```
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

1. 类似数组的对象。实际应用中，常见的是 DOM 操作返回的 NodeList 集合，以及函数内部的arguments对象。Array.from都可以将它们转为真正的数组。

```
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```

2. 只要是部署了 Iterator 接口的数据结构，Array.from都能将其转为数组。如字符串和 Set 结构都具有 Iterator 接口，因此可以被Array.from转为真正的数组。

```
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```

3. 一个真正的数组，Array.from会返回一个一模一样的新数组。

```
Array.from([1, 2, 3])
// [1, 2, 3]
```

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```

下面的例子是取出一组 DOM 节点的文本内容。

```
let spans = document.querySelectorAll('span.name');

// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);

// Array.from()
let names2 = Array.from(spans, s => s.textContent)
```

#### Array.of()

> Array.of方法用于将一组值，转换为数组。

```
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```

Array方法没有参数、一个参数、三个参数时，返回结果都不一样。只有当参数个数不少于 2 个时，Array()才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度。

```
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```

Array.of方法可以用下面的代码模拟实现。

```
function ArrayOf(){
  return [].slice.call(arguments);
}
```

#### 数组实例的 copyWithin()
> 在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

> Array.prototype.copyWithin(target, start = 0, end = this.length)

+ target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
+ start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
+ end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

```
[1, 2, 3, 4, 5].copyWithin(0, 3) // [4, 5, 3, 4, 5]

// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4) // [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1) // [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3) // {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2); // Int32Array [3, 4, 5, 4, 5]

// 对于没有部署 TypedArray 的 copyWithin 方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4); // Int32Array [4, 2, 3, 4, 5]
```

#### 数组实例的 find() 和 findIndex()

> find()，用于找出第一个符合条件的数组成员。如果没有符合条件的成员，则返回undefined。参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。

> findIndex() 返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。用法与find()类似。

```
[1, 4, -5, 10].find((n) => n < 0) // -5
// 上面代码找出数组中第一个小于 0 的成员。

[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
// 上面代码中，find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

[1, 5, 10, 15].findIndex((item)=> item>9) // 2
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2

[NaN].indexOf(NaN) // -1
[NaN].findIndex(y => Object.is(NaN, y)) // 0
// indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到。
```

#### 数组实例的 fill()
> fill()使用给定值，填充一个数组。

```
// 用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。
['a', 'b', 'c'].fill(7) // [7, 7, 7]
new Array(3).fill(7) // [7, 7, 7]

// fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
['a', 'b', 'c'].fill(7, 1, 2) // ['a', 7, 'c']
```

#### 数组实例的 entries()，keys() 和 values()
> 用于遍历数组。

都返回一个遍历器对象，可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

```
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

#### 数组实例的 includes()
> Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。

```
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true

// 第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```

与 indexOf 区别：
1. 不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。
2. 它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判。

```
[NaN].indexOf(NaN) // -1
[NaN].includes(NaN) // true
```

与 has 区别：
1. Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
2. Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。

下面代码用来检查当前环境是否支持includes方法，如果不支持，部署一个简易的替代版本。

```
const contains = (()=>
    Array.prototype.includes ? (arr, value) => arr.includes(value) : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
contains(['foo', 'bar'], 'bar'); // => true
```

#### 数组实例的 flat()，flatMap()
> Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

> flatMap()方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组。

flat()默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将flat()方法的参数写成一个整数，表示想要拉平的层数，默认为1。

```
[1, 2, [3, [4, 5]]].flat() // [1, 2, 3, [4, 5]]
[1, 2, [3, [4, 5]]].flat(2) // [1, 2, 3, 4, 5]

// 如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。
[1, [2, [3]]].flat(Infinity) // [1, 2, 3]

// 如果原数组有空位，flat()方法会跳过空位。
[1, 2, , 4, 5].flat() // [1, 2, 4, 5]
```

flatMap()只能展开一层数组。flatMap()方法还可以有第二个参数，用来绑定遍历函数里面的this。

```
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2]) // [2, 4, 3, 6, 4, 8]

// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
[1, 2, 3, 4].flatMap(x => [[x * 2]]) // [[2], [4], [6], [8]]
```

#### 数组的空位
> 数组的某一个位置没有任何值。比如，Array构造函数返回的数组都是空位。

注意，空位不是undefined，一个位置的值等于undefined，依然是有值的。空位是没有任何值，in运算符可以说明这一点。

```
Array(3) // [, , ,]

0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
// 说明，第一个数组的 0 号位置是有值的，第二个数组的 0 号位置没有值。
```

ES5 与 ES6 的区别：

ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。
- forEach(), filter(), reduce(), every() 和some()都会跳过空位。
- map()会跳过空位，但会保留这个值
- join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。

```
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

ES6 明确将空位转为undefined。
- Array.from()
- ...
- copyWithin()
- fill()
- for...of
- entries() 
- keys()
- values()
- find()
- findIndex()

```
Array.from(['a',,'b']) // [ "a", undefined, "b" ]
[...['a',,'b']] // [ "a", undefined, "b" ]
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
new Array(3).fill('a') // ["a","a","a"]

let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1

[...[,'a'].entries()] // [[0,undefined], [1,"a"]]
[...[,'a'].keys()] // [0,1]
[...[,'a'].values()] // [undefined,"a"]
[,'a'].find(x => true) // undefined
[,'a'].findIndex(x => true) // 0
```

#### Array.prototype.sort() 的排序稳定性
```
arr.sort(); // [2, 3, 4, 4, 6, 7, 9]
arr.sort((a,b)=>{return a-b}); // [2, 3, 4, 4, 6, 7, 9]
arr.sort((a,b)=>{return b-a}); // [9, 7, 6, 4, 4, 3, 2]
```