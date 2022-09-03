## 父子组件间通信

### 父传子
#### 1. props
父组件向子组件传递`props`,子组件接收

```javascript
// 父组件
function Parent (){
	const [name,setName] = useState('frank')
	<Child value={name} />
}
```

函数组件方式的子组件通过参数接收`props`并使用

```javascript
// 子组件 - 函数组件方式
	
  function Child(props) {
    return (
        <span>接收到的props是{props.value}</span>
    );
  }
```

class组件方式的子组件通过`this.props`调用
```javascript
// 子组件 - class组件方式
class Child extends React.Component{
	render(){
		return (
			<span>接收到的props是{this.props.value}</span>
		)
	}
}
```

#### 2.ref
父组件通过`ref`获取到子组件的实例，可以调用子组件实例中的属性或者函数

```javascript
//子组件
class Child extends Component{
  state={
    name:"admin"
  }
  childClickHandle=(city)=>{
    this.setState({
      address:city
    })
  }
  render(){
    return (
      <div>name:{this.state.name},address:{this.state.address}</div>
    )
  }
}

//父组件
class Parent extends Component{
  constructor(){
    super();
    //通过 createRef() 生成ref
    this.childComp=createRef()
  }
 
  clickHandle=()=>{
    //调用子组件的方法，并传递数据
    this.childComp.current.childClickHandle("beijing");
  }

  render(){
    return (
      <div>
        <button onClick={this.clickHandle}>按钮</button>
        //给子组件设置ref属性
        <Child ref={this.childComp}></Child>
      </div>
    )
  }
}

ReactDOM.render(
  <Parent/>,
  document.getElementById('root')
);

```

### 子传父
由于react遵循`单向数据流`的原则，因此子组件应避免修改通过`props`得到的值。

正确的处理方式为父组件将`回调函数`作为`props`传给子组件，子组件通过调用`props`中的方法，将数据传回父组件

```javascript
class Parent1 extends React.Component {
  state={
    ParentMsg:''
  }
  //父组件定义回调函数
  getChildMsg = (msg) =>{
    console.log('接收到子组件数据：',msg)
    this.setState({
        ParentMsg:msg
    })
  }
  
  render() {
    return (
      <div className="ContextTest" style={{color:'red'}}>
        子组件：{this.state.ParentMsg}
        //将该函数作为属性的值，传递给子组件
        <Child1 getMsg={this.getChildMsg}/>
      </div>
    )
  }
}

class Child1 extends React.Component {
    state={
        clidMsg:"老李"
    }
    //子组件通过 props 调用回调函数并将子组件的数据作为参数传递给回调函数
    handleClick = () => {
        this.props.getMsg(this.state.clidMsg) 
    }
    render() {
    return (
        <div className="node" style={{color:'blue'}}>
            <button onClick={this.handleClick}>点我，给父组件传递数据</button>
        </div>
        )
    }
}
export default Parent1;
```

## 祖孙组件间通信（跨级通信）
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c8b0a6f67414c0f963044eaf7d7fe03.png)

组件A与组件C之间即跨级通信

### 1. 使用props层层传递
此方法不适用于三层以上的组件传递

### 2. context
- 创建一个`context`对象 `const XxxContext = React.createContext();`
- 渲染子组件时，在外层包裹`XxxContext.Provider`，通过`value`给后代组件传值

```javascript
	<XxxContext.Provider value={数据}>
		子组件
    </XxxContext.Provider>
```
- 后代组件读取数据时，需要使用`XxxContext.Consumer`声明接收

```javascript
	  <XxxContext.Consumer>
	    {
	      value => ( // value就是context中的value数据
	        要显示的内容
	      )
	    }
	  </XxxContext.Consumer>
```

代码示例:(A-B-C是祖-父-孙的关系)

```javascript
import React, { Component } from 'react'

//创建Context对象
const MyContext = React.createContext()

export default class A extends Component {
	state = {username:'tom',age:18}
	render() {
		const {username,age} = this.state
		return (
			<div className="parent">
				<h3>我是A组件</h3>
				<h4>我的用户名是:{username}</h4>
				<MyContext.Provider value={{username,age}}>
					<B/>
				</MyContext.Provider>
			</div>
		)
	}
}

class B extends Component {
	render() {
		return (
			<div className="child">
				<h3>我是B组件</h3>
				<C/>
			</div>
		)
	}
}

function C(){
	return (
		<div className="grand">
			<h3>我是C组件</h3>
			<h4>我从A组件接收到的用户名:
			<MyContext.Consumer>
				{value => {
				 	return {
						`${value.username},年龄是${value.age}`
					}
				}
			</MyContext.Consumer>
			</h4>
		</div>
	)
}
```

## 兄弟组件间通信
兄弟组件间通常利用`props`以父组件作为中间点传递，

![在这里插入图片描述](https://img-blog.csdnimg.cn/de15824093ef45a48640d6539d412f0e.png)
## 任意组件间通信
通常使用`redux`作为集中状态管理工具，处理无关联或者嵌套层级深的组件间通信

`redux`详细内容可见[redux入门详解](https://github.com/dax1-zyh/Steps-to-Master/blob/main/React/redux%E5%85%A5%E9%97%A8%E8%AF%A6%E8%A7%A3.md)