在多个回调依赖的场景中，尽管Promise通过链式调用取代了回调嵌套，但过多的链式调用可读性仍然不佳，流程控制也不方便，ES7 提出的async 函数，终于让 JS 对于异步操作有了终极解决方案，简洁优美地解决了以上两个问题。

设想一个这样的场景，异步任务a->b->c之间存在依赖关系，如果我们通过then链式调用来处理这些关系，可读性并不是很好，如果我们想控制其中某个过程，比如在某些条件下，b不往下执行到c，那么也不是很方便控制。

```
Promise.resolve(a)
  .then(b => {
    // do something
  })
  .then(c => {
    // do something
  })
```

但是如果通过async/await来实现这个场景，可读性和流程控制都会方便不少。

```
async () => {
  const a = await Promise.resolve(a);
  const b = await Promise.resolve(b);
  const c = await Promise.resolve(c);
}
```

那么我们要如何实现一个async/await呢，首先我们要知道，async/await实际上是对Generator（生成器）的封装，是一个语法糖。由于Generator出现不久就被async/await取代了，很多同学对Generator比较陌生，因此我们先来看看Generator的用法：
ES6 新引入了 Generator 函数，可以通过 yield 关键字，把函数的执行流挂起，通过next()方法可以切换到下一个状态，为改变执行流程提供了可能，从而为异步编程提供解决方案。

```
function* myGenerator() {
  yield '1'
  yield '2'
  return '3'
}
const gen = myGenerator();  // 获取迭代器
gen.next()  //{value: "1", done: false}
gen.next()  //{value: "2", done: false}
gen.next()  //{value: "3", done: true}
```

也可以通过给next()传参, 让yield具有返回值

```
function* myGenerator() {
  console.log(yield '1')  //test1
  console.log(yield '2')  //test2
  console.log(yield '3')  //test3
}
// 获取迭代器const gen = myGenerator();
gen.next()
gen.next('test1')
gen.next('test2')
gen.next('test3')
```

我们看到Generator的用法，应该️会感到很熟悉，/yield 和 async/await看起来其实已经很相似了，它们都提供了暂停执行的功能，但二者又有三点不同：
* async/await自带执行器，不需要手动调用next()就能自动执行下一步
* async函数返回值是Promise对象，而Generator返回的是生成器对象
* await能够返回Promise的resolve/reject的值

我们对async/await的实现，其实也就是对应以上三点封装Generator
##### 1. 自动执行

我们先来看一下，对于这样一个Generator，手动执行是怎样一个流程
```
function* myGenerator() {
  yield Promise.resolve(1);
  yield Promise.resolve(2);
  yield Promise.resolve(3);
}
const gen = myGenerator()
gen.next().value.then(val => {
  console.log(val)
  gen.next().value.then(val => {
    console.log(val)
    gen.next().value.then(val => {
      console.log(val)
    })
  })
})
//输出1 2 3
```

我们也可以通过给gen.next()传值的方式，让yield能返回resolve的值

```
function* myGenerator() {
  console.log(yield Promise.resolve(1))   //1
  console.log(yield Promise.resolve(2))   //2
  console.log(yield Promise.resolve(3))   //3
}
const gen = myGenerator()
gen.next().value.then(val => {
  // console.log(val)
  gen.next(val).value.then(val => {
    // console.log(val)
    gen.next(val).value.then(val => {
      // console.log(val)
      gen.next(val)
    })
  })
})
```

显然，手动执行的写法看起来既笨拙又丑陋，我们希望生成器函数能自动往下执行，且yield能返回resolve的值，基于这两个需求，我们进行一个基本的封装，这里async/await是关键字，不能重写，我们用函数来模拟：

```
function run(gen) {
  var g = gen()                     //由于每次gen()获取到的都是最新的迭代器,因此获取迭代器操作要放在step()之前,否则会进入死循环
  function step(val) {              //封装一个方法, 递归执行next()
    var res = g.next(val)           //获取迭代器对象，并返回resolve的值
    if(res.done) return res.value   //递归终止条件
    res.value.then(val => {         //Promise的then方法是实现自动迭代的前提
      step(val)                     //等待Promise完成就自动执行下一个next，并传入resolve的值
    })
  }
  step()  //第一次执行
}
```

