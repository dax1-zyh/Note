##  hooks的作用
在react16.8版本之后，由于hooks的出现，函数组件得到了扩展，完全不使用"类"，就能写出一个全功能的组件。

		React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。

## 一. useState
`useState()`用于为函数组件引入状态`（state）`。纯函数不能有状态，所以把状态放在钩子里面。

```javascript
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量,初始值为0，后续通过setCount改变它能让视图重新渲染
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

`useState()`函数接受状态的初始值，作为参数。该函数返回一个数组，数组的第一个元素是一个变量（上例是`count`），指向状态的当前值。第二个元素是一个函数（上例是`setCount`），用来更新该状态，约定是set前缀加上状态的变量名

### useState总结
- `useState`让函数组件也可以有`state`状态, 并进行状态数据的读写操作
- 语法: `const [xxx, setXxx] = useState(initValue)`  
-  `useState()`说明:
   - 参数: 第一次初始化指定的值在内部作缓存
   - 返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
- `setXxx()`2种写法:
   - `setXxx(newValue)`: 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
   - `setXxx(value => newValue)`: 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值

## 二. useEffect

`useEffect()`用来引入具有副作用的操作，最常见的就是向服务器请求数据。以前，放在`componentDidMount`里面的代码，现在可以放在`useEffect()`。

```javascript
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```

上面用法中，`useEffect()`接受两个参数。第一个参数是一个函数，异步操作的代码放在里面。第二个参数是一个数组，用于给出 `Effect` 的依赖项，只要这个数组发生变化，`useEffect()`就会执行。

### useEffect的作用
只要是`副作用(side effect)`，都可以使用`useEffect()`引入。它的常见用途有下面几种。

- 获取数据
- 事件监听或订阅
- 改变 DOM
- 输出日志

可以把`useEffect`看做如下三个生命周期的组合
- `componentDidMount()`
- `componentDidUpdate()`
- `componentWillUnmount()` 

### useEffect的返回值
`useEffect()`允许返回一个函数，在组件卸载时，执行该函数，清理副作用。如果不需要清理副作用，`useEffect()`就不用返回任何值。

```javascript
	React.useEffect(()=>{
		let timer = setInterval(()=>{
			setCount(count => count+1 )
		},1000)
		return ()=>{
			clearInterval(timer)
		}
	},[])
```
如上例，`useEffect()在`组件加载时设置了一个定时器，并且返回一个清理函数，在组件卸载时清空定时器。

此处相当于执行了`componentWillUnmount()`生命周期钩子。

### useEffect的第二个的参数
- 省略，省略时相当于组件全局作为依赖，每次组件渲染时都会执行`useEffect()`
- 为`[]`，相当于没有依赖，只会在组件挂载时执行一次`useEffect()`，相当于`componentDidMount()`生命周期钩子
- `[x,xxx]`，数组内的变量为依赖，当依赖发生变化时，都会执行`useEffect()`，相当于`componentDidUpdate()`生命周期钩子

### 多个副作用的注意点

使用`useEffect()`时，有一点需要注意。如果有多个副效应，应该调用多个`useEffect()`，而不应该合并写在一起。

```javascript
function App() {
  const [varA, setVarA] = useState(0);
  const [varB, setVarB] = useState(0);

  useEffect(() => {
    const timeout = setTimeout(() => setVarA(varA + 1), 1000);
    return () => clearTimeout(timeout);
  }, [varA]);

  useEffect(() => {
    const timeout = setTimeout(() => setVarB(varB + 2), 2000);

    return () => clearTimeout(timeout);
  }, [varB]);

  return <span>{varA}, {varB}</span>;
}
```

## 三. useReducer

`useReducers()`钩子用来引入 `Reducer` 功能。

用法： `const [state, dispatch] = useReducer(reducer, initialState);`

它接受 `Reducer` 函数和状态的初始值作为参数，返回一个数组。数组的第一个成员是状态的当前值，第二个成员是发送 `action` 的`dispatch`函数。

下面是一个计数器的例子。用于计算状态的 `Reducer` 函数如下。

```javascript
const myReducer = (state, action) => {
  switch(action.type)  {
    case('countUp'):
      return  {
        ...state,
        count: state.count + 1
      }
    default:
      return  state;
  }
}
```

组件代码如下

```javascript
function App() {
  const [state, dispatch] = useReducer(myReducer, { count:   0 });
  return  (
    <div className="App">
      <button onClick={() => dispatch({ type: 'countUp' })}>
        +1
      </button>
      <p>Count: {state.count}</p>
    </div>
  );
} 
```


## 四. useRef
`useRef` 返回一个可变的 `ref` 对象，其 `current` 属性被初始化为传入的参数`initialValue`。

写法：`const refContainer = useRef(initialValue);`

用法与`createRef()`一致

```javascript
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```


## 参考文档
[React Hooks教程之基础篇](https://juejin.cn/post/6844904017596776461#heading-24)

[React Hooks 入门教程](https://www.ruanyifeng.com/blog/2019/09/react-hooks.html)