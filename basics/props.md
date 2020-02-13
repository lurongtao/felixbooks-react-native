# Props
大多数组件在创建时都可以使用不同的参数进行自定义。这些创建参数称为props，是properties的缩写。
例如，一个基本的React Native 组件 Image。创建图像时，可以使用名为source的属性来控制它显示的图像。

```js
import React, { Component } from 'react'
import { Image } from 'react-native'

export default class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    }
    return (
      <Image source={pic} style={{width: 193, height: 110}}/>
    )
  }
}
```

注意 {pic} 周围的大括号-它们将变量pic嵌入到JSX中。您可以将任何JavaScript表达式放在JSX中的大括号中。
你自己的组件也可以使用 props。这允许你创建一个在应用程序中的许多不同位置使用的组件，每个组件的属性可以略有不同，获取值可以在渲染函数中引用This.props。下面是一个例子：

```js
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Text>Hello {this.props.name}!</Text>
      </View>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center', top: 50}}>
        <Greeting name='张三' />
        <Greeting name='李四' />
        <Greeting name='王五' />
      </View>
    );
  }
}
```

我们在Greeting组件中将name作为一个属性来定制，这样可以复用这一组件来制作各种不同的“问候语”。上面的例子把Greeting组件写在 JSX 语句中，用法和内置组件并无二致——这正是 React 体系的魅力所在——如果你想搭建一套自己的基础 UI 框架，那就放手做吧！

上面的例子出现了一个新的名为 `View` 的组件。`View` 常用作其他组件的容器，来帮助控制布局和样式。

仅仅使用 `props` 和基础的`Text`、`Image` 以及 `View`组件，你就已经足以编写各式各样的 UI 组件了。要学习如何动态修改你的界面，那就需要进一步学习 State（状态）的概念。