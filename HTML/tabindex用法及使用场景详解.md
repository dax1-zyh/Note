## tabindex定义

tabindex 全局属性 ，表示元素是否可以**聚焦**，以及是否能用**键盘导航（Tab）**选中



## 支持聚焦的HTML元素

- 带`href`属性的`<a>`标签
- 带`href`属性的`<link>`标签
- `<button>`
- `<input>`（排除 `type='hidden'`）
- `<select>`
- `<textarea>`



这些元素默认都能使用键盘Tab键，以及JS `focus`方法聚焦

```js
document.querySelector("a").focus();
```



在使用 Tab 键聚焦元素时，聚焦顺序等于元素在源码文件中的出现顺序。 尽管默认行为涵盖了我们所需的大多数交互需求。但在某些情况下，我们可能有移除、添加聚焦，或者重新安排项目聚焦顺序的需要，这个时候就要 `tabindex` 来帮忙了。



## tabindex的值

tabindex接收一个整数作为值，具有不同的结果，具体结果取决于整数的值：



- **负值**:通常为 `index='-1'`,表示元素是可聚焦的，但是**不能通过键盘导航来访问到该元素**。

```js
<button type="button">Click me to start, then press the "tab" key</button>
<button type="button">I’ll be in focus first</button>
<button type="button" tabindex="-1">I won’t be in focus :(</button>
<button type="button">I’ll be in focus last</button>
```

效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8cdea67b442c479d886612de4fd490ce.webp)





- **0**: `tabindex='0'`,表示元素是可聚焦的，并且**可以通过键盘导航来聚焦到该元素**，相对顺序是当前元素**处于DOM中的位置**来决定的

```js
<button type="button">Click me to start, then press the "tab" key</button>
<button type="button">I’ll be in focus first</button>
<div tabindex="0">I’m a DIV and will be in focus second</div>
<button type="button">I’ll be in focus last</button>
```

效果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2f2ceb4a9d62430b93985f45ccf5a2a2.webp)


`tabindex='0'`通常应用在不可聚焦的元素上，让这些元素变得原本就可聚焦一样。



- **正值**:表示元素是可聚焦的，并且**可以通过键盘导航来访问到该元素**；它的**相对顺序按照`tabindex` 的数值递增而滞后**获焦。如果多个元素拥有相同的 `tabindex`，它们的相对顺序按照他们在当前DOM中的先后顺序决定。

```js
<button type="button" tabindex="1">I’m the first focusable item on the page</button>
<button type="button" tabindex="500">I’m the second</button>
```

效果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/56e33be2d0124281a148cc671a9aaeb1.webp)




## 键盘导航顺序

根据键盘序列导航的顺序，值为 `0` 、非法值、或者没有 `tabindex` 值的元素应该放置在 `tabindex` 值为正值的元素后面。



## 使用场景

### 负值场景

模态框是一个很好的说明例子。模态容器通常是不可聚焦的元素，像 `<div>` 或 `<section>` 这些元素，当模式窗口打开时，我们会将焦点移到该窗口，以便屏幕阅读器读其内容。但是，我们又不希望模式容器接受 Tab 聚焦。这时就可以使用一个 `tabindex` 负值来实现。

```js
<div id="modal" tabindex="-1">
    <!-- Modal content -->
</div>

<script>
function openModal() {
    document.getElementById("modal").focus();
    
    // Other stuff to show modal
}
</script>
```



### 0场景

`tabindex="0"` 通常用来为不可聚焦元素添加可聚焦属性。

一个比较好的用例就是在使用自定义元素的时候。比如，我们在创建一个自定义按钮元素，由于它不是 `<button>`，因此默认它是无法聚焦的。我们可以使用 `tabindex` 属性为它添加聚焦功能，就能像常规按钮一样会被安排焦点顺序了。

```js
<my-custom-button tabindex="0">Click me!</my-custom-button>
```



### 正值场景

通过正值的大小关系调整键盘导航的访问先手顺序

----------------

原文搬运 [How and when to use the tabindex attribute](https://bitsofco.de/how-and-when-to-use-the-tabindex-attribute/#fn-1)

译文搬运 [[译] 如何使用 HTML tabindex 属性](https://juejin.cn/post/6844904129408532487)