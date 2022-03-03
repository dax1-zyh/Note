## 全局事件总线
一种组件间通信的方式，适用于任意组件间通信

## 安装全局事件总线

```javascript
new Vue({
	...
	beforeCreate() {
		Vue.prototype.$bus = this
		// 安装全局事件总线，$bus就是当前应用的vm
	}
})
```

## 使用全局事件总线

### 接收数据
A组件想要接收数据，则在A组件中给$bus绑定自定义事件，时间的回调留在A组件自身。

**方法一：回调函数写在methods中**
```javascript
methods:{
	demo(){
		doSomething
	}
},
mounted(){
	this.$bus.$on('xxxx',this.demo)
}
```

**方法二：回调函数用箭头函数**

```javascript
mounted(){
	this.$bus.$on('xxxx',()=>{doSomething})
}
```

当组件B触发‘xxxx’事件时，A组件的回调函数会被调用.

### 提供数据

```javascript
this.$bus.$emit('xxxx',要传递的数据)
```

### 解绑事件总线
最好在使用全局总线的组件中，在beforeDestroy钩子中，用$off去解绑当前组件用到的事件

```javascript
this.$bus.$off('xxxx')
```