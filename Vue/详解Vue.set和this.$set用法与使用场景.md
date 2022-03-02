## 一。为什么有Vue.set
由于JavaScript的限制，Vue无法检测到data中数组和对象的变化，因此也不会触发视图更新

## 二。解决方法
### 数组
#### 1.使用Vue提供的变异方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/d9c667c5fffe465ebcbba52b3445a469.png)
Vue对这些JS数组方法进行了封装，通过这些方法是可以检测到数组更新的。

#### 2.将原数组整个替换
如下例中，是要实现vm.items[1] = 'excess'
```javascript
<body>
<div id="app">
    <ul>
        <li v-for="(item, index) in items">
            {{ index }} : {{ item }}
        </li>
    </ul>
</div>

<script>
let vm = new Vue({
    el: '#app',
    data: {
        items: ['a', 'b', 'c']
    },
    created() {
        this.items = ['a', 'test', 'c']
    }
})
</script>
</body>
```

#### 3.使用Vue.set（见后文）

-----------

### 对象
#### 1.将原对象整个替换
如下例中，是要实现给object新增一个键值对{test: 'newthing'}

```javascript
<body>
<div id="app">
    <ul>
        <li v-for="(value, name) in object">
            {{ name }} : {{ value }}
        </li>
    </ul>
</div>

<script>
let vm = new Vue({
    el: '#app',
    data: {
        object: {
            title: 'How to do lists in Vue',
            author: 'Jane Doe',
            publishedAt: '2016-04-10'
        }
    },
    created() {
        this.object = {
            title: 'How to do lists in Vue',
            author: 'Jane Doe',
            publishedAt: '2016-04-10',
            test: 'newthing'
        }
    }
})
</script>
</body>
```

#### 2.使用Vue.set（见后文）
-------
## 三。Vue.set
### 对于数组
Vue不能检测以下数组的变动:

 - 利用索引值直接设置一个数组项时，例如`vm.list[0]=newValue`
 - 修改数组长度时，例如`vm.list.length=newLength`

举个栗子

```javascript
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

这时可以使用Vue.set或者this.$set

#### 使用方法
`Vue.set(target,index,newValue)`

```javascript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
```

```javascript
// this.$set
vm.$set(vm.items, indexOfItem, newValue)
```

### 对于对象
Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property 必须在 data 对象上存在才能让 Vue 将它转换为响应式的。

举个栗子

```javascript
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的
```

#### 使用方法
`Vue.set(target,key,value)`


```javascript
Vue.set(vm.someObject, 'b', 2)
```

```javascript
this.$set(this.someObject,'b',2)
```

### 注意
Vue不允许动态添加**根级**响应式属性

```javascript
const app = new Vue({
  data: {
    a: 1
  }
  // render: h => h(Suduko)
}).$mount('#app1')

Vue.set(app.data, 'b', 2)
```
![](https://img-blog.csdnimg.cn/f66f23d6084f44e08b1645eaa890d01d.png)
只可以使用 Vue.set(object, propertyName, value) 方法向**嵌套对象**添加响应式属性

```javascript
var vm=new Vue({
    el:'#test',
    data:{
        //data中已经存在info根属性
        info:{
            name:'小明';
        }
    }
});
//给info添加一个性别属性
Vue.set(vm.info,'sex','男');
```
## 四。使用场景
当我们对data中的数组或对象进行修改时，有些操作方式是非响应式的，Vue检测不到数据更新，因此也不会触发视图更新。此时需要使用Vue.set()进行响应式的数据更新。