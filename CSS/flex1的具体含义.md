flex属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为0 1 auto。后两个属性可选。

## flex-grow
`flex-grow` 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

## flex-shrink
`flex-shrink` 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

## flex-basis
 `flex-basis`给上面两个属性分配多余空间之前, 计算项目是否有多余空间, 默认值为 auto, 即项目本身的大小

## flex:1
`flex:1` 的完整写法相当于

```css
flex-grow: 1;
flex-shrink: 1;
flex-basis: 0%
```