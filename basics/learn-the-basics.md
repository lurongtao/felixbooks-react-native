# 基础知识
React Native 与 React类似，但它使用原生(native)组件而不是基于浏览器(web)组件作为构建块。因此，要了解 React Ntive 应用程序的基本结构，您需要了解一些基本的 React 概念，如JSX、组件、状态和属性。如果你已经了解 React，那么你仍然需要学习一些 React Native 特定的东西，比如 原生(Native) 组件。本教程面向所有人群，无论你是否有 React 经验。

## Hello World
编程界的老习惯，先来个 Hello World 尝尝鲜：

```jsx
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class HelloWorldApp extends Component {
  render() {
    return (
      <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
        <Text>Hello, world!</Text>
      </View>
    );
  }
}
```

如果你感到好奇，这不就是React程序吗？是的，可以直接在web模拟器中运行这段代码。也可以将其粘贴到App.js文件中，以便在本地计算机上创建真正的原生应用程序。

## 奇葩的语法

这里的一些内容看来可能不像 JavaScript。别慌。这就是未来。

首先，ES2015（也称为ES6）是对JavaScript的一系列改进，ECMAScript 现在是官方标准的一部分，但还没有得到所有浏览器的支持。React Native ships 支持 ES2015，因此你可以使用这些内容而不必担心兼容性。上述示例中的import、from、class 和 extends 都是ES2015的特性。如果你不熟悉ES2015，你也可以通过阅读本教程中的示例代码来了解它。

在这个代码示例中，另一个不寻常的事情是`<View><Text>Hello world！</Text></View>`。这是JSX——一种在JavaScript中嵌入XML的语法。许多框架使用一种专门的模板语言，允许您在标记语言中嵌入代码。在React中，没有使用模板。JSX允许您在代码中编写标记语言。它看起来像web上的HTML，但这里使用的是React组件，而不是像`<div>` 或 `<span>`这样的 HTML 标签。在本例中，<Text>是一个内置组件，它显示一些文本，类似于`<div>`或`<span>`。

## 组件
这段代码定义了HelloWorldApp，这是一个新组件。当你在构建一个 React 本地应用程序时，你将大量地生成新组件。你在屏幕上看到的任何东西都是某种组件。