# 使用 flexbox 布局

我们在 React Native 中使用 flexbox 规则来指定某个组件的子元素的布局。Flexbox 可以在不同屏幕尺寸上提供一致的布局结构。

一般来说，使用 `flexDirection`、`alignItems` 和 `justifyContent` 三个样式属性就已经能满足大多数布局需求。

> React Native 中的 Flexbox 的工作原理和 web 上的 CSS 基本一致，当然也存在少许差异。首先是默认值不同：flexDirection的默认值是column而不是row，而flex也只能指定一个数字值。

## Flex

flex 属性决定元素在主轴上如何填满可用区域。整个区域会根据每个元素设置的flex属性值被分割成多个部分。

在下面的例子中，在设置了 `flex: 1` 的容器view中，有红色，黄色和绿色三个子 `view`。红色 `view` 设置了 `flex: 1`，黄色 `view` 设置了 `flex: 2`，绿色 `view` 设置了 `flex: 3`。`1+2+3 = 6`，这意味着红色 `view` 占据整个区域的 `1/6`，黄色 `view` 占据整个区域的 `2/6`，绿色 `view` 占据整个区域的`3/6`。

## Flex Direction

在组件的 style 中指定 `flexDirection` 可以决定布局的主轴。子元素是应该沿着水平轴 (row) 方向排列，还是沿着竖直轴 (column) 方向排列呢？默认值是竖直轴 (column) 方向。

```js
import React, { Component } from 'react'
import { View } from 'react-native'

export default class FlexDirectionBasics extends Component {
  render() {
    return (
      // 尝试把`flexDirection`改为`column`看看
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    )
  }
}
```

## Layout Direction

布局方向指定层次结构中的子项和文本的布局方向。布局方向也会影响边起点和终点所指的对象。默认情况下，React Native布局使用LTR布局方向。在这种模式下，开始是指左边，结束是指右边。

* LTR（默认值）文本和子级，并从左到右排列。应用的边距和填充元素的开头应用于左侧。

* 从右到左排列的RTL文本和子项。应用的边距和填充元素的开头应用于右侧。

## Justify Content
在组件的 style 中指定 justifyContent 可以决定其子元素沿着主轴的排列方式。子元素是应该靠近主轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：flex-start、center、flex-end、space-around、space-between 以及 space-evenly。

```js
import React, { Component } from 'react'
import { View } from 'react-native'

export default class JustifyContentBasics extends Component {
  render() {
    return (
      // 尝试把`justifyContent`改为`center`看看
      // 尝试把`flexDirection`改为`row`看看
      <View style={{
        flex: 1,
        flexDirection: 'column',
        justifyContent: 'space-between',
      }}>
        <View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'skyblue'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'steelblue'}} />
      </View>
    )
  }
}
```