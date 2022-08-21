## 字符串方式

```javascript
class Demo entends React.Component{

	const {input1} = this.refs
		
	showData = () => {
		console.log(input1.value)
	}
	
	render(){
		return(
			<div>
				<Input ref="input1" />
				<button onClick={this.showData}></button>	
			</div>
		)
	}
}
```

**备注**：此方法虽然简单，但是存在效率问题，已废弃此方法。

## 回调函数方式

```javascript
class Demo entends React.Component{

	const {input2} = this	// 注意此处与字符串方式的区别，没有refs
	
	showData2 = () => {
		console.log(input2.value)
	}
	
	render(){
		return(
			<div>
				<Input ref={c => this.input2 = c} />
				<button onClick={this.showData2}></button>	
			</div>
		)
	}
}
```

**备注**：如果ref回调函数是以`内联函数`的方式定义的，在`更新过程`中，它会被执行两次，第一次传入参数`null`，然后第二次会传入参数DOM元素。这是因为每次渲染时会创建一个新的函数实例，所以React清空旧的ref并且设置新的。通过`将ref的回调函数定义成class的绑定方式`可以避免问题，但大多数情况下是无关紧要的。


**class绑定方式写法：**
```javascript
class Demo entends React.Component{

	const {input2} = this	// 注意此处与字符串方式的区别

	saveInput = (c) => {	// class形式的回调函数
		this.input2 = c
	}
	
	showData2 = () => {
		console.log(input2.value)
	}
	
	render(){
		return(
			<div>
				// 指定class形式的回调函数
				<Input ref={this.saveInput} />	
				<button onClick={this.showData2}></button>	
			</div>
		)
	}
}
```


## createRef方式

```javascript
class Demo entends React.Component{

	// React.createRef调用后可以返回一个容器
	// 该容器可以存储被ref标识的节点,该容器“专人专用”，一一对应
	myRef = React.createRef()
	
	showData3 = () => {
		console.log(this.myRef.current.value)	
		// 不要忘记.current.value
	}
	
	render(){
		return(
			<div>
				<Input ref={this.myRef} />
				<button onClick={this.showData3}></button>	
			</div>
		)
	}
}
```

**备注**：是写法最复杂同时也是react最推荐的一种方式