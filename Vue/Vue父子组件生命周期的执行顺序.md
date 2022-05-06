## 加载渲染过程
**父组件先创建，然后子组件创建；子组件先挂载，然后父组件再挂载**

即生命周期如下
- 父beforeCreate
- 父created
- 父beforeMount
- 子beforeCreate
- 子created
- 子beforeMount
- 子mounted
- 父Mounted

## 销毁过程
**父组件先做销毁准备，然后子组件开始完整销毁，然后父组件完整销毁**

即生命周期如下
- 父beforeDestory
- 子beforeDestory
- 子destoryed
- 父destoryed