## 两栏布局 
### float + margin

```html
    <div class="box">
        <div class="left">左</div>
        <div class="right">右</div>
    </div>
```

```css
        .box {
            overflow: hidden;
        }

        .left {
            background: gray;
            float: left;
            height: 200px;
            width: 200px;
        }

        .right {
            background: lightgray;
            margin-left: 210px;
            height: 100px;
        }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6a1a6da306e84aee9ef3b20a6173bfcd.png)

## 三栏布局
### 1.absolute + 左右margin

```html
    <div class="box">
        <div class="left">左</div>
        <div class="middle">中间</div>
        <div class="right">右</div>
    </div>
```

```css
        .box {
            position: relative
        }

        .left {
            position: absolute;
            background: gray;
            left: 0;
            top: 0;
            height: 200px;
            width: 200px;
        }

        .middle {
            background: lightgray;
            margin-left: 210px;
            margin-right: 210px;
            height: 200px;
        }

        .right {
            position: absolute;
            background: gray;
            width: 200px;
            height: 200px;
            right:0;
            top:0;
        }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7fd3878181b44bddaf562ad9a73496e1.png)
### 2.float + margin


```html
<div class="box">
    <div class="left">左</div>
    <div class="right">右</div>
    <div class="middle">中间</div>
</div>
```
**注意div的顺序**

```javascript
    .box {
        overflow: hidden;
    }

    .left {
        float: left;
        background: gray;
        height: 200px;
        width: 200px;
    }

    .right {
        float: right;
        background: gray;
        width: 200px;
        height: 200px;
    }

    .middle {
        background: lightgray;
        margin-left: 210px;
        margin-right: 210px;
        height: 200px;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ffd47da411e4b6da8e6635f896ecd7d.png)

**这里由于float布局的特性，必须把.middle元素放在最后而不是中间，从代码层面上来讲html内容结构是不正确的**


## 什么是圣杯布局和双飞翼布局

圣杯布局和双飞翼布局也是**两边定宽，中间自适应的三栏布局**。并且中间栏要放在父元素的第一儿子位置，以优先渲染。因此，有些人会说这带来了代码层面上 html 内容结构的不正确。然而，这么做的好处就是可以让更为重要的**中间部分优先渲染**，从这个角度来讲，这或许是更加好的结构。

## 圣杯布局

### 实现思路
- 左中右三个元素分别左浮动。
- 中间元素占据第一位置优先渲染，设置该元素 width 为 100%
- 左元素设置左边距为-100%，以使得左元素上升一行并且处于最左位置，右元素设置左边距为自身宽度的负值，使得右元素上升一行处于最右位置。
- 设置父元素的左右 padding 为左右两个元素留出空间，以展示中间元素内容。
- 设置左右元素为相对定位，左元素的 left 和右元素的 right 为内边距的宽度的负值。

### 代码
```css
<div class="box">
    <div class="middle">中间</div>
    <div class="left">左</div>
    <div class="right">右</div>
</div>
```

```javascript
.box {
        padding: 0 200px;
    }

    .middle {
        background: gray;
        height: 200px;
        width: 100%;
        float: left;
    }

    .left {
        background: lightgray;
        width: 200px;
        height: 200px;
        float: left;
        margin-left: -100%;
        position: relative;
        left: -200px;
    }

    .right {
        background: lightgray;
        width: 200px;
        height: 200px;
        float: left;
        margin-left: -200px;
        position: relative;
        right: -200px;
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4257e266298c4a94a95bfa5f0e890402.png)

## 双飞翼布局

### 实现思路
- 左中右三个元素分别左浮动。
- 中间元素占据第一位置优先渲染，设置该元素 width 为 100%
- 左元素设置左边距为-100%，以使得左元素上升一行并且处于最左位置，右元素设置左边距为自身宽度的负值，使得右元素上升一行处于最右位置。
- 设置中间元素的子元素左右边距为左右元素留空位，以展示中间元素内容。

### 代码

```html
<div class="box">
    <div class="middle">
        <div class="child">中间</div>
    </div>
    <div class="left">左</div>
    <div class="right">右</div>
</div>
```

```css
    .box {
        overflow: hidden;
    }

    .middle {
        float: left;
        width: 100%;
    }

    .child {
        margin: 0 200px;
        height: 200px;
        background: gray;
    }


    .left {
        background: lightgray;
        width: 200px;
        height: 200px;
        float: left;
        margin-left: -100%;
    }

    .right {
        background: lightgray;
        width: 200px;
        height: 200px;
        float: left;
        margin-left: -200px;
    }
```

## 圣杯布局与双飞翼布局的区别
可以看出，圣杯布局与双飞翼布局的整体实现思路是一样的，只是最后一步的处理方式存在差别。

- 圣杯布局使用父元素的左右内边距来为左右元素腾出空间
- 双飞翼布局使用中间元素的子元素左右外边距来为左右元素腾出空间