```javascript
console.log(Function.prototype.hasOwnProperty('call')) //true
console.log(Function.prototype.hasOwnProperty('apply')) //true
console.log(Function.prototype.hasOwnProperty('bind')) //true
```
上面代码中，都返回了true，表明三种方法都是继承自Function.prototype的。当然，普通的对象，函数，数组都继承了Function.prototype对象中的三个方法，所以这三个方法都可以在对象，数组，函数中使用。三个方法的作用都是一样的，**简单来说就是改变当前使用该方法的对象中的this指向。**



## this的指向

call/apply/bind方法的第一个参数都是 this 指定的值；如果第一个参数为空，或参数为 null或 undefined 或 this，则等同于指向全局对象window。

```javascript
window.color='red';
var obj={color:"blue"};
function sayColor(){
console.log(this.color);
};
sayColor(); //red
sayColor.call(this); //red
sayColor.call(window); //red
sayColor.call(obj); //blue
sayColor.apply(obi); //blue
sayColor.bind(obj)(); //blue
```



## call apply bind的区别

 - call后面的参数是arguments 或其他参数
 - apply 的第二个参数必须是数组,内含所有其他参数
 - 由于bind返回的仍然是一个函数，所以我们还可以在调用的时候再进行传参。



## 总结



 **call、apply与bind的差别**
call和apply改变了函数的this上下文后便执行该函数,而bind则是返回改变了上下文后的一个函数。

**call、apply的区别**
他们俩之间的差别在于参数的区别，call和apply的第一个参数都是要改变上下文的对象，而call从第二个参数开始以参数列表的形式展现，apply则是把除了改变上下文对象的参数放在一个数组里面作为它的第二个参数。