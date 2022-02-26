## 在常规html标签中应用

```html
<div id="test" ref="test">test</div>
```

```javascript
console.log(document.getElementById('test'))
console.log(this.$refs.test)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f9ed30805f1f48fa84128b7fbfe36287.png)

ref与getElement获取到的内容是相同的

## 在组件中应用

```html
<Test ref="testCom"/>
```

```javascript
console.log(this.$refs.testCom)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3d54e82375864d30a158b631ca43d9d7.png)

ref获取到的是组件对象，可以调用该对象下的属性和方法

## 与v-for组合使用

```html
<ul v-for="item in list" :key="item.name">
	<li ref="item">{{item.name}}</li>
</ul>
```

```javascript
  data() {
    return{
      list:[
        {name: 1},
        {name: 2},
        {name: 3}
      ]
    }
  },
  mounted(){
    console.log(this.$refs.item)
  }

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5af5e329143f439480c74a567a0fe877.png)
可以看到，输出的是ref所在循环中的所有元素。

## 官方文档

![在这里插入图片描述](https://img-blog.csdnimg.cn/b34b8cbd7705487fa3f2d9b3bb03ba04.png)
## 总结

 - ref被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的$ref对象上
 - 如果在普通的DOM元素上使用，引用指向的就是DOM元素；如果用在子组件上，引用就指向组件实例
 - 当v-for用于元素或组件的时候，引用信息奖是包含DOM借点或组件实例的数组