对于我们之前的例子，我们就能这样执行：

```
function* myGenerator() {
  console.log(yield Promise.resolve(1))   //1
  console.log(yield Promise.resolve(2))   //2
  console.log(yield Promise.resolve(3))   //3
}
run(myGenerator)
```

这样我们就初步实现了一个async/await

上边的代码只有五六行，但并不是一下就能看明白的，我们之前用了四个例子来做铺垫，也是为了让读者更好地理解这段代码。 简单的说，我们封装了一个run方法，run方法里我们把执行下一步的操作封装成step()，每次Promise.then()的时候都去执行step()，实现自动迭代的效果。在迭代的过程中，我们还把resolve的值传入gen.next()，使得yield得以返回Promise的resolve的值

这里插一句，是不是只有.then方法这样的形式才能完成我们自动执行的功能呢？

答案是否定的，yield后边除了接Promise，还可以接thunk函数，thunk函数不是一个新东西，所谓thunk函数，就是单参的只接受回调的函数，详细介绍可以看阮一峰Thunk 函数的含义和用法，无论是Promise还是thunk函数，其核心都是通过传入回调的方式来实现Generator的自动执行。thunk函数只作为一个拓展知识，理解有困难的同学也可以跳过这里，并不影响后续理解。

##### 2. 返回Promise & 异常处理
虽然我们实现了Generator的自动执行以及让yield返回resolve的值，但上边的代码还存在着几点问题：
1. 需要兼容基本类型：这段代码能自动执行的前提是yield后面跟Promise，为了兼容后面跟着基本类型值的情况，我们需要把yield跟的内容(gen().next.value)都用Promise.resolve()转化一遍
2. 缺少错误处理：上边代码里的Promise如果执行失败，就会导致后续执行直接中断，我们需要通过调用Generator.prototype.throw()，把错误抛出来，才能被外层的try-catch捕获到
3. 返回值是Promise：async/await的返回值是一个Promise，我们这里也需要保持一致，给返回值包一个Promise


我们改造一下run方法：
```
function run(gen) {
  //把返回值包装成promise
  return new Promise((resolve, reject) => {
    var g = gen()
    function step(val) {
      //错误处理
      try {
        var res = g.next(val)
      } catch(err) {
        return reject(err);
      }
      if(res.done) {
        return resolve(res.value);
      }
      //res.value包装为promise，以兼容yield后面跟基本类型的情况
      Promise.resolve(res.value).then(
        val => {
          step(val);
        },
        err => {
          //抛出错误
          g.throw(err)
        });
    }
    step();
  });
}
```
然后我们可以测试一下：
```
function* myGenerator() {
  try {
    console.log(yield Promise.resolve(1))
    console.log(yield 2)   //2
    console.log(yield Promise.reject('error'))
  } catch (error) {
    console.log(error)
  }
}
const result = run(myGenerator)     //result是一个Promise//输出 1 2 error
```
到这里，一个async/await的实现基本完成了。最后我们可以看一下babel对async/await的转换结果，其实整体的思路是一样的，但是写法稍有不同：

```
//相当于我们的run()
function _async ToGenerator(fn) {
  return function() {
    var self = this
    var args = arguments
    return new Promise(function(resolve, reject) {
      var gen = fn.apply(self, args);
      //相当于我们的step()
      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'next', value);
      }
      //处理异常
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'throw', err);
      }
      _next(undefined);
    });
  };
}
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {
    var info = gen[key](arg);
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }
  if (info.done) {
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}
```

使用方式：
```
const foo = _asyncToGenerator(function* () {
  try {
    console.log(yield Promise.resolve(1))   //1
    console.log(yield 2)                    //2
    return '3'
  } catch (error) {
    console.log(error)
  }
})
foo().then(res => {
  console.log(res)                          //3
})
```
有关async/await的实现，到这里告一段落。但是直到结尾，我们也不知道await到底是如何暂停执行的，有关await暂停执行的秘密，我们还要到Generator的实现中去寻找答案