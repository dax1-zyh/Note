## MVC

 - 模型（Model）：数据保存
 - 视图（View）：用户界面。
 - 控制器（Controller）：业务逻辑



**通信方式：**

![image-20220217172408247](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20220217172408247.png)



**通信流程：**

1. 用户通过UI界面的交互触发View响应，View发送指令给Controller
2. Controller完成业务逻辑之后，要求Model更新数据
3. Model将新数据发送给View，要求View更新，更新后用户得到反馈

**典型框架：react**

**注意：MVC所有通信都是单向的**





## MVP

MVP模式将MVC的Controller更名为Presenter



**通信方式：**

![image-20220217172855685](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20220217172855685.png)



**注意：**

1. 各部分之间的通信都是**双向**的
2. View和Model不发生联系，都通过Presenter传递
3. View 非常薄，不部署任何业务逻辑，称为"**被动视图**"（Passive View），即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。



## MVVM

MVVM 模式将 Presenter 改名为 ViewModel（视图模型），基本上与 MVP 模式完全一致。



**通信方式**

![image-20220217173230943](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20220217173230943.png)

唯一的区别是，View和ViewModel之间是**双向绑定**



**通信流程：**

1. View 接收用户交互请求
2. View 将请求转交给ViewModel
3. ViewModel 操作Model数据更新
4. Model 更新完数据，通知ViewModel数据发生变化
5. ViewModel 更新View数据



**典型框架：Vue、Angular**



**以Vue为例**

- **model**：对应data中的数据
- **v**：模板、UI界面
- **VM**：Vue实例对象



**MVVM与MVC最大的区别在于MVVM中的ViewModel会自动更新视图**



## 参考文档

[MVC，MVP 和 MVVM 的图示 - 阮一峰的网络日志 (ruanyifeng.com)](https://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)