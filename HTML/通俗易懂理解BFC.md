## 什么是BFC
BFC全称**Block Formatting Context**，中文名称**块级格式化上下文**

具有BFC特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在i布局上影响到外面的元素，并且BFC具有普通容器所没有的一些特性。

## 如何触发BFC
- html根元素 
- 浮动元素 （元素的float不是none）
- 绝对定位元素 （元素的position为absolute或者fixed）
- 行内块元素 （inline-block）
- overflow的值不为visible的块元素 （hidden,scroll,auto都可）
- 弹性元素 （display为flex或者inline-flex元素的直接子元素）

## BFC的应用场景

### 外边距重叠
同一个BFC内部，元素之间的外边距会发生折叠，如下。

```html
<head>
div{
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
</head>
<body>
    <div></div>
    <div></div>
</body>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/c3d24d9aac103bcbee829c94a7a848c4.png)

从效果上看，因为两个 div 元素都处于同一个 BFC 容器下 (这里指 body 元素) 所以第一个 div 的下边距和第二个 div 的上边距发生了重叠，所以两个盒子之间距离只有 100px，而不是 200px。

首先这不是 CSS 的 bug，我们可以理解为一种规范，**如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中**。


```html
<div class="container">
    <p></p>
</div>
<div class="container">
    <p></p>
</div>
```

```css
.container {
    overflow: hidden;
}
p {
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
```

此时两个盒子的间距就变成了200px

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/095216be67d1102a01a5e9a77925391d.png)
### 清除浮动
浮动的元素会脱离普通文档流，让浮动元素的容器触发BFC，即可清楚浮动。

```html
<div style="border: 1px solid #000;">
    <div style="width: 100px;height: 100px;background: #eee;float: left;"></div>
</div>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/aa982cfe7109025d7fd2b05b23489cd3.png)
由于容器内元素浮动，脱离了文档流，所以容器只剩下 2px 的边距高度。如果使触发容器的 BFC，那么容器将会包裹着浮动元素。

```html
<div style="border: 1px solid #000;overflow: hidden">
    <div style="width: 100px;height: 100px;background: #eee;float: left;"></div>
</div>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/54cd13503dca141b0cd3eb39e0fa7b49.png)

### 阻止元素被浮动元素覆盖

```html
<div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
<div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动, 
也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/3e16974f8190eabb262ff5cd3d655f38.png)

这时候其实第二个元素有部分被浮动元素所覆盖，(但是文本信息不会被浮动元素所覆盖) 如果想避免元素被覆盖，可触第二个元素的 BFC 特性，在第二个元素中加入 overflow: hidden，就会变成：

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/452bdfcee7da500e7156eced13af1b96.png)

这个方法可以用来实现两列自适应布局，效果不错，这时候左边的宽度固定，右边的内容自适应宽度(去掉上面右边内容的宽度)。

## 参考文档
[10 分钟理解 BFC 原理](https://zhuanlan.zhihu.com/p/25321647)