## 作用
在**下一次DOM更新**结束后执行其指定的回调。


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