当并列书写多个div标签，它们会纵向向下排位，如果我们想将多个div并列成一排，就得借助position，float，或display属性，这便是传统的盒模型做法。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923220917955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RheDFf,size_16,color_FFFFFF,t_70#pic_center)


而flex布局则是一种新的布局方案，通过为修改父div的**display属性**，让父元素成为一个**flex容器**，从而可以自由的操作容器中子元素(项目)的排列方式。

例如我们让多个div横向排列，传统做法是使用浮动，但浮空后因为脱离文档流的缘故，父元素会失去高度，这又涉及了清除浮动等一系列的问题。

而flex布局相对简单很多，修改父元素**display:flex**，你会发现div自动就排列成了一行，而且没有浮动之后的副作用，从回流角度考虑，flex的性能更优于float；随着浏览器不断兼容以及旧版本的淘汰，flex布局注定会成为更为流行的布局方案。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923220956947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RheDFf,size_16,color_FFFFFF,t_70#pic_center)
flex布局属性主要由**容器属性**和**项目属性**构成，下面我会分开讨论这两类属性。

--------------

## 主轴方向

flex容器分为x轴与y轴，x轴正方向默认从左至右，y轴正方向默认从上到下。

定义一个容器为弹性布局display:flex;主轴默认方向为左到右;如果我们想去改变flex的默认方向，就需要用到flex-direction属性flex-direction有四个属性值，分别是row、row-reverse、column、column-reverse，分别为从左到右、从右到左、从上到下、从下到上!

## 主轴方向的对齐方式

(1)justify-content:flex-start则主轴为左对齐

(2)justify-content:flex-end 则主轴为右对齐

(3)justify-content:center 则主轴为居中

(4)justify-content:space-between 先两边贴边，再平分剩余空间

(5)justify-content:space-around 平分剩余空间

## 侧轴单行对齐方式

(1)align-item:flex-start侧轴从上到下

(2)align-item:flex-end侧轴从下到上

(3)align-item:center侧轴居中对齐

(4)align-item:baseline侧轴以基线对齐

(5)align-item:strech这是默认方式

## 侧轴多行对齐方式

(1)align-content:flex-start上对齐

(2)align-content:flex-end下对齐

(3)align-content:center上下居中

(4)align-content:space-between 先两边贴边，再平分剩余空间

(5)align-content:space-around 平分剩余空间

以上均是添加到父元素身上的属性

## 侧轴单个元素对齐方式
(1)align-self:flex-start上对齐

(2)align-self:flex-end下对齐

(3)align-self:center上下居中

(4)align-self:space-between 先两边贴边，再平分剩余空间

(5)align-self:space-around 平分剩余空间



## 子项目换行

(1)flex-wrap:wrap超出父元素会换行

(2)flex-wrap:wrap-reverse反向换行

(3)flex-wrap:no wrap这是默认方式,不换行


## 子项目的一些属性

- order:0,定义子项目的排序位置，数值越小越靠前，默认为0

- flex-grow:0;定义子项目的放大比例，默认为0不放大

- flex-shrink:1;定义子项目的缩小比例，默认为1，空间不足将等比缩小，0则不缩小，负值无效

- flex-basis:1;定义子项目占据空间，默认为auto，可以设置百分比，也可以是固定值

以上三种属性可以简写，比如flex：1,1,1顺序如上

## 练习flex布局的小游戏
[帮小青蛙回家！](https://flexboxfroggy.com/)