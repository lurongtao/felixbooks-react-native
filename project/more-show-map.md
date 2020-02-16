# 构建 More.tsx 页面

更多页面实现了两个功能：

1、是否显示地图页签 

2、[拍照功能](./more-take-picture.md)

## 是否显示地图页签

#### 更新 store

添加了 isShow 属性，和 setVisible 方法。

```js
// store/index.js

import {
  observable,
  action,
  computed
} from 'mobx'

class Store {
  // swiper 与 top10 共享的数据
  @observable
  list = []

  // 定义是否显示地图按钮
  @observable
  isShow = true

  // swiper 数据过滤
  @computed
  get swiper() {
    return this.list.slice(0, 5).map((value, index) => {
      return {
        img: value.img
      }
    })
  }

  // top10 数据过滤
  @computed
  get top10() {
    return this.list.slice(0, 10).map((value, index) => {
      return {
        img: value.img,
        all_click: value.all_click,
        favorites: value.favorites,
        name: value.name
      }
    })
  }

  // 装载 list 数据
  @action.bound
  setList(data) {
    this.list = data
  }

  // 修改是否显示地图按钮
  @action.bound
  setVisible(status) {
    this.isShow = status
  }
}

export default new Store()
```

#### 添加 More.tsx 文件

在根目录pages下创建 more 文件夹，再创建 More.tsx 文件，内容如下

```ts
// pages/more/More.tsx

import React, { Component } from 'react'
import { View, Text, Switch, AsyncStorage } from 'react-native'
import { observer, inject } from 'mobx-react'

interface Props {
  store?: any
}

interface State {

}

@inject('store')
@observer
export default class Profile extends Component<Props, State> {
  state = {

  }

  async componentDidMount() {
    
  }

  render() {
    return (
      <View>
        <View style={{
          flexDirection: 'row', 
          justifyContent: 'flex-start', 
          alignItems: 'flex-start', 
          padding: 20
        }}>
          <View style={{
            height: 30, 
            justifyContent: 'center', 
            alignItems: 'center'
          }}>
            <Text>是否显示地图：</Text>
          </View>
          <Switch
            value={this.props.store.isShow}
            onValueChange={(value) => {
              this.props.store.setVisible(value)
              AsyncStorage.setItem('isShow', value.toString())
            }}
          ></Switch>
        </View>
      </View>
    )
  }
}
```

#### 修改 pages/index/Index.tsx 文件

##### 1、修改代码的要点

```js
  // 定义 store
  interface Props {
    navigation?: any
    store?: any
  }

  // 记录用户缓存
  async componentDidMount() {
    let isShow = await AsyncStorage.getItem('isShow')
    this.props.store.setVisible(JSON.parse(isShow))
  }

  // 在 tabbar 里修改
  {
    this.props.store.isShow
      ? (
        <TabNavigator.Item
          selected={this.state.selectedTab === 'map'}
          title="地图"
          titleStyle={styles.titleStyle}
          selectedTitleStyle={styles.selectedTitleStyle}
          renderIcon={() => <Img source={map} />}
          renderSelectedIcon={() => <Img source={mapActive} />}
          onPress={() => {
            this.setState({ selectedTab: 'map' })
            this.props.navigation.setOptions({ title: '地图' })
          }}
        >
          <Map></Map>
        </TabNavigator.Item>
      )
      : null
  }
```

##### 2. 全部代码

```ts
// pages/index/Index.tsx

import React, { Component, ContextType } from 'react'
import TabNavigator from 'react-native-tab-navigator'
import * as Device from 'expo-device'
import { observer, inject } from 'mobx-react'

import { Provider } from '../../context/navigation'

import {
  View,
  Text,
  AsyncStorage
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

import Home from '../home/Home'
import List from '../list/List'
import Map from '../map/Map'
import More from '../more/More'

interface Props {
  navigation?: any
  store?: any
}

interface State {
  selectedTab: string
}

@inject('store')
@observer
class Index extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
  }

  state: State = {
    selectedTab: 'home'
  }

  async componentDidMount() {
    let isShow = await AsyncStorage.getItem('isShow')
    this.props.store.setVisible(JSON.parse(isShow))
  }

  render() {
    return (
      <>
        <TabNavigator
          tabBarStyle={Device.deviceName === 'iPhone Xʀ' ? styles.tabBarStyle : null}
        >
          <TabNavigator.Item
            selected={this.state.selectedTab === 'home'}
            title="美食大全"
            titleStyle={styles.titleStyle}
            selectedTitleStyle={styles.selectedTitleStyle}
            renderIcon={() => <Img source={cookbook} />}
            renderSelectedIcon={() => <Img source={cookbookActive} />}
            onPress={() => {
              this.setState({ selectedTab: 'home' })
              this.props.navigation.setOptions({ title: '美食大全' })
            }}
          >
            <Provider value={{...this.props}}>
              <Home></Home>
            </Provider>
          </TabNavigator.Item>
          <TabNavigator.Item
            selected={this.state.selectedTab === 'category'}
            title="热门"
            titleStyle={styles.titleStyle}
            selectedTitleStyle={styles.selectedTitleStyle}
            renderIcon={() => <Img source={category} />}
            renderSelectedIcon={() => <Img source={categoryActive} />}
            onPress={
              () => {
                this.setState({ selectedTab: 'category' })
                this.props.navigation.setOptions({ title: '热门' })
              }
            }
          >
            <Provider value={{...this.props}}>
              <List></List>
            </Provider>
          </TabNavigator.Item>
          {
            this.props.store.isShow
              ? (
                <TabNavigator.Item
                  selected={this.state.selectedTab === 'map'}
                  title="地图"
                  titleStyle={styles.titleStyle}
                  selectedTitleStyle={styles.selectedTitleStyle}
                  renderIcon={() => <Img source={map} />}
                  renderSelectedIcon={() => <Img source={mapActive} />}
                  onPress={() => {
                    this.setState({ selectedTab: 'map' })
                    this.props.navigation.setOptions({ title: '地图' })
                  }}
                >
                  <Map></Map>
                </TabNavigator.Item>
              )
              : null
          }
          <TabNavigator.Item
            selected={this.state.selectedTab === 'more'}
            title="更多"
            titleStyle={styles.titleStyle}
            selectedTitleStyle={styles.selectedTitleStyle}
            renderIcon={() => <Img source={more} />}
            renderSelectedIcon={() => <Img source={moreActive} />}
            onPress={() => {
              this.setState({ selectedTab: 'more' })
              this.props.navigation.setOptions({ title: '更多' })
            }}
          >
            <More></More>
          </TabNavigator.Item>
        </TabNavigator>
      </>
    )
  }
}

export default Index
```

