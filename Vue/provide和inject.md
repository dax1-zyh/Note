## 前言

Vue的父子间传值 ： `props`

爷孙或者更深嵌套的组件间传值： `provide/inject`

更复杂的结构：`vuex`



## provide/inject

提供/注入

### provide 

- 一个对象或返回一个对象的函数。该对象包含可注入其子孙的属性。在该对象中你可以使用 ES2015 Symbols 作为 key，但是只在原生支持 Symbol 和 Reflect.ownKeys 的环境下可工作。



### inject

- 一个字符串数组，或一个对象



## 使用场景

![在这里插入图片描述](https://img-blog.csdnimg.cn/adfa0e32ed494cae8989dfa0827a4223.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARGF4MV8=,size_20,color_FFFFFF,t_70,g_se,x_16)




例如这样的层次结构

```text
Root
└─ 组件A
   ├─ 组件B
   └─ 组件C
      ├─ 组件D
      └─ 组件E
```



如果要将 `组件A` 的数据直接传递给 `组件E`，我们要将 prop 逐级传递下去：`组件A` -> `组件C` -> `组件E`。而通过provide/inject ，可直接从 `组件A`传递给 `组件E`



## 使用方法

```js
// 父组件/祖先组件 ，提供provide
//第一种
export default {
  name: "grandfather",
  provide(){
    return{
      foo:'halo'
    }
  },
}

//第二种
export default {
  name: "grandfather",
  provide:{
    foo:'halo~~~~'
  },
}
```



```js
// 后代组件 ，注入inject
export default {
  inject:['foo'],
}
```



**provide方法**

祖先组件的provide方法只建议第一种，即函数写法。若想传递的数据只是字符串，两种方式并无区别，但是第二种方法（对象写法）不能传递对象。



## provide/inject 不是可响应的

官方文档

```text
provide 和 inject 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。
```



我们可以通过传递一个对象的方式，实现数据的响应式。

```js
// 祖先组件
data() {
  return {
    obj:{name:'dax1'},
  }
}
provide(){
  return{
    username:this.obj	// 此处provide一个对象
  }
},
```

```js
// 后代组件
export default {
    inject: ['username']    
}
```

此时 `username`是响应式的数据，因为**对象的属性是响应的**。



## 实现全局传递

provide/inject 只能从祖先传递给后代，但是可以通过在`App.vue`绑定provide，所有的子组件就都能注入inject，从而达到全局传递。





------

参考文档

[Provide / Inject | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/component-provide-inject.html)

 [你可能不知道的provide/inject用法](https://segmentfault.com/a/1190000020954324)