## 前言
react中的状态更新方法`setState()`是异步的


## 对象式写法
`setState(stateChange, [callback])`
- `stateChange`为状态改变对象(该对象可以体现出状态的更改)
- `callback`是可选的回调函数, 它在状态更新完毕、界面也更新后(`render`调用后)才被调用

```javascript
	state = {count:0}

	add = ()=>{
		// 对象式setState
		this.setState({count:this.state.count+1},()=>{
			console.log(this.state.count);
		})
	}
```

## 函数式写法
 `setState(updater, [callback])`
 - `updater`为返回`stateChange`对象的函数。
 - `updater`可以接收到`state`和`props`。**（主要区别）**
 - `callback`是可选的回调函数, 它在状态更新、界面也更新后(`render`调用后)才被调用。

```javascript
	state = {count:0}

	add = ()=>{
		//函数式的setState
		this.setState( state => ({count:state.count+1}),()=>{
			console.log(this.state.count)
		})
	}
```

## 总结
- 对象式的`setState`是函数式的`setState`的简写方式(语法糖)
- 使用原则：
  - 如果新状态不依赖于原状态 ===> 使用对象方式
  - 如果新状态依赖于原状态 ===> 使用函数方式
  - 如果需要在`setState()`执行后获取最新的状态数据, 要在第二个`callback`函数中读取