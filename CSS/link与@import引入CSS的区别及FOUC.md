## link和@import
在CSS中，`link`和`@import`都可以用来引入外部样式表，但它们有一些区别。

```html
 <link href="a.css" rel="stylesheet">

 <style> @import url('a.css'); </style>
```

- 用法：`link`是HTML标签，还可以用来定义RSS、rel链接等，而`@import`只能用来引入样式表。

- 加载顺序：`link`标签在页面加载时同时被加载，而`@import`在页面加载完成后才会被加载。因此，`link`可以让页面更快地加载，而`@import`会导致页面加载速度变慢。

- 浏览器兼容性：`link`标签的兼容性更好，可以被所有浏览器支持，而`@import`在一些旧版本的浏览器中可能不被支持。

- DOM操作：`link`标签可以通过JavaScript来操作DOM，而`@import`不可以。

- FOUC问题：`link`最大限度地支持并行下载，`@import`过多嵌套会导致串行下载，出现`FOUC`。

- 优先级： `link > @import`
##  FOUC

### 什么是FOUC

FOUC(Flash of Unstyled Content):用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再重新显示文档，造成**页面闪烁**，在网速较慢或过多使用`@import`时出现，影响用户体验。

### 如何避免FOUC
- 把CSS代码放在文档的`<head>`
- 尽量使用`link`代替`@import`

## 总结
`link标签`优于`@import方法`