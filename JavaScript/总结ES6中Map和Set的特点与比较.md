## Map（映射）
Map 是键值对的集合，为JS带来了真正的键值存储机制。

#### 主要特点
 - Map允许任何类型的键
 - Map具有极快的查找速度(存储键值较少的情况下)
 - Map不适用于存储数量很多的键值对

#### 选择Object还是Map

 - 内存占用
给定固定大小的内存，Map大约可以比Object多存储50%的键值对

- 插入性能
如果代码涉及大量插入操作，那么显然Map性能更佳

- 查找速度
存储键值少的情况下，Map查找速度更优；如果代码涉及大量查找操作，那么某些情况下可能选择Object更好一些

- 删除性能
对大多数浏览器引擎来说，Map的 delete()操作都比插入和查找更快。如果代码涉及大量删除操作，那么毫无疑问应该选择Map

#### 方法和属性
- new Map() —— 创建 map。
- map.set(key, value) —— 根据键存储值。
- map.get(key) —— 根据键来返回值，如果 map 中不存在对应的 key，则返回 undefined。
- map.has(key) —— 如果 key 存在则返回 true，否则返回 false。
- map.delete(key) —— 删除指定键的值。
- map.clear() —— 清空 map。
- map.size —— 返回当前元素个数。

```javascript
let map = new Map();

map.set('1', 'str1');   // 字符串键
map.set(1, 'num1');     // 数字键
map.set(true, 'bool1'); // 布尔值键

// 还记得普通的 Object 吗? 它会将键转化为字符串
// Map 则会保留键的类型，所以下面这两个结果不同：
alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```

--------

## WeakMap 
WeakMap是一组键值对的集合，其中的键是**弱引用**对象，而值可以是任意。

WeakMap弱引用的只是键名，而不是键值，键值依然是正常使用的。

### 特点
- 只接受对象作为键名
- 键名是弱引用，可以被垃圾回收
- 不可遍历
- API和Map相同

-------

## Set（集合）
Set与Map类似，是一组Key的集合，但不存储Value，由于Key不能重复，因此他最大的特点是**所有的元素都是唯一的**。

#### 方法和属性
- new Set(iterable) —— 创建一个 set，如果提供了一个 iterable 对象（通常是数组），将会从数组里- 面复制值到 set 中。
- set.add(value) —— 添加一个值，返回 set 本身
- set.delete(value) —— 删除值，如果 value 在这个方法调用的时候存在则返回 true ，否则返回 false。
- set.has(value) —— 如果 value 在 set 中，返回 true，否则返回 false。
- set.clear() —— 清空 set。
- set.size —— 返回元素个数。


#### 使用技巧：数组去重
因为Set的特点为集合中的元素是唯一的，因此可以用于数组去重。

```javascript
const arr = [1,2,2,3,4,4,4,5,6,7,7]
const arr2 = [... new Set(arr2)] // [1,2,3,4,5,6,7]
```

## WeakSet
WeakSet相较于Set，区别在于它只接收对象作为成员，且成员都是弱引用

### 特点
- 成员都是对象
- 成员都是弱引用，可以被垃圾回收机制回收
- 不可遍历
- API和Set相同

