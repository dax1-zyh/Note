## 水平居中
### 行内元素
行内元素想要水平居中，只需要给父元素添加`text-align:center`即可。

```html
<div class='container'>
  <span>example</span>
</div>
```

```css
.container {
  text-align: center;
}

span {
  border:1px solid red;
}
```

### 块级元素
#### 定宽块级元素
给需要居中的块级元素加`margin:0 auto`，但是块级元素一定要有`width`
```css
  .center{
      width:200px;
      margin:0 auto;
      border:1px solid red;
  }
  <div class="center">水平居中</div>
```

### 不定宽块级元素
#### 方法1：转换为table

先给要居中的元素设置`display:table`，然后设置`margin:0 auto`
```css
  .center{
      display:table;
      margin:0 auto;
      border:1px solid red;
  }
  <div class="center">水平居中</div>
```

#### 方法2：设置inline-block（多个块级元素）

如果多个块级元素要水平居中，则给子元素设置`display:inline-block`，再给父元素设置`text-align:center`

```css
  .center{
      text-align:center;
  }
  .inlineblock-div{
      display:inline-block;
  }
  <div class="center">
      <div class="inlineblock-div">1</div>
      <div class="inlineblock-div">2</div>
  </div>

```

#### 方法3：flex布局

```css
  .center{
      display:flex;
      justify-content:center;
  }
  <div class="center">
      <div class="flex-div">1</div>
      <div class="flex-div">2</div>
  </div>
```

#### 方法4：position + 负margin；
#### 方法5：position + margin：auto；
#### 方法6：position + transform；

注：这里方法4、5、6同下面垂直居中一样的道理，只不过需要把top/bottom改为left/right，在垂直居中部分会详细讲述。

## 垂直居中
### 单行文本垂直居中
- 设置`padding-top`和`padding-bottom`相等
- 设置`line-height=height`

### 块级元素垂直居中
#### 方法一：flex布局

```css
  <div class="container">
    <div>example</div>
  </div>
  
.container {
  border: 1px solid red;
  height: 100px;
  display: flex;
  align-items: center;
}
```

#### 方法二：绝对定位，top：0，负margin（需知自身宽高）
- 对要居中的元素设置`position:absolute`，父元素设置`position:relative`
- 居中元素设置`top:50%`，再设置`margin-top:`，值为自身`height`一半的负数

```css
  <div class="parent">
    <div class="child"></div>
  </div>

.parent {
  position: relative;
  border: 1px solid red;
  height: 300px;
}

.child {
  position: absolute;
  border: 1px solid blue;
  height: 100px;
  width: 100px;
  top:50%;
  margin-top: -50px;
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1557e0b1d5684ff1b7d025b6375ecc1c.png)
#### 方法三：绝对定位，top/bottom=0，margin:auto
-  对要居中的元素设置`position:absolute`，父元素设置`position:relative`
- 居中元素设置`top: 0, bottom:0, margin:auto`

```css
  <div class="parent">
    <div class="child"></div>
  </div>

.parent {
  position: relative;
  border: 1px solid red;
  height: 300px;
}

.child {
  position: absolute;
  border: 1px solid blue;
  height: 100px;
  width: 100px;
  top: 0;
  bottom: 0 ;
  margin: auto;
}
```

注：上述的块级垂直居中方法，稍加改动，即可成为块级水平居中方法，如top/bottom换成left/right

## 水平垂直居中
### flex布局

```css
.parent {
  border: 1px solid blue;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.child {
  border: 1px solid red;
  height: 100px;
  width: 100px;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e14221ddfcc44ec858f038b880c9678.png)

### 绝对定位+top/bottom/left/right:0+margin:auto

```css

.parent {
  position: relative;
  border: 1px solid blue;
  height: 300px;
}

.child {
  position: absolute;
  border: 1px solid red;
  height: 100px;
  width: 100px;
  top: 0;
  bottom: 0;
  left: 0;
  right:0 ;
  margin: auto;
}
```

### 绝对定位+left/top:50%+负margin（需知自身宽高）
```css
.parent {
  position: relative;
  border: 1px solid blue;
  height: 300px;
}

.child {
  position: absolute;
  border: 1px solid red;
  height: 100px;
  width: 100px;
  left: 50%;
  top: 50%;
  margin-left: -50px;
  margin-top: -50px;
}
```

### 绝对定位+transform

```css
.parent {
  position: relative;    
  border: 1px solid blue; 
  height: 300px;
}

.child {
  position:absolute;
  border: 1px solid red;
  height: 100px;
  width: 100px;
  top: 50%;
  left: 50%;
  transform:translate(-50%,-50%)
}
```

### 绝对定位+calc

```css
.parent {
  position: relative;
  border: 1px solid blue;
  height: 300px;
}

.child {
  position: absolute;
  border: 1px solid red;
  height: 100px;
  width: 100px;
  top: calc(50% - 50px);
  left: calc(50% - 50px);
}
```
### 父元素table-cell,vertical-align,text-align，子元素inline-block

```css
.parent {
  border: 1px solid blue;
  height: 300px;
  width: 300px;
  display: table-cell;
  vertical-align:middle;
  text-align: center;
}

.child {
  border: 1px solid red;
  height: 100px;
  width: 100px;
  display: inline-block;
}
```

### 父元素table-cell,vertical-align,子元素margin:0,auto

```css
.parent {
  border: 1px solid blue;
  height: 300px;
  width: 300px;
  display: table-cell;
  vertical-align:middle;
}

.child {
  border: 1px solid red;
  height: 100px;
  width: 100px;
  margin: 0 auto;
}
```