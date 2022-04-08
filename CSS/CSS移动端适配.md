##  meta viewport
viewport即视口，代表当前可见的计算机图形区域，通常与浏览器窗口相同。

在移动端如何配置视口 ？ =》 `meta`标签

```javascript
<meta name="viewport" content="width=device-width; initial-scale=1; maximum-scale=1; minimum-scale=1; user-scalable=no;">
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2045b81da3f4f90bface65d4f51cf79.png)

上述的meta设置，就是我们的理想设置，他规定了我们的视口宽度为屏幕宽度，初始缩放比例为1，禁止用户手动缩放。

## CSS长度单位
**1. px**
px即像素，而像素是相对显示器屏幕分辨率而言的

**2. em**
- em的值并不固定，根据使用它的元素的大小决定
- em继承**父级元素**的字体大小
- 任意浏览器的默认字体都是16px

**3. rem**
- rem是CSS3中新增的单位，并且相对的元素都是**html根元素(html)**，默认也是16px
- 通过它既可以左到只修改根元素就成比例的调整所有字体大小，又可以避免字体大小逐层复合的连锁反应
- 区别em是根据父元素继承相应大小而不是固定的，rem是继承html根元素的大小
- 只有改变根元素html的`fonst-size`才能改变rem的值


## rem适配

```css
//假设我给根元素的大小设置为14px
html{
    font-size：14px
}
//那么我底下的p标签如果想要也是14像素
p{
    font-size:1rem
}
```

rem的布局 不得不提flexible，flexible方案是阿里早期开源的一个移动端适配解决方案，引用flexible后，我们在页面上统一使用rem来布局。

他的原理非常简单

```javascript
// set 1rem = viewWidth / 10

function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
}
setRemUnit();
```

rem 是相对于html节点的font-size来做计算的。所以在页面初始话的时候给根元素设置一个font-size，接下来的元素就根据rem来布局，这样就可以保证在页面大小变化时，布局可以自适应

另一种写法：

```html
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      <title>动态REM</title>
      <script>
        var pageWidth = window.innerWidth
         document.write('<style>html{font-size:'+pageWidth+'px;}</style>')
      </script>
    </head>
```

## @media 媒体查询
媒体查询可以根据屏幕尺寸的变化,进行动态适配,语法为`@margin (条件) {}`

```css
 @media (max-width: 700px) {
	border: 1px solid red;
 }
```
相当于屏幕宽度在700px以下时，做对应的CSS样式修改。