## 隐患
- Vue3实现数据响应式的方式为`Proxy`，对IE11以下的版本存在`兼容性问题`
- 框架底层进行了大量重构，新增、废弃了很多API，需要对这些地方进行修改，以保证程序运转
- 依赖也需要升级，包括vue相关的插件和第三方插件

## 升级步骤
通过`工具+手动`

### 步骤一：
先把Vue2框架整体升级到Vue3，由于Vue3也兼容`Option Api`，因此代码结构可以暂时不用调整，之后再改为官方推荐的`Composition Api`

### 步骤二：
升级Vue相关的依赖，`Vue/Vuex/Vue-router/Vue 编译工具` 

### 步骤三：
把 Vue3 中已经不再支持的 API 和语法进行修改替换。例如已不再支持的过滤器filter，v-model 用法也改变等。

### 步骤四：
将第三方组件库、插件等依赖升级到适配Vue3的版本

### 步骤五：
确保项目代码语法编译无误后，需要检查代码中的业务逻辑是否正确，以保证项目能完整运行。

### 步骤六：
使用 TypeScript 重构 JS 代码，TypeScript 比 JavaScript 多了静态类型检查，也增加了一些新的语法，是给项目锦上添花。但是这一步会比较耗时（因为相当于修改把JS代码都要过一遍），但是项目中可以同时存在JS 和 TS，所以可以逐步替换。

### 升级工具
通过开源工具`gogocode` [文档](https://gogocode.io/zh/docs/vue/vue2-to-vue3)，可以进行上述步骤一二三的转换。

**ps**：对于工具改造代码，存在很多未知性，项目业务代码中有些比较复杂的（例如表单联动，规则校验等等）可能会对原来的逻辑有影响，需要逐一人工比对和测试。

## 升级需要的改变
### 1. 全局 API
- 全局 Vue API 已更改为使用应用程序实例
- 全局和内部 API 已经被重构为支持 `tree-shake`

### 2. 模板指令
- 组件上 `v-model` 用法已更改，以替换 `v-bind.sync`
- `<template v-for>` 和非 `v-for` 节点上的 `key`用法已更改
- 在同一元素上使用的 `v-if` 和 `v-for` 优先级已更改（`v-if`优先级大于`v-for`）
- `v-bind=“object”` 现在排序敏感
- `v-on:event.native` 修饰符已移除
- `v-for` 中的 `ref` 不再注册 `ref 数组

### 3. 组件
- 只能使用普通函数创建函数式组件
- `functional attribute` 在单文件组件 (SFC) 的 `<template>`和 `functional` 组件选项中被废弃
- 异步组件现在需要使用 `defineAsyncComponent` 方法来创建
- 组件事件现在需要在 `emits` 选项中声明


### 4.渲染函数
- 渲染函数 API 更改
- `$scopedSlots property` 已移除，所有插槽都通过 `$slots` 作为函数暴露
- `$listeners` 被移除或整合到 `$attrs`
- `$attrs` 现在包含 `class` 和 `style attribute`

### 5.自定义元素
- 自定义元素检测现在在模板编译时执行
- 特殊的 `is attribute` 的使用被严格限制在被保留的 `<component>` 标签中

### 6.其他小改变
- `destroyed` 生命周期选项被重命名为 `unmounted`
- `beforeDestroy` 生命周期选项被重命名为 `beforeUnmount`
- `default prop` 工厂函数不再可以访问 this 上下文
- 自定义指令的 API 已更改为与组件生命周期一致，且 `binding.expression` 已移除
- `data` 选项应始终被声明为一个函数
- 来自 `mixin` 的 `data` 选项现在为浅合并
- `Attribute` 强制策略已更改
- 一些过渡的 `class` 被重命名
- `<TransitionGroup>` 不再默认渲染包裹元素
- 当侦听一个数组时，只有当数组被替换时，回调才会触发，如果需要在变更时触发，则必须指定 deep 选项
- 没有特殊指令的标记 (v-if/else-if/else、v-for 或 v-slot) 的 `<template>` 现在被视为普通元素，并将渲染
- 原生的 `<template>` 元素，而不是渲染其内部内容。
- 已挂载的应用不会取代它所挂载的元素
- 生命周期的 `hook:` 事件前缀改为 `vnode-`

### 7.被移除的 API
- `keyCode` 作为 `v-on` 修饰符的支持
- `on、off` 和 `$once` 实例方法
- 过滤器 (filter)
- 内联模板 `attribute`
- `$children` 实例 `property`
- `propsData` 选项
- `$destroy` 实例方法。用户不应再手动管理单个 Vue 组件的生命周期。
- 全局函数 `set` 和 `delete` 以及实例方法 `$set` 和 `$delete`。基于代理的变化检测已经不再需要它们了
- Element 和 Element Plus 框架也有所修改，请自行查阅 Element Plus 文档，对组件用法进行调整。


## 参考文档
[Vue2 大型项目升级 Vue3 详细经验总结](https://blog.csdn.net/Kevinblant/article/details/126238184)