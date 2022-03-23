## 定位方式
`position`属性有五个属性值，分别代表五种定位方式
- static 静态定位
- relative 相对定位
- fixed 固定定位
- absolute 绝对定位
- sticky 粘性定位

## static 静态定位
`static`是`position`属性的默认值，若省略`position`属性，则默认是静态定位。

静态定位下的元素为标准流，即按照元素的顺序来决定每个元素的位置，元素与元素之间不会产生重叠。

![在这里插入图片描述](https://img-blog.csdnimg.cn/612fe811972d43099183c6a356abbd28.png)

## relative 相对定位
相对定位，是相对默认位置进行偏移，即定位基点为元素的默认位置。
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5af74c7eb0a4edda4105cae552f225c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4549a3bcdabf4c6a89bef526006f48ed.png)


必须搭配`top,bottom,left,right`四个属性一起使用，用来指定偏移的方向和距离。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c591e49c4d14af2b0ed281fa806f052.png)

相对定位的元素**不会脱离标准流**，即进行偏移后，原来的默认位置仍然占有着。

## absolute 绝对定位
`absolute`是相对于祖先元素进行便宜，即定位基点是祖先元素。

同时，绝对定位的祖先元素有很重要的限制条件，祖先元素必须也有定位（静态定位除外），若祖先元素都没有定位，则该元素将以浏览器为基点，若祖先元素都有定位，则以距离（层级关系）最近的祖先元素为基点。

绝对定位也必须搭配`top,bottom,left,right`四个属性一起使用

![在这里插入图片描述](https://img-blog.csdnimg.cn/610385e04df749449087ba6b762d72f7.png)

```css
/*
  HTML 代码如下
  <div id="father">
    <div id="son"></div>
  </div>
*/

#father {
  positon: relative;
}
#son {
  position: absolute;
  top: 20px;
}
```
由于上文提到的必要条件，用到绝对定位的时候，一般要使用“子绝父相”（子元素绝对定位，父元素相对定位）的环境。

上面代码中，父元素是`relative`定位，子元素是`absolute`定位，所以子元素的定位基点是父元素，相对于父元素的顶部向下偏移20px。如果父元素是static定位，上例的子元素就是距离网页的顶部向下偏移20px。

绝对定位的元素会**脱离标准流**，即原来的默认位置不会继续占有。

## fixed 固定定位
`fixed`表示相对于视口(viewport，浏览器窗口)进行偏移，即定位基点是浏览器窗口。这会导致元素的位置不随页面滚动而变化，好像固定在网页上一样。

![在这里插入图片描述](https://img-blog.csdnimg.cn/916d7e455e454262b2ea0d0d49df5fb2.png)

它如果搭配`top、bottom、left、right`这四个属性一起使用，表示元素的初始位置是基于视口计算的，否则初始位置就是元素的默认位置。


## sticky 粘性定位
`sticky`粘性定位像是相对定位和固定定位的结合，一些时候是相对定位（定位基点是自身默认位置），一些时候是固定定位（定位基点是视口）

固定定位的生效前提是，`top、bottom、left、right`符合条件，此时定位方式变为固定定位，否则为相对定位。

它的具体规则是，当页面滚动，父元素开始脱离视口时（即部分不可见），只要与`sticky`元素的距离达到生效门槛，`relative`定位自动切换为`fixed`定位；等到父元素完全脱离视口时（即完全不可见），`fixed`定位自动切换回`relative`定位。

粘性定位的元素**不会脱离标准流**，即进行偏移后，原来的默认位置仍然占有着。

## 定位方式的比较
![在这里插入图片描述](https://img-blog.csdnimg.cn/202b7bc380d843fd8d4058898ecfd5ae.png)



他们的主要区别在两点上

- 定位基点
- 是否脱离标准流

## 参考文档
[CSS定位详解 - 阮一峰](https://www.ruanyifeng.com/blog/2019/11/css-position.html)