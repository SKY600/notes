#### 1. Grid如何实现类似 flex：row-reverse的效果?
通过grid-auto-flow和direction

grid-auto-flow：row|column|dense 该属性指定采用 grid 布局的容器内部的元素如何排序

direction：rtl / ltr 指定文本 表列的水平方向

#### 2. 盒子三等分
（1）Grid
```
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr（fr=fraction）;
  gap: 1rem;
}
```
（2）flex
```
.flex-container {
  display: flex;
  flex-wrap: wrap;
  // gap: 1rem;
​
  .item {
    flex: 0 0 calc(100% / 3);
  }
}
```

#### 3.Grid的优势
Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是一维布局。

Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是二维布局。 Grid 布局远比 Flex 布局强大。
