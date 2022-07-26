## Vue2.x vs Vue3.x

 - Vue2中响应式是通过`defineProperty`实现的
 - Vue3中响应式是通过ES6的`Proxy`实现的
 - Vue3中实现响应式数据的方法是`ref和reactive`

## reactive
### 特点

 - reactive的参数一般是**对象或者数组**,他能够将复杂数据类型变为响应式数据。
 - reactive的响应式是深层次的，底层本质是将传入的数据转换为Proxy对象

### 使用方法

```javascript
import { reactive } from 'vue'
export default {
  name: 'App',
  setup () {
    const user = reactive({
      name: 'frank',
      age: 18
    })
    const handleChange = () => {
		user.name = 'mike'
		user.age = 20
    }

    return { user, handleChange }
  }
}
```


### Proxy vs defineProperty
reactive方法内部是利用ES6的Proxy API来实现的，这里与Vue2中的defineProperty方法有本质的区别。

 - defineProperty只能单一地监听已有属性的修改或者变化，无法检测到对象属性的新增或删除，而Proxy可以轻松实现
 - defineProperty无法监听属性值是数组类型的变化，而Proxy可以轻松实现

### 用reactive声明简单数据类型

```javascript
const num = reactive(100)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1689f9d0ce4f4a08a67b6249147ba7f5.png)

可以看到，用reactive定义number变量时，vue给出了一个警告，提示这个值是不能被reactive创建的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/dae3be75187e4dfe810abdcbaa3dc4f0.png)
通过Vue3源码可以发现，当使用reactive定义数据时，会先进行判断定义的数据是否是对象，是对象的话才会继续进行数据响应式的处理，反之就直接被return出来了。

因此可以得出结论reactive更推荐用于**对象或数组**的数据类型。


## ref
### 特点

 - ref的参数一般是基本数据类型，也可以是对象类型
 - 如果参数是对象类型，其实底层的本质还是reactive，系统会自动将ref转换为reactive，例如

 	`ref(1) ===> reactive({value:1})`

 - 在模板中访问ref中的数据，系统会自动帮我们添加`.value`,在JS中访问ref中的数据，需要手动添加`.value`
 - ref的底层原理同reactive一样，都是Proxy

 ### 使用方法

```javascript
<template>
	<h1>{{ name }}</h1>
	<h1>{{ age }}</h1>
</template>

<script>
import { ref} from 'vue'
export default {
  name: 'App',
  setup () {
	const name = ref('frank')
	const age = ref(18)
    const handleChange = () => {
		name.value = 'mike'
		age.value = 20
    }
    return { name, age, handleChange }
  }
}
</script>
```

## reactive vs ref

 - reactive参数一般接受**对象或数组**，是深层次的响应式。ref参数一般接收**简单数据类型**，若ref接收对象为参数，本质上会转变为reactive方法
 - 在JS中访问ref的值需要手动添加`.value`，访问reactive不需要
 - 响应式的底层原理都是Proxy
