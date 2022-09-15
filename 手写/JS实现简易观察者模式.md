```javascript
// 被观察者或消息发布主体
class Subject {
    constructor(name) {
        this.name = name
        this.observers = [] // 存放观察者
        this.state = '' // 发布的消息
    }
    subscribe(observer){
        this.observers.push(observer)
    }
    unsubscribe(observer){
        this.observers.filter(item => {
            return item !== observer
        })
    }
    setState(newState){ // 改变被观察者的状态，并通知关注的用户
        this.state = newState
        this.observers.forEach(item => {
            item.update(newState)
        })
    }
}

// 观察者
class Observer {
    constructor(name){
        this.name = name
    }
    update(newState) {
        console.log(this.name + '接收到了' + newState)
    }
}

let sub = new Subject('发布主题')
let a = new Observer('用户A')
let b = new Observer('用户B')

// 订阅观察者
sub.add(a)
sub.add(b)

// 发布消息
sub.setState('新发布内容')
// 用户A 接收到了 新发布内容
// 用户B 接收到了 新发布内容

```

