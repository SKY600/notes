### ES6常用方法

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

#### repeat()

#### padStart()，padEnd()
#### trimStart()，trimEnd()
#### matchAll()