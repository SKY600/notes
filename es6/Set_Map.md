### Set
> Set 一个构造函数，用来生成 Set 数据结构。类似于数组，但是成员的值都是唯一的。

```
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
for (let i of s) {
  console.log(i);
}
// 2 3 5 4

const set = new Set([1, 2, 3, 4, 4]);
set // {1, 2, 3, 4}
[...set]// [1, 2, 3, 4]
```

可用于数组和字符串去重

```
[...new Set(array)]
[...new Set('ababbc')].join('')
```

Set 实例的属性：
+ Set.prototype.constructor：构造函数，默认就是Set函数。
+ Set.prototype.size：返回Set实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。
+ 四个操作方法：
    - Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
    - Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
    - Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
    - Set.prototype.clear()：清除所有成员，没有返回值。

+ 四个遍历方法，可以用于遍历成员。
    - Set.prototype.keys()：返回键名的遍历器
    - Set.prototype.values()：返回键值的遍历器
    - Set.prototype.entries()：返回键值对的遍历器
    - Set.prototype.forEach()：使用回调函数遍历每个成员

### WeakSet
语法: 
> const ws = new WeakSet();

方法：
- WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
- WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
- WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在。


与 Set 的区别：
- WeakSet 的成员只能是对象，而不能是其他类型的值。
- WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用。
- WeakSet 不可遍历。

    
    也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
    这是因为垃圾回收机制依赖引用计数，如果一个值的引用次数不为0，垃圾回收机制就不会释放这块内存。结束使用该值之后，有时会忘记取消引用，导致内存无法释放，进而可能会引发内存泄漏。WeakSet 里面的引用，都不计入垃圾回收机制，所以就不存在这个问题。因此，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。
    由于上面这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。

### Map
> 它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"

// 分别使用 Set 对象和 Map 对象，当作Map构造函数的参数，结果都生成了新的 Map 对象。
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const m1 = new Map(set);
m1.get('foo') // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz') // 3

// 注意，只有对同一个对象的引用，Map 结构才将其视为同一个键。这一点要非常小心。
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```

实例的属性：
+ size 属性
+ Map.prototype.set(key, value)
+ Map.prototype.get(key) get方法读取key对应的键值，如果找不到key，返回undefined。
+ Map.prototype.has(key) has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
+ Map.prototype.delete(key) delete方法删除某个键，返回true。如果删除失败，返回false。
+ Map.prototype.clear() clear方法清除所有成员，没有返回值。

实例的操作方法：遍历、与其他结构的互相转换

遍历：Map 的遍历顺序就是插入顺序。
+ Map.prototype.keys()：返回键名的遍历器。[...map.keys()]
+ Map.prototype.values()：返回键值的遍历器。
+ Map.prototype.entries()：返回所有成员的遍历器。
+ Map.prototype.forEach()：遍历 Map 的所有成员。

与其他结构的互相转换：
+ Map 转为数组 —— [...map]
+ 数组 转为 Map —— new Map(arr)
+ Map 转为对象
+ 对象转为 Map —— Object.entries()
+ Map 转为 JSON
    - Map 的键名都是字符串，这时可以选择转为对象 JSON。
    - Map 的键名有非字符串，这时可以选择转为数组 JSON。
+ JSON 转为 Map

### WeakMap
与Map结构类似，也是用于生成键值对的集合。

WeakMap 与 Map 的区别有两点：
1. 首先，WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
2. 其次，WeakMap的键名所指向的对象，不计入垃圾回收机制。
3. 没有遍历操作（即没有keys()、values()和entries()方法），也没有size属性。
4. 二是无法清空，即不支持clear方法。因此只有四个方法 get()、set()、has()、delete()。

它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。一个典型应用场景是，在网页的 DOM 元素上添加数据，就可以使用WeakMap结构。当该 DOM 元素被清除，其所对应的WeakMap记录就会自动被移除。
