## 前言
在Vue官方文档中，明确说明了`不建议将v-for和v-if同时使用`。因为两者作用在同一个元素时，优先级是不同的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8dff05ea2c22463ebc0c73b3294d87b2.png)



- **在vue2中，v-for的优先级更高**

- **在vue3中，v-if的优先级更高**

  

## Vue2
在Vue2中，v-for的优先级是高于v-if的，如果作用在同一元素上，输出的渲染函数中可以看除会先执行循环再判断条件，哪怕只渲染列表中一小部分元素，也得在每次重渲染的时候遍历整个列表，这会造成性能的浪费



## Vue3
而在Vue3中，v-if的优先级时高于v-for的，因此v-if执行时要调用的变量可能还不存在，会导致报错。



## 使用场景
通常有两种情况导致需要v-if和v-for同时使用：

- 为了**过滤列表中的项目**，例如`v-for = 'user in users' v-if  = 'user.isActive'` 。此时可以定义出一个计算属性，例如`activeUsers`，让其返回过滤后的列表即可，`users.filter( u => u.isActive)`

- 为了**避免渲染本应该被隐藏的列表**，例如`v-for = 'user in users' v-if = 'shouldShowUsers'`。此时可以把`v-if`绑定在容器元素上，例如`ul,ol`或在外包一层`template`