## 报错场景
vue项目启动，`npm run dev`之后报错如图：

```javascript
【Error: listen EADDRINUSE: address already in use :::8099】
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/914620e79a514ebeb48b972c718ad064.png)



## 原因分析

启动服务的地址端口已被占用，需要kill目标进程

## 解决方案
终端输入`netstat -ano`

![在这里插入图片描述](https://img-blog.csdnimg.cn/965b7fb81f31477b929c686e02fb62a6.png)

找到端口为8099的进程，记住PID:14724

终端输入`tskill 14724`

再次运行`npm run dev`，成功启动。