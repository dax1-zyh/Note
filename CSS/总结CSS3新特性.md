## 盒子模型
CSS3中新增了`box-sizing`属性，有两个值分别为`border-box`和`content-box`
- content-box:也称IE盒模型，特点是 width = content + padding +border
- border-box:也称标准和模型，特点是width = content


## 新选择器
CSS3中新增了一些选择器，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/e8fc3985d4794f3c89c5dc8fa94c6f0e.png)

## 新样式
### 边框

- border-radius：创建圆角边框
- box-shadow：为元素添加阴影
- border-image：使用图片来绘制边框

### 背景
**background-clip**

用于确定背景画区，有以下几种可能的属性：

- background-clip: border-box; 背景从border开始显示
- background-clip: padding-box; 背景从padding开始显示
- background-clip: content-box; 背景显content区域开始显示
- background-clip: no-clip; 默认属性，等同于border-box

通常情况，背景都是覆盖整个元素的，利用这个属性可以设定背景颜色或图片的覆盖范围



**background-origin**

当我们设置背景图片时，图片是会以左上角对齐，但是是以border的左上角对齐还是以padding的左上角或者content的左上角对齐? border-origin正是用来设置这个的

- background-origin: border-box; 从border开始计算background-position
- background-origin: padding-box; 从padding开始计算background-position
- background-origin: content-box; 从content开始计算background-position
默认情况是padding-box，即以padding的左上角为原点

**background-size**

background-size属性常用来调整背景图片的大小，主要用于设定图片本身。有以下可能的属性：

- background-size: contain; 缩小图片以适合元素（维持像素长宽比）
- background-size: cover; 扩展元素以填补元素（维持像素长宽比）
- background-size: 100px 100px; 缩小图片至指定的大小
- background-size: 50% 100%; 缩小图片至指定的大小，百分比是相对包 含元素的尺寸

**background-break**

元素可以被分成几个独立的盒子（如使内联元素span跨越多行），background-break 属性用来控制背景怎样在这些不同的盒子中显示

- background-break: continuous; 默认值。忽略盒之间的距离（也就是像元素没有分成多个盒子，依然是一个整体一样）
- background-break: bounding-box; 把盒之间的距离计算在内；
- background-break: each-box; 为每个盒子单独重绘背景


## transfrom 转换
transform属性允许你旋转，缩放，倾斜或平移给定元素

transform-origin：转换元素的位置（围绕那个点进行转换），默认值为`(x,y,z):(50%,50%,0)`

- transform: translate(120px, 50%)：位移
- transform: scale(2, 0.5)：缩放
- transform: rotate(0.5turn)：旋转
- transform: skew(30deg, 20deg)：倾斜

## transition 过渡
`transition: CSS属性 花费时间 效果曲线（默认ease） 延迟时间（默认0）`

## animation 动画
创建CSS3动画，首先需要了解`@keyframes`，它指定一个CSS样式和动画将逐步从目前的样式更改为新的样式。

用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。

```css
@keyframes myfirst
{
    from {background: red;}
    to {background: yellow;}
}
```
然后将`@keyframes`创建的动画绑定到一个选择器，如下，将myfirst动画捆绑到div元素，时长5s

```css
div
{
    animation: myfirst 5s;
}
```
animation也有很多的属性

- animation-name：动画名称
- animation-duration：动画持续时间
- animation-timing-function：动画时间函数
- animation-delay：动画延迟时间
- animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
- animation-direction：动画执行方向
- animation-paly-state：动画播放状态
- animation-fill-mode：动画填充模式

## 过渡和动画的区别
**transition的局限性**
- transition需要事件触发，没法在网页加载时发生
- transition是一次性的，除非一直触发，否则不会重复发生
- transition只能定义开始和结束两个状态，不能定义中间状态
- 一条transition规则，只能绑定一个属性或者所有属性的变化，不能涉及多个属性

**过渡与动画的主要区别**
- 动画不需要事件触发，过渡需要事件触发
- 动画可以设置多个关键帧，而过渡只有“开始-结束”一组。

## 渐变
颜色渐变是指在两个颜色之间平稳的过渡，css3渐变包括

- linear-gradient：线性渐变
`background-image: linear-gradient(direction, color-stop1, color-stop2, ...);`

- radial-gradient：径向渐变
`linear-gradient(0deg, red, green);`

## flex弹性布局和Grid栅格布局
后单独写一篇文章总结

## 参考文档
[面试官：CSS3新增了哪些新特性？](https://github.com/febobo/web-interview/issues/106)