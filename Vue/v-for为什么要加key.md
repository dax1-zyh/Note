## 举个栗子

**不设置key**
```javascript
 <div id="app">
    <div>
      <input type="text" v-model="name">
      <button @click="add">添加</button>
    </div>
    <ul>
      <li v-for="(item, i) in list">
        <input type="checkbox"> {{item.name}}
      </li>
    </ul>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        name: '',
        newId: 3,
        list: [
          { id: 1, name: '李斯' },
          { id: 2, name: '吕不韦' },
          { id: 3, name: '嬴政' }
        ]
      },
      methods: {
        add() {
         //注意这里是unshift
          this.list.unshift({ id: ++this.newId, name: this.name })
          this.name = ''
        }
      }
    });
  </script>
  </div>
```

当选中吕不韦时，添加楠楠后选中的确是李斯，并不是我们想要的结果，我们想要的是当添加楠楠后，一种选中的是吕不韦

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e6fadb297a84e5e9c57647e534e44e6.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d096a3aac9c248db91e4f043b6c186fc.png)


**设置key**

```javascript
  <div id="app">
    <div>
      <input type="text" v-model="name">
      <button @click="add">添加</button>
    </div>
    <ul>
      <li v-for="(item, i) in list" :key="item.id">
        <input type="checkbox"> {{item.name}}
      </li>
    </ul>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        name: '',
        newId: 3,
        list: [
          { id: 1, name: '李斯' },
          { id: 2, name: '吕不韦' },
          { id: 3, name: '嬴政' }
        ]
      },
      methods: {
        add() {
         //注意这里是unshift
          this.list.unshift({ id: ++this.newId, name: this.name })
          this.name = ''
        }
      }
    });
  </script>
  </div>
```

同样当选中吕不为时，添加楠楠后依旧选中的是吕不为。

![在这里插入图片描述](https://img-blog.csdnimg.cn/17398ce5633c41519309265b48762b12.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/377ef7f9a54e47d39be44d37d48e98c1.png)
**可以简单的这样理解：加了key(一定要具有唯一性) id的checkbox跟内容进行了一个关联。是我们想达到的效果**

------------
## 为什么要key

 - vue中列表循环需要加`:key='唯一标识'`，唯一标识尽量是id，目的是为了高效地更新虚拟DOM
 - key主要用于dom diff算法，diff算法为同级比较，比较当前标签上的key还有他当前的标签名，如果key和标签名都一样时只移动，不会重新创建元素和删除元素
 - 没有key地时候默认使用**就地复用**策略。如果数据的顺序被改变，vue不是移动DOM元素来匹配数据项的改变，而是简单复用原来位置的每个元素，在进行比较时发现标签一样值不一样时，就会复用之前的位置，将新值直接放到该位置，以此类推，最后多出一个就会把最后一个删除掉。
 - 尽量不要使用索引值index作key值，一定要用唯一标识的值，如id等。因为若用数组索引index为key，当向数组中指定位置插入一个新元素后，因为这时候会重新更新index索引，对应着后面的虚拟DOM的key值全部更新了，这个时候还是会做不必要的更新，就像没有加key一样，因此index虽然能够解决key不冲突的问题，但是并不能解决复用的情况。如果是静态数据，用索引号index做key值是没有问题的。

## 为什么key不要用index
继续举个栗子

```javascript
const list = [
    {
        id: 1,
        name: 'test1',
    },
    {
        id: 2,
        name: 'test2',
    },
    {
        id: 3,
        name: 'test3',
    },
]
```

```javascript
<div v-for="(item, index) in list" :key="index" >{{item.name}}</div>
```

上面的代码使用了index作为key

### 在列表最后添加一条数据

```javascript
const list = [
    {
        id: 1,
        name: 'test1',
    },
    {
        id: 2,
        name: 'test2',
    },
    {
        id: 3,
        name: 'test3',
    },
    {
        id: 4,
        name: '我是在最后添加的一条数据',
    },
]
```
此时前三条数据直接复用之前的,新渲染最后一条数据,此时用index作为key,没有任何问题

### 在中间插入一条数据

```javascript
const list = [
    {
        id: 1,
        name: 'test1',
    },
    {
        id: 4,
        name: '我是插队的那条数据',
    }
    {
        id: 2,
        name: 'test2',
    },
    {
        id: 3,
        name: 'test3',
    },
]
```

此时更新渲染数据,通过index定义的key去进行前后数据的对比,发现

```js
之前的数据                         之后的数据

key: 0  index: 0 name: test1     key: 0  index: 0 name: test1
key: 1  index: 1 name: test2     key: 1  index: 1 name: 我是插队的那条数据
key: 2  index: 2 name: test3     key: 2  index: 2 name: test2
                                 key: 3  index: 3 name: test3
```
通过上面清晰的对比,发现除了第一个数据可以复用之前的之外,另外三条数据都需要重新渲染;

最好的办法是使用数组中不会变化的那一项作为key值,对应到项目中,即每条数据都有一个唯一的id,来标识这条数据的唯一性;使用id作为key值,我们再来对比一下向中间插入一条数据,此时会怎么去渲染

```javascript
之前的数据                               之后的数据

key: 1  id: 1 index: 0 name: test1     key: 1  id: 1 index: 0  name: test1
key: 2  id: 2 index: 1 name: test2     key: 4  id: 4 index: 1  name: 我是插队的那条数据
key: 3  id: 3 index: 2 name: test3     key: 2  id: 2 index: 2  name: test2
                                       key: 3  id: 3 index: 3  name: test3
```
现在对比发现只有一条数据变化了,就是id为4的那条数据,因此只要新渲染这一条数据就可以了,其他都是就复用之前的;

## diff算法图解

diff算法的处理方法，对操作前后的dom树同一层的节点进行对比，一层一层对比

![在这里插入图片描述](https://img-blog.csdnimg.cn/e59bb27f3c04494f8e738d12d3cc77a1.png)

在以下的使用场景：

![在这里插入图片描述](https://img-blog.csdnimg.cn/e42f9b5f8e584a0f952479ee9f668c34.png)

我们希望可以在B和C之间加一个F，Diff算法默认执行起来是这样的：

![在这里插入图片描述](https://img-blog.csdnimg.cn/42da3d12a5e64b509b3cb9ac85f8760f.png)

即把C更新成F，D更新成C，E更新成D，最后再插入E，是不是很没有效率？

所以我们需要使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点。


![在这里插入图片描述](https://img-blog.csdnimg.cn/634a675390374dfbad38488af521414e.png)

所以一句话，key的作用主要是为了高效的更新虚拟DOM。另外vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

## 总结

 - 简单通俗地讲，没有key时，状态默认绑定的是位置，有key时，状态根据key的属性值绑定到了响应的数组元素。
 - key是为了更高效的更新虚拟DOM
 - 推荐使用数组内的字段（保证唯一性）作为key的唯一标识，不建议直接使用index

## 参考文档


[VUE中演示v-for为什么要加key](https://www.jianshu.com/p/4bd5e745ce95)

[vue核心面试题：v-for中为什么要用key](https://blog.csdn.net/qq_42072086/article/details/107997149)