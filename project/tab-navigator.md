# 引入 react-native-tab-navigator

在项目环境命令行里安装 tabbar 导航器，详细内容可参见 [react-native-tab-navigator](https://github.com/ptomasroos/react-native-tab-navigator) 官网

```
yarn add react-native-tab-navigator -S
```

## 修改 index.tsx, 引入 tab-navigator 代码：

```ts
import React, { Component } from 'react'
import TabNavigator from 'react-native-tab-navigator'

import {
  View,
  Text
} from 'react-native'

import {
  Img
} from './styled_index'
import styles from './style_index'

import cookbook from '../../assets/images/cookbook.png'
import cookbookActive from '../../assets/images/cookbook-active.png'
import category from '../../assets/images/menu.png'
import categoryActive from '../../assets/images/menu-active.png'
import map from '../../assets/images/location.png'
import mapActive from '../../assets/images/location-active.png'
import more from '../../assets/images/more.png'
import moreActive from '../../assets/images/more-active.png'

interface Props {

}

interface State {
  selectedTab: string
}

class Index extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
  }

  state: State = {
    selectedTab: 'home'
  }

  componentDidMount() {

  }

  render() {
    return (
      <TabNavigator
        tabBarStyle={styles.tabBarStyle}
      >
        <TabNavigator.Item
          selected={this.state.selectedTab === 'home'}
          title="美食大全"
          titleStyle={styles.titleStyle}
          selectedTitleStyle={styles.selectedTitleStyle}
          renderIcon={() => <Img source={cookbook} />}
          renderSelectedIcon={() => <Img source={cookbookActive} />}
          onPress={() => this.setState({ selectedTab: 'home' })}
        >
          {<View><Text>美食大全</Text></View>}
        </TabNavigator.Item>
        <TabNavigator.Item
          selected={this.state.selectedTab === 'category'}
          title="分类"
          titleStyle={styles.titleStyle}
          selectedTitleStyle={styles.selectedTitleStyle}
          renderIcon={() => <Img source={category} />}
          renderSelectedIcon={() => <Img source={categoryActive} />}
          onPress={() => this.setState({ selectedTab: 'category' })}
        >
          {<View><Text>分类</Text></View>}
        </TabNavigator.Item>
        <TabNavigator.Item
          selected={this.state.selectedTab === 'map'}
          title="地图"
          titleStyle={styles.titleStyle}
          selectedTitleStyle={styles.selectedTitleStyle}
          renderIcon={() => <Img source={map} />}
          renderSelectedIcon={() => <Img source={mapActive} />}
          onPress={() => this.setState({ selectedTab: 'map' })}
        >
          {<View><Text>地图</Text></View>}
        </TabNavigator.Item>
        <TabNavigator.Item
          selected={this.state.selectedTab === 'more'}
          title="更多"
          titleStyle={styles.titleStyle}
          selectedTitleStyle={styles.selectedTitleStyle}
          renderIcon={() => <Img source={more} />}
          renderSelectedIcon={() => <Img source={moreActive} />}
          onPress={() => this.setState({ selectedTab: 'more' })}
        >
          {<View><Text>更多</Text></View>}
        </TabNavigator.Item>
      </TabNavigator>
    )
  }
}

export default Index
```

问题：
* ts 提示引入的 png 不能识别，飘红了。解决方案是在项目跟目录下创建 `images.d.ts` 文件，内容如下：

```js
declare module '*.svg'
declare module '*.png'
declare module '*.jpg'
declare module '*.jpeg'
declare module '*.gif'
declare module '*.bmp'
declare module '*.tiff'
```

## 在 `pages/index` 下创建样式文件：

* 安装 styled-components 模块，创建 styled_index.js, 内容如下：

```js
import styled from 'styled-components'

const Img = styled.Image `
  width: 25px;
  height: 25px;
`

export {
  Img
}
```

## 再创建一个样式文件 style_index.js, 内容如下：

```js
import { StyleSheet } from 'react-native'

export default StyleSheet.create({
  titleStyle: {
    color: '#666'
  },
  
  tabBarStyle: {
    paddingBottom: 34, 
    height: 80
  },

  selectedTitleStyle: {
    color: '#000'
  }
})
```

## tabbar 的兼容处理

安装 expo-device

```
npm i expo-device -S
```

修改 index.ts, 根据您设备情况引入不同的样式，此处只是测试性代码，只做了iphone XR 和 其他非 “齐刘海” iPhone 手机：

```js
// 载入模块
import * as Device from 'expo-device'

// 在 TabNavigator 上修改 tabBarStyle
<TabNavigator
  tabBarStyle={
    Device.deviceName === 'iPhone Xʀ' ? styles.tabBarStyle : null
  }
>
```