## ref vs toRef

 - 如果利用ref函数将某个对象中的属性变成响应式数据，对其进行修改是不会影响原始数据的。
 - 利用toRef进行上方操作，对数据的修改是响应式的

## toRef vs toRefs
- toRef：创建一个**ref对象**，其value值指向另一个对象中的某个属性
- toRef语法：`const name = toRef(object , 'property')`
- toRefs：功能与toRef一致，但可以批量创建多个ref对象
- toRefs语法：`toRefs(object)`
- 他们的作用都是将响应式对象中的某个属性**单独**提供给外部使用

## toRefs与扩展运算符(...)
在setup()中可以用`return { ...toRefs(object)}`的方式，将整个响应式对象object的所有属性提供给外部使用。