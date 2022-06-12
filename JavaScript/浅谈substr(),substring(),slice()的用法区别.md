## 1 三者区别
- `substr(start,length)`返回从指定下标开始的长度为`length`的字符，可以为负数

- `substring(start,end)`返回指定下标间的字符，包含start，不包含end

- `slice(start,end)`返回指定下标间的字符，包含start，不包含end

  

## 2 substring和slice



### 2.1 substring MDN



![在这里插入图片描述](https://img-blog.csdnimg.cn/85181be21c1f46f3a1049874e35b607a.png)



### 2.2 slice MDN



![在这里插入图片描述](https://img-blog.csdnimg.cn/0adbc16136ce4873b65ac5f25a2a5070.png)



### 2.3 区别

`substring`和`slice`的主要区别在于，`substring`任一参数小于0或者为Nan，则当作0。`slice`的参数为负数，则它表示在原数组中的倒数第几个元素。




## 3. 举个栗子

```javascript
var stringValue = "hello world";

console.log(stringValue.slice(3));          //”lo world”
console.log(stringValue.substring(3));      //”lo world”
console.log(stringValue.substr(3));        //”lo world”

console.log(stringValue.slice(3,7));         //”lo w”
console.log(stringValue.substring(3,7));    //”lo w”
console.log(stringValue.substr(3,7));       //”lo worl”

console.log(stringValue.slice(-3));         //"rld"　从后往前数3个开始
console.log(stringValue.substring(-3));     //"hello world" 为负，默认从0开始
console.log(stringValue.substr(-3));        //"rld"

console.log(stringValue.slice(3,-4));       //”lo w”　下标从3开始到-4(从后往前数4个)
console.log(stringValue.substring(3,-4));   //”hel”　
console.log(stringValue.substr(3,-4));      //””　长度为负，默认不显示
```