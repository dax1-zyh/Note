## redux定义
- redux是一个专门用于做`状态管理`的`js库`
- 它可以用于原生js，react，vue，angular等项目中，但基本与react配合使用
- 作用：集中式管理react应用中多个组件`共享`的状态

## 什么情况下需要redux
- 某个组件的状态，需要让其他组件可以随时拿到（共享）
- 一个组件需要改变另一个组件的状态（通信）

## redux流程图
![在这里插入图片描述](https://img-blog.csdnimg.cn/05ab4dd3fa8b43bf95339c85cda0b1e0.png)

## 三大原则
- 单一数据源
  - 整个应用的`state`被储存在一棵`object tree`中，并且这个`object tree`只存在于唯一一个`store`中。
- `State`是只读的
  - 唯一改变`state`的方法就是`触发(dispatch)action`
- 使用`纯函数`来执行修改



## Action Creators
作用：创建`action`对象（`action`可以不止一个）

```javascript
const addStuduent = data => ({ type:ADD_STUDENT, data })
```

## Action对象
 - 动作的对象
 - 包含两个属性：
   - `type`：标识属性，值为字符串，唯一，必要属性
   - `data`：数据属性，值类型任意，可选属性
- 例子：`{ type:'ADD_STUDENT' , data:{name:'tom', age:18} }` 

调用方法：`store.dispatch(addStudent(data))`

## 异步Action
由于`Action Creators`创建的必须是一个`action对象`，而普通对象不能创建一个异步任务，因此创建异步`Action`的最佳做法是使用`Redux Thunk中间件`。

通过使用指定的`middleware(中间件)`，`Action Creators`除了返回`action对象`外还可以返回函数。这时，这个 `action Creator`就成为了`thunk`。

当 `Action Creators`返回函数时，这个函数会被`Redux Thunk middleware`执行。这个函数并不需要保持纯净；它还可以带有副作用，包括执行异步API请求。这个函数还可以`dispatch action`，就像`dispatch`前面定义的同步`action`一样。

使用方法：
- 从`redux-thunk`引入`thunk`
- 从`redux`引入`applyMiddleware`
- `createStore()`时，添加`applyMiddleware(thunk)`方法

```javascript
// redux/store.js

//引入createStore，专门用于创建redux中最为核心的store对象
import {createStore,applyMiddleware} from 'redux'
//引入为Count组件服务的reducer
import countReducer from './reducers/count'
//引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'
//暴露store
export default createStore(countReducer,applyMiddleware(thunk))
```

使用中间件之后，`Action Creators`不仅可以创建`action`对象，也可以创建异步`action`函数，一般在`action`函数中会调用同步`action`

```javascript
// redux/actions/count.js

import {INCREMENT,DECREMENT} from './constant'

//同步action，就是指action的值为Object类型的一般对象
export const createIncrementAction = data => ({type:INCREMENT,data})
export const createDecrementAction = data => ({type:DECREMENT,data})

//异步action，就是指action的值为函数,异步action中一般都会调用同步action，异步action不是必须要用的。
export const createIncrementAsyncAction = (data,time) => {
	return (dispatch)=>{
		setTimeout(()=>{
			dispatch(createIncrementAction(data))
		},time)
	}
}
```

ps：中间件方法不是必要的，可以在组件调用`store.dispatch()`时，在外层添加定时器函数，达到异步的效果。

## Reducer
- 作用：用于初始化状态、加工状态
- 加工时，根据旧的`state`和`action`，产生新的state`纯函数`

```javascript
const initState = 0 //初始化状态
export default function countReducer(preState=initState,action){
	//从action对象中获取：type、data
	const {type,data} = action
	//根据type决定如何加工数据
	switch (type) {
		case 'increment': 
			return preState + data
		case 'decrement': 
			return preState - data
		default:
			return preState
	}
}
```

注意点：
- Redux首次执行时，`state`为`undefined`，此时我们可借机设置并返回应用的初始`state`。
- Reducer函数是一个`纯函数`，不允许修改原始`state`
- 在`default`情况下返回原始的`state`。遇到未知的`action`时，一定要返回原始的`state`。

## 合并Reducers
每个`reducer`只负责管理全局`state`中它负责的一部分。每个`reducer`的`state`参数都不同，分别对应它管理的那部分`state`数据

Redux 提供了`combineReducers()`来合并所有的`reducer`

```javascript
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

上面的写法等同于

```javascript
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```


## Store
- 将`state`、`action`、`reducer`联系在一起的`对象`
- Redux应用只有一个单一的`store`
- 如何得到此对象
  - `import {createStore} from 'redux'`
  - `import reducer from './reducers'`
  - `const stroe = createStore(reducer)`，`createStore`的第二个参数是可选的，用于设置`state`初始状态
- 3个api
  - `getState()`:得到`state`
  - `dispatch(action)`:分发`action`,触发`reducer`调用，产生新的`state`
  - `subscribe(listener)`:注册监听，当产生了新的`state`时，自动调用

Redux 应用只有一个单一的`store`。当需要拆分数据处理逻辑时，你应该使用`reducer组合` 而不是创建多个 `store`。使用 `combineReducers()` 将多个`reducer`合并成为一个。



## react-redux模型
![在这里插入图片描述](https://img-blog.csdnimg.cn/de6c18b8fbb74081a5aeb8ecb78646e9.png)

## 容器组件和展示组件（UI组件）
react-redux是基于容器组件和展示组件分离的开发思想

![在这里插入图片描述](https://img-blog.csdnimg.cn/afe27333bfd242ebae18c01389fcc521.png)

- 容器组件：负责和redux通信，将结果交给展示组件
- 展示组件：不能使用任何redux的api，只负责页面的呈现、交互等

## 创建容器组件
官方不建议手写一个容器组件，而是使用react-redux提供的`connect()`方法来生成

`connect()`方法优势：对react做了性能优化来避免很多不必要的重复渲染

 

 1. 在使用`connect()`方法之前需要先定义`mapStateToProps`和`mapDispatchToProps`两个函数

- `mapStateToProps`指定当前`state`映射到展示的`props`中，返回值是一个对象
- `mapDispatchToProps`接收`dispatch()`方法并返回期望注入到展示组件的props中的回调方法，返回值是一个对象或者函数

2. 接着使用`connect()`创建容器组件

**以Count组件为例：**

```javascript
// containers/Count/index.jsx

import React, { Component } from 'react'
import {
	createIncrementAction,
	createDecrementAction,
	createIncrementAsyncAction
} from '../../redux/count_action'
import {connect} from 'react-redux'

//定义展示组件
class Count extends Component {
	state = {}
	increment = ()=>{
		const {value} = this.selectNumber
		this.props.jia(value*1)
	}
	decrement = ()=>{
		const {value} = this.selectNumber
		this.props.jian(value*1)
	}
	incrementIfOdd = ()=>{
		const {value} = this.selectNumber
		if(this.props.count % 2 !== 0){
			this.props.jia(value*1)
		}
	}
	incrementAsync = ()=>{
		const {value} = this.selectNumber
		this.props.jiaAsync(value*1,500)
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{this.props.count}</h1>
				<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>&nbsp;
			</div>
		)
	}
}

