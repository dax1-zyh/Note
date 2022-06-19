## 为什么需要nextTick
Vue中有异步更新策略，如果数据发生变化，Vue不会立刻更新DOM，而是开启一个队列，把组件更新函数保存在队列中，在同一事件循环中发生的所有数据变更会异步地批量更新。这一策略导致我们对数据的修改不会立刻体现在DOM上，此时如果想要获取更新后的DOM状态，就需要使用nextTick。



## 作用
在**下一次DOM更新**结束后执行其指定的回调。

在Vue内部，nextTick之所以能够让我们看到DOM更新之后的结果，是因为传入的callback会被添加到队列刷新函数`flushSchedulerQueue`的后面，这样等队列内部的更新函数都执行完毕，所有DOM操作也就结束了，callback自然能够获取到最新的DOM值。




## 语法

```javascript
this.$nextTick(回调函数)
```
将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。



## 使用场景
更改数据后当你想立即使用js操作新的视图的时候需要使用它



**举个栗子**

点击按钮显示原本以 v-show = false 隐藏起来的输入框，并获取焦点。

```javascript
showsou(){
  this.showit = true //修改 v-show
  document.getElementById("keywords").focus() 
   //在第一个 tick 里，获取不到输入框，自然也获取不到焦点
}
```

修改为

```javascript
showsou(){
  this.showit = true
  this.$nextTick(function () {
    // DOM 更新了
    document.getElementById("keywords").focus()
  })
}
```

在数据修改后立即调用nextTick，从而获取焦点。