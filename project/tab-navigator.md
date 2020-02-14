# 引入 react-native-tab-navigator

在项目环境命令行里安装 tabbar 导航器，详细内容可参见 [react-native-tab-navigator](https://github.com/ptomasroos/react-native-tab-navigator) 官网

```
yarn add react-native-tab-navigator -S
```

修改 index.tsx, 引入 tab-navigator 代码：

```ts
import React, { Component } from 'react'
import { TabNavigator } from 'react-native-tab-navigator'

import {
  View,
  Text
} from 'react-native'

import { Img } from './styledIndex.js'
import styles from './styleIndex.js'

import cookbook from '../../assets/images/cookbook.png'
import cookbookActive from '../../assets/images/cookbook-active.png'
import menu from '../../assets/images/menu.png'
import menuActive from '../../assets/images/menu-active.png'
import location from '../../assets/images/location.png'
import locationActive from '../../assets/images/location-active.png'
import more from '../../assets/images/more.png'
import moreActive from '../../assets/images/more-active.png'

interface Props {
  
}

interface State {
  selectedTab: string
}

export default class Index extends Component<Props, State> {
  constructor(props) {
    super(props)
  }
  
  state: State = {
    selectedTab: 'home'
  }

  componentDidMount() {
    
  }

  render() {
    return (
      <View style={ styles.layout }>
        <TabNavigator style={styles.navigator}>
          <TabNavigator.Item
            selected={this.state.selectedTab === 'home'}
            title="美食平台"
            selectedTitleStyle={{color: '#000'}}
            renderIcon={() => <Img source={cookbook} />}
            renderSelectedIcon={() => <Img source={cookbookActive} />}
            onPress={() => this.setState({ selectedTab: 'home' })}>
            <View><Text>美食大全</Text></View>
          </TabNavigator.Item>
          <TabNavigator.Item
            selected={this.state.selectedTab === 'category'}
            title="热门分类"
            selectedTitleStyle={{color: '#000'}}
            renderIcon={() => <Img source={menu} />}
            renderSelectedIcon={() => <Img source={menuActive} />}
            onPress={() => this.setState({ selectedTab: 'category' })}>
            <View><Text>热门分类</Text></View>
          </TabNavigator.Item>

          <TabNavigator.Item
            selected={this.state.selectedTab === 'map'}
            title="地图"
            selectedTitleStyle={{color: '#000'}}
            renderIcon={() => <Img source={location} />}
            renderSelectedIcon={() => <Img source={locationActive} />}
            onPress={() => this.setState({ selectedTab: 'map' })}>
            <View><Text>美食地图</Text></View>
          </TabNavigator.Item>
          
          <TabNavigator.Item
            selected={this.state.selectedTab === 'profile'}
            title="更多"
            selectedTitleStyle={{color: '#000'}}
            renderIcon={() => <Img source={more} />}
            renderSelectedIcon={() => <Img source={moreActive} />}
            onPress={() => this.setState({ selectedTab: 'profile' })}>
            <View><Text>更多</Text></View>
          </TabNavigator.Item>
        </TabNavigator>
      </View>
    )
  }
}
```

问题：

* 可能遇到和expo版本不兼容，建议用 expo34

```
yarn add expo@34.0.1 -S
yarn add react-native-screens@1.0.0-alpha.22
```

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