/* 
	1.mapStateToProps函数返回的是一个对象；
	2.返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
	3.mapStateToProps用于传递状态
*/
function mapStateToProps(state){
	return {count:state}
}

/* 
	1.mapDispatchToProps函数返回的是一个对象；
	2.返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
	3.mapDispatchToProps用于传递操作状态的方法
*/
function mapDispatchToProps(dispatch){
	return {
		jia:number => dispatch(createIncrementAction(number)),
		jian:number => dispatch(createDecrementAction(number)),
		jiaAsync:(number,time) => dispatch(createIncrementAsyncAction(number,time)),
	}
}

//使用connect()()创建并暴露一个Count的容器组件
export default connect(mapStateToProps,mapDispatchToProps)(Count)
```

这里可以进行代码优化，不用事先声明`mapStateToProps`和`mapDispatchToProps`，直接将两个方法的返回值带入`connect()`方法
```javascript
export default connect(
	// mapStateToProps的简写
	state => ({count:state})，
	// mapDispatchToProps的简写
	{
		jia:createIncrementAction,
		jian:createDecrementAction,
		jiaAsync:createIncrementAsyncAction,
	}
)(Count)
```

## 传递Store
所有容器组件都可以访问`Store`，所以可以手动监听它。一种方式是把它以`props`的形式传入到所有容器组件中。但这太麻烦了，因为必须要用`Store`把展示组件包裹一层，仅仅是因为恰好在组件树中渲染了一个容器组件。

建议的方式是使用指定的react-redux组件 `<Provider>` 来魔法般的让所有容器组件都可以访问`Store`,而不必显式地传递它。只需要在渲染根组件时使用即可。

```javascript
// src/index.js

import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

## redux和react-redux的区别
- redux是进行状态管理的js库，react-redux是react绑定库
- redux组件直接访问`Store`，react-redux组件通过`connect()`连接容器组件和展示组件
- 获取`State`的方式不一样，redux使用`store.getState()`，react-redux使用`mapStateToProps`函数传递
- 触发`action`的方式不一样，redux使用`store.dispatch()`，react-redux使用`mapDispatchToProps`函数传递