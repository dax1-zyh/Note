## scoped作用

当 `<style>` 标签有 `scoped` 属性时，它的 CSS 只作用于当前组件中的元素



## scoped目标

`scoped`是为了实现组件的私有化，表示style只属于当前模块，不对全局造成污染。

使用 `scoped` 后，**父组件的样式将不会渗透到子组件中**。不过一个子组件的根节点会同时受其父组件有作用域的 CSS 和子组件有作用域的 CSS 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。



## scoped原理

通过观察渲染之后的html及CSS可以发现，添加`scoped`属性的组件，DOM结构中都会出现 `data-v-469af010` 的字段，相当于是唯一标识符。

![image-20220210010222070](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20220210010222070.png)

![image-20220210010233109](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20220210010233109.png)

添加了`scoped`属性的组件，为了达到组件私有化的目的：

- 在DOM结构中加入了 `data-v-469af010` 唯一标识符
- 组件的每个样式选择器后添加一个等同与“不重复属性”相同的字段，实现类似于“作用域”的作用，不影响全局
- 如果组件内部还有组件，只会给最外层的组件里的标签加上唯一属性字段，不影响组件内部引用的组件



## 混用本地和全局样式

一个组件中有作用域和无作用域的样式是可以同时使用的

```html
<style>
/* 全局样式 */
</style>

<style scoped>
/* 本地样式 */
</style>
```



## 谨慎使用的场景

从原理可见，之所以`scoped`可达到类似组件私有化、样式设置“作用域”的效果，其实只是在设置`scoped`属性的组件上的所有标签添加唯一的`data`开头的属性，且在标签选择器的结尾加上和属性同样的字段，起到唯一性的作用，但是这样如果组件中也引用其他组建就会出现类似下面的问题：

- 父组件无`scoped`属性，子组件带有`scoped`，父组件是无法操作子组件的样式的，虽然我们可以在全局中通过该类标签的标签选择器设置样式，但会影响到其他组件
- 父组件有`scoped`属性，子组件无，父组件也无法设置子组件样式，因为父组件的所有标签都会带有`data-v-469af010`唯一标志，但子组件不会带有这个唯一标志属性，与1同理，虽然我们可以在全局中通过该类标签的标签选择器设置样式，但会影响到其他组件
- 父子组件都有`scoped`属性，同理也无法设置样式，



## 深度作用选择器  

###  写在前面，目前只推荐 ::v-deep 

**官方文档：**

如果你希望 `scoped` 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 `>>>` 操作符：

```html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

有些像 Sass 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 操作符取而代之——这是一个 `>>>` 的别名，同样可以正常工作。



**修正：**

但是，并不是所有的操作符都是爸爸的好孩子，其中有些是已经被官方弃用的，根据尤雨溪2020年9月的这篇[文章](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Frfcs%2Fblob%2Fmaster%2Factive-rfcs%2F0023-scoped-styles-changes.md)，我们大概可以了解到：

1. 最初是使用 `>>>` 来达到 "deep" 的效果，但是某些 CSS 预处理器不支持

2. 后来选择了 `/deep/`，它曾经是 CSS 的一个真正的提议（甚至在 Chrome 中自带），但是后来被放弃了。所以为了避免用户困惑，最后使用了 `::v-deep`（加了个 v，证明是我 Vue 的纯正血统）

3. 然后 Vue 3 来了，我们可以抛弃历史包袱了，所以在 Vue 3 中，我们不再支持 `>>>` 和 `/deep/` ，推荐大家使用 `::v-deep`，而且为了更加符合 CSS 的书写习惯，希望大家使用 `::v-deep(.class)` 的书写规则

4. 在 Vue 3 中还提供了 `::v-slotted` 和 `::v-global` 两种新的操作符，针对 `<slot>` 和全局 CSS 规则

5.  `::v-slotted` 编译之后的属性值为 `data-v-xxx-s`，-s 的后缀使得它只针对 `<slot>` 内容

6. `::v-global` 编译之后则不带 `data-v-xxx` 的属性

   

**Vue2:**

- 不推荐使用 `/deep/`
- 在 Sass 之类的预处理器中使用 `::v-deep`
- 没有预处理器的情况下使用 `>>>`
- 使用上面的操作符，`<style>` 必须有 `scoped` 属性



**Vue3:**

- 不支持 `/deep/` 和 `>>>`
- 推荐使用 `::v-deep(.class)` 代替 `::v-deep .class`
- 针对 `<slot>` 可以使用 `::v-slotted` 选择器
- 可以使用 `::v-global` 注册全局样式
- 使用上面的操作符，`<style>` 必须有 `scoped` 属性





## 参考文档

[CSS 作用域 · vue-loader (vuejs.org)](https://vue-loader-v14.vuejs.org/zh-cn/features/scoped-css.html)

[vue中scoped的原理及慎用原因](https://www.jianshu.com/p/b92e2a022cd8)

[Vue scoped属性和深度作用选择器](https://juejin.cn/post/6934534556343091207)