## 前言
react 生命周期指的是组件从创建到卸载的整个过程，每个过程都有对应的钩子函数会被调用，它主要有以下几个阶段：

- 挂载阶段  ：组件实例被创建和插入 DOM 树的过程
- 更新阶段  ：组件被重新渲染的过程
- 卸载阶段  ：组件从 DOM 树中被删除的过程

下文分别从上述三个阶段解读react的生命周期

## 旧版生命周期

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df9e862e253e4c8ebbba5d2e245b7989~tplv-k3u1fbpfcp-zoom-1.image)



- 挂载阶段：`componentWillMount` - `render` - `componentDidMount`
- 更新阶段：`componentWillReceiveProps` - `shouldComponentUpdate` - `componentWillUpdate` - `render` - `componentDidUpdate`
- 卸载阶段：`componentWillUnmount`

##  挂载阶段
### componentWillMount

`componentWillMount`发生在`render`之前，此时还没有挂载DOM，有可能会被执行多次

### componentDidMount

常用的钩子，在组件挂载成功之后调用，该过程组件已经成功挂载到了真实 DOM 上。

一般在这个钩子中做一些初始化的事，例如：`开启定时器、发送网络请求、订阅消息`

```javascript
componentDidMount(){
  fetch('https://api.github.com/users').then(res=>res.json()).then(users=>{
    console.log(users);
    this.setState({users});
  });
}
```

## 更新阶段

### componentWillReceiveProps(newProps)

在`props`发生改变（父组件重新`render`或者更新`props`）时调用，这个钩子提供对 `props` 的监听，在 `props` 发生改变后，相应改变组件的一些 `state`。在这个方法中改变 `state` 不会二次渲染，而是直接合并 state。


### shouldComponentUpdate(nextProps, nextState)
这个钩子相当于一个阀门，返回一个布尔值，决定是否更新组件。

由于 react 父组件更新，必然会导致子组件更新，因此我们可以在子组件中通过手动对比 `props` 与 `nextProps`，`state` 与 `nextState` 来确定是否需要重新渲染子组件，如果需要则返回`true`，不需要则返回 `false`。该函数默认返回 `true`。

### componentWillUpdate
组件更新前调用的钩子

### componentDidUpdate
组件更新完成后调用的钩子

- 因为组件已经重新渲染了所以这里可以对组件中的 DOM 进行操作；
- 在比较了 `this.props` 和 `nextProps` 的前提下可以发送网络请求。

```javascript
componentDidUpdate(prevProps, prevState, snapshot) {
	if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```

## 卸载阶段

### componentWillUnmount
卸载阶段唯一的生命周期钩子，通常在这里处理一些善后工作，例如关闭定时器、取消监听等等

## 旧版生命周期执行流
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a552d144bb854b2390fe303ed6a023aa~tplv-k3u1fbpfcp-zoom-1.image)

## 新版生命周期
react 打算在17版本推出新的 `Async Rendering（异步渲染）`，提出一种可被打断的生命周期，而可以被打断的阶段正是实际 `dom` 挂载之前的虚拟 `dom` 构建阶段，也就是要被去掉的三个生命周期。

- 废弃了三个生命周期：`componentWillMount`,`componentWillUpdate`,`componentWillUnmount`
- 新增了两个生命周期：`static getDerivedStateFromProps(nextProps, prevState)`，`getSnapshotBeforeUpdate(prevProps, prevState)`

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06391bb1e06843dd85ff970c10221dbe~tplv-k3u1fbpfcp-zoom-1.image)



- 挂载阶段：`getDerivedStateFromProps` - `render` - `componentDidMount`
- 更新阶段：`getDerivedStateFromProps` - `shouldComponentUpdate` - `render` - `getSnapShotBeforeUpdate` - `componentDidUpdate`
- 卸载阶段：`componentWillUnmount`

## static getDerivedStateFromProps(nextProps, prevState)
该生命周期在 `render`方法之前调用，在初始化和后续更新都会被调用

它接收两个参数，一个是传进来的 `nextProps` 和之前的 `prevState`。他应该返回一个对象来更新 `state`。如果返回 `null` 则不更新任何内容。

这个生命周期主要为我们提供了一个可以在组件实例化或 `props`、`state` 发生变化后根据 `props` 修改 `state` 的一个时机。

## getSnapshotBeforeUpdate(prevProps, prevState)
在更新阶段 `render` 后挂载到真实 `DOM` 前进行的操作，它使得组件能在发生更改之前从 `DOM` 中捕获一些信息。此组件返回的任何值将作为 `componentDidUpdate` 的第三个参数。

```javascript
  getSnapshotBeforeUpdate(prevProps, prevState){
    return "getSnapshotBeforeUpdate";
  }

  // 组件更新成功钩子
  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log(snapshot); // "getSnapshotBeforeUpdate"
  }
```

## 父子组件生命周期执行顺序
### 父子组件初始化
- 父组件 `constructor`
- 父组件 `getDerivedStateFromProps`
- 父组件 `render`
- 子组件 `constructor`
- 子组件 `getDerivedStateFromProps`
- 子组件 `render`
- 子组件 `componentDidMount`
- 父组件 `componentDidMount`


### 子组件修改自身state
- 子组件 `getDerivedStateFromProps`
- 子组件 `shouldComponentUpdate`
- 子组件 `render`
- 子组件 `getSnapShotBeforeUpdate`
- 子组件 `componentDidUpdate`

### 父组件修改props
- 父组件 `getDerivedStateFromProps`
- 父组件 `shouldComponentUpdate`
- 父组件 `render`
- 子组件 `getDerivedStateFromProps`
- 子组件 `shouldComponentUpdate`
- 子组件 `render`
- 子组件 `getSnapShotBeforeUpdate`
- 父组件 `getSnapShotBeforeUpdate`
- 子组件 `componentDidUpdate`
- 父组件 `componentDidUpdate`

### 卸载子组件
- 父组件 `getDerivedStateFromProps`
- 父组件 `shouldComponentUpdate`
- 父组件 `render`
- 父组件 `getSnapShotBeforeUpdate`
- 子组件 `componentWillUnmount`
- 父组件 `componentDidUpdate`

## 参考文档
[React 框架生命周期（类组件与函数组件）](https://juejin.cn/post/6871728918643081230#heading-7)

[深入详解React生命周期](https://juejin.cn/post/6914112105964634119#heading-25)