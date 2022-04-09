## 1.新建全局样式表
新建`global.css`文件，并在`main.js`中引入。`global.css`文件一般都放在`src/asset/style`下

```javascript
import "./assets/style/global.css"
```

在 **global.css** 文件中写的样式，无论在哪一个 vue 单页面都会覆盖 ElementUI 默认的样式。

## 2.在当前vue单页面中添加一个新的style标签
在当前的vue单页面的style标签后，添加一对新的style标签，新的style标签中不要添加`scoped`。在有`scoped`属性的style标签中的样式不会覆盖ElementUI默认的样式

## 3.使用`/deep/`深度修改标签样式
找到需要修改的ElementUI标签的类名，然后在类名前加上`/deep/`，可以强制修改默认样式。这种方式可以直接用到有`scoped`属性的style标签中。

```css
// 修改级联选择框的默认宽度
/deep/ .el-cascader {
  width: 100%;
}
```

## 4.通过内联样式或者绑定类样式覆盖默认样式
通过内联样式style可以在某些标签中直接覆盖默认样式，不是很通用。


## 总结
- 第一种全局引入CSS文件的方式，适合于对ElementUI整体的修改，比如整体配色的修改
- 第二种添加一个style标签的形式，也能够实现修改默认样式的效果，但实际上因为是修改了全局的样式，因此在不同的vue组件中修改同一个样式可能有冲突
- 第三种方式通过`/deep/`的方式可以很方便地在vue组件中修改默认样式，也不会与其他页面冲突
- 第四种内联样式局限性比较大，不太通用