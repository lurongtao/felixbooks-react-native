# 构建 Map.tsx 美食地图页面

“美食地图” 主要应用的知识点是 WebView, 根据官网推荐，使用 [react-native-webview](https://github.com/react-native-community/react-native-webview) 项目来实现在RN里嵌入Web页面。

## 模块安装

```
npm install --save react-native-webview

react-native link react-native-webview
```

## Map.tsx 文件构建

在项目根目录 pages 下创建目录 map, 在 map 目录下创建 Map.tsx 文件，文件内容如下：

```ts
import React, { Component } from 'react'
import { View } from 'react-native'
import { WebView } from 'react-native-webview'

interface Props {
  
}
interface State {
  
}

export default class Map extends Component<Props, State> {
  state = {}

  render() {
    return (
      <View
        style={{
          width: '100%',
          flex: 1
        }}
      >
        <WebView
          source={{ uri: 'https://map.baidu.com/search/%E7%BE%8E%E9%A3%9F/@12959238.56,4825347.47,12z?querytype=s&da_src=shareurl&wd=%E7%BE%8E%E9%A3%9F&c=131&src=0&pn=0&sug=0&l=12&b=(12905478.56,4795011.47;13012998.56,4855683.47)&from=webmap&biz_forward=%7B%22scaler%22:2,%22styles%22:%22pl%22%7D&device_ratio=2' }}
          style={{ 
            width: '100%',
            height: '100%'
          }}
        />
      </View>
    )
  }
}
```