## 1.插值表达式
数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值

```html
<span>Message: {{ msg }}</span>
```

`msg`的值会从data中传递，而这就造成了在某种情况下的**弊端**：

当网络环境较差、网速较慢、刷新频繁的时候，插值表达式会出现`{undefined{message}}`的情况，（之后再用真实的数据替换掉）。因为页面加载是自上而下的，加载到表达式时，可能还没有解析到script标签中的data数据。

**解决方法**

 - 使用`v-clock`解决屏幕闪动问题

 	Vue提供了内置属性v-clock，这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 [v-cloak] { display: none } 一起用时，可以隐藏未编译的 Mustache 标签直到实例准备完毕。

```html
<p v-clock> {{msg}} </p>
```


 - 使用`v-text`或者`v-html`



## 2.v-text

```html
<p v-clock> {{msg}} </p>
<p v-text='msg'></p>
```
v-text的显示效果与插值表达式一致，并且不会出现上述的屏幕闪动现象。那么v-text和插值表达式有什么具体的区别呢？

```html
<p v-clock> Message:{{msg}} </p>
<p v-text='msg'>Message:</p>
```

这里`msg`的值为ABCDEFG

运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f65501ff4c794c08b90dcb956f2ceb07.png)

**结论:**
**插值表达式相当于一个占位符，只会替换掉占位置的内容。
v-text只渲染Vue传递过来的数据，会替换掉节点内已有内容。
因此需要根据需求判断是否需要同时展示。**



## 3.v-html
双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：

```html
 	<p v-text='title'></p>
 	<p v-html='title'></p>
```

```javascript
	data:{
		title:'<h1>我是h1标题</h1>'
	}	
```

运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/40056eb0c32949bf9de99057a884fa86.png)

**结论：**
**插值表达式和v-text不能够解析html标签，v-html能够解析html标签**



## 总结

 - 如果只是单独展示Vue对象里的数据，建议使用“v-text”指令。
 - 如果要同时展示用户前台数据，那么就需要用插值表达式，但是不要忘记和“v-cloak”属性一起使用（同时需要设置样式[v-cloak]{display:none;}）。
 - 如果Vue对象传递过来的数据含有HTML标签，则使用v-html