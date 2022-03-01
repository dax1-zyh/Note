## 问题复现
今天遇到了一个没想过的问题

```javascript
    const arr = ['1','2','3','4','5']
    const res1 = arr.map((x) => parseInt(x))
    const res2 = arr.map((x) => {parseInt(x)})
    console.log(res1)
    console.log(res2)
```
我以为第2，3两行的输出结果应该是一样的，可是结果是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/e8bd734ca7b14c5598274e70d46c85cd.png)
## 正确理解
经过查阅资料，得出结论，当使用箭头函数时，函数体

 - 不加括号，默认会补充上return关键字
 - 加括号，需要自己手动写上return关键字