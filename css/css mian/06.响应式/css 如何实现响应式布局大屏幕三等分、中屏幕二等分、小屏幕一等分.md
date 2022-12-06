### css 如何实现响应式布局大屏幕三等分、中屏幕二等分、小屏幕一等分？

```
<div class="container" style="width: 100%;height: 600px;background-color: antiquewhite;">
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
  <div class="item" style="height: 100%;background-color: aquamarine;"></div>
</div>
```

```
@media (min-width: 768px) {
  .container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (min-width: 1024px) {
  .container {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}

.container {
  display: grid;
}

.conainer {
  gap: 1rem;
}
```

**总结：** 
Grid 布局可以自动判断容器大小，无论大小屏幕自动撑满并均分，请看以下属性
```
.container {
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}
```
* repeat: 用以 N 整分
* auto-fill：表示自动填充
* minmax: 即书面意思，最小宽度为 300px