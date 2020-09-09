### ajax 创建过程
1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象.
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
3. 设置响应HTTP请求状态变化的函数.
4. 发送HTTP请求.
5. 获取异步调用返回的数据.
6. 使用JavaScript和DOM实现局部刷新.

```
var xmlHttp = new XMLHttpRequest();
    xmlHttp.open('GET','demo.php','true');
    xmlHttp.send()
    xmlHttp.onreadystatechange = function(){
        if(xmlHttp.readyState === 4 & xmlHttp.status === 200){
        }
    }
```

### ajax的缺点
1. ajax不支持浏览器back按钮。
2. 安全问题 AJAX暴露了与服务器交互的细节。
3. 对搜索引擎的支持比较弱。
4. 破坏了程序的异常机制。
5. 不容易调试。

### ajax 在 IE缓存问题
在IE浏览器下，如果请求的方法是GET，并且请求的URL不变，那么这个请求的结果就会被缓存。解决这个问题的办法可以通过实时改变请求的URL，只要URL改变，就不会被缓存，可以通过在URL末尾添加上随机的时间戳参数```('t'= + new Date().getTime())``` 或者：
```open('GET','demo.php?rand=+Math.random()',true);```

### Ajax请求的页面历史记录状态问题
* 可以通过锚点来记录状态，```location.hash```。让浏览器记录Ajax请求时页面状态的变化。

* 还可以通过 HTML5 的 ```history.pushState```，来实现浏览器地址栏的无刷新改变