## 方法一，给父元素加上.clearfix
```css
.clearfix:after{
	content: '';
	display: block; /*或者table*/
	clear: both;
}
```

## 方法二，给父元素加上overflow:hidden
其目的就是让父元素触发BFC，因为BFC的特性之一可以清除浮动。