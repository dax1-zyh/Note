## DOM （Document Object Model）文档对象模型

## 关于JS事件
- JS程序使用的是事件驱动的设计模式，为一个元素添加事件监听函数，当这个元素的对应事件被触发，那么添加的事件监听函数就会被调用。
- 事件是JS和HTML交互的基础，任何文档或者浏览器窗口发生的交互，都要通过绑定事件进行交互。
- 所有浏览器都支持DOM0级的事件处理程序，使用该方式时，事件处理都是在元素的作用域中运行，因此程序中的this都是指向该元素。

## 什么是DOM0，1，2，3 ？
在W3C协会制定的DOM标准中，DOM标准可以分为DOM1，DOM2，DOM3三个版本。

事实上，DOM0级标准是不存在的，所谓DOM0只是DOM历史坐标中的一个参照点。

- DOM1主要定义的是HTML和XML文档的底层结构
- DOM2和DOM3分别在这个结构的基础上引入更多的交互能力，例如扩展API，也支持了更高级的XML特性

## DOM0
一开始浏览器处理事件的时候只有原始事件模型，例如直接在HTML标签上绑定事件`onclick`

```javascript
<input id="myButton" type="button" value="Press Me" onclick="alert('thanks');">
```

或者获取DOM节点后绑定事件

```javascript
document.getElementById("myButton").onclick = function () {
        alert('thanks');
}
```


在js中html元素都有一个对应的对象，这个对象的属性对应那个html元素的性质，所以可以用js代码添加事件监听函数。

无论用html还是js，都是把一个函数赋值给文档元素，在事件监听函数被调用时候它是作为产生事件的元素的放法调用的，所以this引用的是那个目标元素(例子中的Input对象)。

## DOM1
DOM1级于1998年10月1日成为W3C推荐标准。

DOM1标准中并没有定义事件相关的内容，所以没有所谓的DOM1事件模型

## DOM2
DOM2中，当事件发生在节点时，目标元素的事件处理函数被触发，并且事件传播分为三个阶段进行。即**捕获阶段、事件处理阶段、冒泡阶段**。

![在这里插入图片描述](https://img-blog.csdnimg.cn/7679478ecf8b45af983d27cc1f416b26.png)
- 在捕获阶段，事件从DOM对象沿着文档树向下传播给节点。如果目标的任何一个父元素注册了事件监听函数，那么事件传播的过程中就会运行这些函数
- 在事件处理阶段，运行注册在目标节点上的事件监听函数
- 在冒泡阶段，事件将从目标元素向上传播回DOM对象（与捕获阶段相反）。

### 事件对象
在触发DOM上的某个事件时，会产生一个事件对象`event`，这个对象中包含所有与事件相关的信息。

### DOM2注册和销毁事件监听函数
DOM2事件模型中，通过调用对象的`addEventListner()`方法为元素设置事件监听函数，即只有通过这个API注册的函数才能在触发上述事件传播的三个阶段。

**`addEventListener`参数**

- 第一个参数为`String`类型，是调用事件的名称，与DOM0级不同的是不需要前缀`on`，例如要注册`click事件`就传入`click`而不是`onclick`
- 第二个参数是回调函数，在调用的时候JS回传给函数一个`event对象`，这个对象包含有关事件的所有细节，如果调用这个对象的`stopPropagation()`方法，则会组织事件进一步传播。
- 第三个参数为`boolean`类型，`true`表示事件监听函数能够在三个阶段中的任何一个阶段捕捉到事件，`false`表示事件监听函数不能再捕获阶段捕获到事件（表现同DOM0）。

使用`removeEventListener()`方法来删除事件处理程序。

### DOM2中的event对象
- **type**:发生的事件的类型，例如"click", "mouseover"
- **target**:发生事件的节点，可能与currentTarget不同
- **currentTarget**:正在处理事件的节点，如果在capturing阶段和冒泡阶段处理事件，这个属性就与target属性不同。在事件监听函数中应该用这个属性而不是this
- **stopPropagation**():可以阻止事件从当前正在处理他的节点传播
- **preventDefault**():阻止浏览器执行与事件相关的默认动作，与0级DOM中返回false一样
- **clientX**, **clientY**:鼠标相对于浏览器的x坐标y坐标
- **screenX**, **screenY**:鼠标相对于显示器左上角的x坐标y坐标


## DOM0和DOM2的区别
### DOM0注册的事件处理函数会被覆盖

```javascript
    <button id="btn" onclick="console.log(111)">按钮</button>
    <script>
        const btn = document.getElementById("btn")
        btn.onclick = () => {
            console.log(222)
        }
    </script>
```
  如上的代码中，使用两种DOM0的方法给同一元素绑定了`click事件`。
  事实上，当点击按钮时，只会打印`222`，因为后注册的函数覆盖了之前的处理函数。

### DOM2注册的事件处理函数不会被覆盖，会按顺序依次执行

```javascript
    <button id="btn" onclick="console.log(111)">按钮</button>
    <script>
        const btn = document.getElementById("btn")
        btn.onclick = () => {
            console.log(222)
        }
        btn.addEventListener("click",()=>{
            console.log(333)
        })
        btn.addEventListener("click",()=>{
            console.log(444)
        })
```
执行结果如下 

![在这里插入图片描述](https://img-blog.csdnimg.cn/94193855263f455c97902b9f7c241591.png)

可以看出，同一元素上，DOM2绑定的事件不会影响DOM0绑定的事件，并且DOM2事件不会被覆盖。

### DOM0使用局限大
DOM0注册的事件，HTML与JS代码紧密耦合，若要更换事件处理程序，则需要改动HTML和JS两处的代码。

DOM0同一事件只能触发一种事件处理程序（因为会被覆盖），DOM2可以添加多个事件处理程序。

### 删除事件处理程序的方法不同

若要删除事件处理程序，

- DOM0只需要`btn.onclick = null`
- DOM2需要使用`removeEventListener`方法

## DOM3
DOM3事件在DOM2事件的基础上添加了更多的事件类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/40b3c3e19c3941aebaf7a7745bf88cd7.png)
同时DOM3事件也允许开发人员自定义事件