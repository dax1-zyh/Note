**注意点**
- 可拖拽元素为绝对定位
- 分别监听`mousedown`，`mousemove`，`mouseup`三个事件
- `mousemove`事件要判断`dragging`的值


```javascript
<div id="box"></div>
<style>
    #box {
        position: absolute;	// 重要
        border: 1px solid red;
        height: 100px;
        width: 100px;
    }
</style>
<script>
    let dragging = false
    let position = null
    box.addEventListener('mousedown', function (e) {
        dragging = true // 正在移动
        position = [e.clientX, e.clientY]
    })
    document.addEventListener('mousemove', function (e) {
        if (dragging === false) {
            return
        }
        const x = e.clientX
        const y = e.clientY
        const deltaX = x - position[0]
        const deltaY = y - position[1]
        const left = parseInt(box.style.left || 0)
        const top = parseInt(box.style.top || 0)
        box.style.left = left + deltaX + 'px'
        box.style.top = top + deltaY + 'px'
        position = [x, y]
    })
    document.addEventListener('mouseup', function (e) {
        dragging = false
    })
</script>
```