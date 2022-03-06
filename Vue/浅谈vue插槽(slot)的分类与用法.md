## 前言
在Vue2.6中，**具名插槽**和**作用域插槽**引入了统一的语法：`v-slot`，取代了`slot`和`slot-scope`。

Slot通俗的理解就是**占坑**，在组件模板中站好了位置，当使用该组件标签的时候，组件标签里面的内容就会自动填坑。坑里面放什么内容由父组件决定。

## 默认插槽

 - 父组件 home.vue
 - 子组件 test.vue

```html
//home.vue
<test>
     Hello Word
</test>
```

```html
//test.vue
<a href="#">
	 <slot></slot>
</a>
```
当组件渲染的时候，`<slot></slot>`会被替换为Hello Word


## 具名插槽

有时候我们一个组件里需要多个插槽，slot元素有一个特殊的特性：name ，这个特性可以用来定义额外的插槽

```html
<div>
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>

```

```html
<div>
  <header>
    <slot name="header"></slot>
  </header>
  
  <main>
    <slot></slot>
  </main>
  
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
如果一个<slot>不带name属性的话，那么它的name默认为default

在向具名插槽提供内容的时候，我们可以在template元素上使用v-slot指令，并以参数的形式提供其名称

```html
<div>
   <template v-slot:header>
    <h1>Here might be a page title</h1>
   </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here some contact info</p>
  </template>
</div>
```

## 作用域插槽
插槽跟模板其他地方一样都可以访问相同的实例属性(也就是相同的"作用域")，而不能访问test的作用域

如果想访问test作用域，我们把需要传递的内容绑到 slot 上，然后在父组件中用v-slot设置一个值来定义我们提供插槽的名字：

```html
//test.vue
<div>
	<!-- 设置默认值：{{user.lastName}}获取 Jun -->
	<!-- 如果home.vue中给这个插槽值的话，则不显示 Jun -->
	<!-- 设置一个 usertext 然后把user绑到设置的 usertext 上 -->
	<slot v-bind:usertext="user">{{user.lastName}}</slot>
</div>

//定义内容
data(){
  return{
	user:{
	  firstName:"Fan",
	  lastName:"Jun"
	}
  }
}
```
然后在home.vue中接收传过来的值：

```html
//home.vue
<div>
  <test v-slot:default="slotProps">
    {{slotProps.usertext.firstName}}
  </test>
</div>
```
这样就可以获得test.vue组件传过来的值了

绑定在 <slot> 元素上的特性被称为插槽 prop。在父组件中，我们可以用 v-slot 设置一个值来定义我们提供的插槽 prop 的名字，然后直接使用就好了

## 参考文档
[Vue 插槽(slot)使用(通俗易懂)](https://juejin.cn/post/6844903920037281805)