### display: inline 的元素设置 margin 和 padding 会生效吗？

inline 元素的 margin 与 padding 左右生效，上下生效，准确说在上下方向不会使其它元素受到挤压，仿佛不生效，如下图：

<div style='width: 500px;background-color: #ccc;height: 10rem;'>
  我是<span style='padding: 1rem;margin: 1rem;border: 1px solid red;background: skyblue;'>行内元素</span>白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。
</div>

<br>
```
<div class="container">
  我是<span class="item">行内元素</span>白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。白日依山尽，黄河入海流。
</div>

.item {
  padding: 1rem;
  margin: 1rem;
  border: 1px solid red;
  background: skyblue;
}

.container {
  width: 500px;
  background-color: #ccc;
  height: 10rem;
}
```