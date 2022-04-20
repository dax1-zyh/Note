> 很长时间了，一直把splice和slice混淆，于是整理一下资料，常看看。

资料来源
**slice** [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
**splice** [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

## 英语小课堂

我觉得总混淆的原因其中就是单词释义没有记清楚，事实证明如果单词理解正确的话，理解就很容易。
![在这里插入图片描述](https://img-blog.csdnimg.cn/82d4268ebf044592a7678d889b895b1f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARGF4MV8=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7b455914496a4666965bd648f81dc65d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARGF4MV8=,size_20,color_FFFFFF,t_70,g_se,x_16)
**slice：薄片，切开**
**splice：连接**

----------
## 是否改变原数组？有返回值吗？


**slice方法不会改变原数组，slice方法返回新截取的数组**

**splice方法改变原数组，splice方法返回截取到的数组**

-----------

## Array.prototype.slice()
**slice() 方法返回一个新的数组对象**，这一对象是一个由 **begin** 和 **end** 决定的原数组的浅拷贝（**包括 begin，不包括end**）。**原始数组不会被改变。**

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2));
// expected output: Array ["duck", "elephant"]

console.log(animals.slice(2, -1));
// expected output: Array ["camel", "duck"]

```

**语法**


```javascript
arr.slice([begin[, end]])
```


**参数**

 - **begin（可选）**
 提取起始处的索引（从 0 开始），从该索引开始提取原数组元素。
 如果该参数为**负数**，则表示从原数组中的**倒数第几个元素**开始提取，slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
 如果省略 begin，则 slice 从索引 0 开始。
 如果 begin 超出原数组的索引范围，则会返回空数组。
 - **end（可选）**
提取终止处的索引（从 0 开始），在该索引处结束提取原数组元素。
slice 会提取原数组中索引从 begin 到 end 的所有元素（包含 begin，但不包含 end）。
slice(1,4) 会提取原数组中从第二个元素开始一直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
如果该参数为**负数**， 则它表示在原数组中**的倒数第几个元素结束**抽取。 slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
如果 end 被省略，则 slice 会一直提取到原数组末尾。
如果 end 大于数组的长度，slice 也会一直提取到原数组末尾。

**返回值**

 - 一个含有被提取元素的新数组。


------------------------

## Array.prototype.splice()
splice() 方法通过**删除或替换现有元素或者原地添加新的元素**来修改数组,并以数组形式返回被修改的内容。**此方法会改变原数组**。

```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at index 1
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');
// replaces 1 element at index 4
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]

```

**语法**

```javascript
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```


**参数**

 - **start**
指定修改的开始位置（从0计数）。
如果**超出了数组的长度**，则从数组**末尾**开始添加内容；
如果是**负值**，则表示从数组**末位开始的第几位**（从-1计数，这意味着-n是倒数第n个元素并且等价于array.length-n）；如果**负数的绝对值大于数组的长度**，则表示开始位置为**第0位**。

 - **deleteCount （可选）**
整数，表示**要移除的数组元素的个数**。
如果 **deleteCount 大于 start 之后的元素的总数**，则从 start 后面的元素**都将被删除**（含第 start 位）。
如果 deleteCount 被**省略**了，或者它的值大于等于array.length - start(也就是说，如果它大于或者等于start之后的所有元素的数量)，那么start之后数组的所有元素都会被删除。
如果 **deleteCount 是 0 或者负数**，则**不移除元素**。这种情况下，至少应添加一个新元素。

 - **item1, item2, ... （可选）**
要添加进数组的元素,从start 位置开始。如果不指定，则 splice() 将只删除数组元素。

**返回值**

由**被删除的元素组成的一个数组**。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。