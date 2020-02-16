# 配置导航

本项目应用 React Navigation 构建路由系统。有关 React Navigation 请参阅 <千锋大前端小册> 系列之 React Navigation 部分》。

## 安装 React Navigation 环境

```
npm install @react-navigation/native

npm install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view

npm install @react-navigation/stack
```

## 给App.tsx配置路由

```ts
import React from 'react'
import { NavigationContainer } from '@react-navigation/native'
import { createStackNavigator } from '@react-navigation/stack'

import { Provider } from 'mobx-react'
import store from './store/'

import Index from './pages/index/Index'
import List from './pages/list/List'
import Detail from './pages/detail/Detail'

const Stack = createStackNavigator()

export default function App() {

  // 这里配置了三个页面
  return (
    <NavigationContainer>
      <Provider store={store}>
        <Stack.Navigator
          screenOptions={{ 
            headerStyle: {
              backgroundColor: '#ee7530'
            },
            headerTintColor: '#fff',
            headerTitleStyle: {
              fontWeight: 'bold',
              fontSize: 20
            }
          }}
        >
          <Stack.Screen 
            name="Index" 
            component={Index}
            options={{ 
              title: '首页'
            }}
          />
          <Stack.Screen
            name="List"
            component={List}
            options={{
              title: '热门'
            }}
          />
          <Stack.Screen
            name="Detail"
            component={Detail}
            options={{
              title: '详情'
            }}
          />
        </Stack.Navigator>
      </Provider>
    </NavigationContainer>
  )
}
```

## 创建 Context

为了让组件能收到路由的信息，这里我们自己构建了一个 Context。

在根目录下创建一个context目录，在此目录下创建一个 `navigation.js` 文件，内容如下：

```js
// context/navigations.js

import { createContext } from 'react'

const navigationContext = createContext()

let { Provider, Consumer } = navigationContext

export {
  navigationContext,
  Provider,
  Consumer
}
```

## 修改 index.tsx

##### 1.解构出Provider

```js
import { Provider } from '../../context/navigation'
```

##### 2.通过Context 的Provider，将props递交给后代组件

```js
<Provider value={{...this.props}}>
  <Home></Home>
</Provider>

<Provider value={{...this.props}}>
  <List></List>
</Provider>
```

##### 3.全部内容

```ts
// pages/index/Index.tsx

import React, { Component, ContextType } from 'react'
import TabNavigator from 'react-native-tab-navigator'
import * as Device from 'expo-device'

// 解构出 Provider
import { Provider } from '../../context/navigation'

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

import Home from '../home/Home'
import List from '../list/List'
import Detail from '../detail/Detail'

interface Props {
  navigation?: any
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
            {/* 通过Context 的Provider，将props递交给后代组件 */}
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
            {/* 通过Context 的Provider，将props递交给后代组件 */}
            <Provider value={{...this.props}}>
              <List></List>
            </Provider>
          </TabNavigator.Item>
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
            {<View><Text>地图</Text></View>}
          </TabNavigator.Item>
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
            {<View><Text>更多</Text></View>}
          </TabNavigator.Item>
        </TabNavigator>
      </>
    )
  }
}

export default Index
```

## 修改 home.tsx

##### 1. 将路由信息传给HotCate

```js
<HotCate { ...this.props }></HotCate>
```

##### 2.定义Props

```js
interface Props {
  navigation?: any
}
```

##### 3.全部内容

```ts
// pages/home/Home.tsx

import React, { Component } from 'react'
import { ScrollView, StatusBar } from 'react-native'

import Swiper from './Swiper'
import HotCate from './HotCate'
import Top10 from './Top10'

interface Props {
  navigation?: any
}

interface State {
  
}

class Home extends Component<Props, State> {
  render() {
    return (
      <ScrollView>
        <StatusBar backgroundColor="blue" barStyle="light-content" />
        <Swiper></Swiper>
        {/* 将路由信息传给HotCate */}
        <HotCate { ...this.props }></HotCate>
        <Top10></Top10>
      </ScrollView>
    )
  }
}

export default Home
```

## 修改 HotCate.tsx

##### 1. 导入 

```js
import { Consumer } from '../../context/navigation'
```

##### 2. 路由到“热门”页面

```js
_onPress = (navigation) => {
  return () => {
    navigation.push('List')
  }
}

<View style={styles.container}>
  <Consumer>
    {
      ({navigation}) => {
        return (
          <Grid
            data={this.state.hotCate}
            renderItem={this._renderItem}
            hasLine={false}
            onPress={this._onPress(navigation)}
          ></Grid>
        )
      }
    }
  </Consumer>
</View>
```

###### 3. 全部代码

```ts
// pages/home/HotCate.tsx

import React, { Component } from 'react'
import { Grid } from '@ant-design/react-native'
import { get } from '../../utils/http'
import { Consumer } from '../../context/navigation'

import styles from './style_home'

import {
  View,
  Text,
  Image
} from 'react-native'

interface Props {
  
}
interface State {
  hotCate: Array<object>
}

export default class HotCate extends Component<Props, State> {
  state = {
    hotCate: []
  }

  _renderItem(el, index) {
    return (
      <View
        style={styles.gridContainer}
      >
        {el.img ? <Image source={{uri: el.img}} style={styles.gridImg}></Image> : null}
        <Text style={styles.gridText}>{el.title}</Text>
      </View>
    )
  }

  _onPress = (navigation) => {
    return () => {
      navigation.push('List')
    }
  }
  
  async componentDidMount() {
    let hotCate = await get('http://localhost:9000/api/hotcate')

    // 补全最后一项数据
    hotCate.push({
      img: '',
      title: '更多...'
    })

    this.setState({
      hotCate
    })
  }

  render() {
    return (
      <View style={styles.container}>
        <Consumer>
          {
            ({navigation}) => {
              return (
                <Grid
                  data={this.state.hotCate}
                  renderItem={this._renderItem}
                  hasLine={false}
                  onPress={this._onPress(navigation)}
                ></Grid>
              )
            }
          }
        </Consumer>
      </View>
    )
  }
}
```

## 修改 Top10.tsx

##### 1. 通过 contextType 定义 context

```ts
import { navigationContext } from '../../context/navigation'
```

##### 2. 导航到详情页，并传参

```ts
import { navigationContext } from '../../context/navigation'

static contextType = navigationContext

_onPress = (e) => {
  this.context.navigation.push('Detail', { name: e.name })
}

<Grid
  data={this.props.store.top10}
  columnNum={2}
  hasLine={false}
  renderItem={this._renderTop10}
  onPress={this._onPress}
/>
```

##### 3. 全部代码

```ts
// pages/home/Top10.tsx

import React, { Component } from 'react'
import { Grid } from '@ant-design/react-native'
import { observer, inject } from 'mobx-react'
import { navigationContext } from '../../context/navigation'

import {
  View,
  Text,
  Image
} from 'react-native'

import styles from './style_home.js'

interface Props {
  // store 作为组件的 props
  store?: any
}

interface State {
  
}

// 注入 store 与 将类变为可观察的对象
@inject('store')
@observer
class Top10 extends Component<Props, State> {

  static contextType = navigationContext

  _renderTop10(el, index) {
    return (
      <View style={styles.top10ItemContainer}>
        <View style={styles.top10ImgContainer}>
          <Image style={styles.Top10Img} source={{uri: el.img}}></Image>
        </View>
        <View style={styles.top10DesContainter}>
          <Text style={styles.top10Titie}>{el.name}</Text>
          <Text style={styles.Top10Desc}>{el.all_click} {el.favorites}</Text>
        </View>
      </View>
    )
  }

  _onPress = (e) => {
    this.context.navigation.push('Detail', { name: e.name })
  }

  render() {   
    return (
      <View style={styles.top10Container}>
        <View style={styles.top10Head}>
          <Text style={styles.top10HeadText}>精品好菜</Text>
        </View>
        <View style={styles.gridContainer}>
          <Grid
            data={this.props.store.top10}
            columnNum={2}
            hasLine={false}
            renderItem={this._renderTop10}
            onPress={this._onPress}
          />
        </View>
      </View>
    )
  }
}

export default Top10
```

## 修改 List.tsx

##### 1. 载入路由相关模块，实现路由到详情页的功能，主要代码：

```js
// 1. 载入Context
import { navigationContext } from '../../context/navigation'

// 2. 在 Props 里定义 navigation
interface Props {
  store?: any,
  navigation?: any
}

// 3. 在类里定义 contextType 静态变量
static contextType = navigationContext

// 4. 在组件类里定义路由跳转响应方法
_onPress = (name: string) => {
  return () => {
    // 鉴于此页面从 TabBar 和 首页两个入口进入
    // 路由跳转的方式也不同
    if (this.context) {
      // 从Tabbar进入
      this.context.navigation.push('Detail', {name})
    } else {
      // 从首页进入
      this.props.navigation.push('Detail', {name})
    }
  }
}

// 5. 应用 TouchableOpacity 组件绑定路由跳转事件
<TouchableOpacity
  onPress={this._onPress(name)}
>
  <View style={styles.listWrap}>
    <View style={styles.imgWrap}>
      <Image style={styles.image} source={{uri: img}}></Image>
    </View>
    <View style={styles.descWrap}>
      <Text style={styles.title}>{name}</Text>
      <Text style={styles.subtitle} numberOfLines={1}>{burdens}</Text>
      <Text style={styles.desc}>{all_click} {favorites}</Text>
    </View>
  </View>
</TouchableOpacity>
```

##### 2. 全部代码

```js
import React, { Component, createRef } from 'react'
import { navigationContext } from '../../context/navigation'

import {
  inject,
  observer
} from 'mobx-react'

import {
  View,
  Text,
  Image,
  FlatList,
  TouchableOpacity
} from 'react-native'

import styles from './style_list'

interface Props {
  store?: any,
  navigation?: any
}

interface State {
  // 记录上拉加载更多的当前页码
  curPage: number, 

  // 页面显示的数据
  datalist: Array<object>, 

  // 控制下拉刷新的开关
  refresh: boolean 
}

let pageSize = 10

@inject('store')
@observer
export default class List extends Component<Props, State> {
  constructor (
    public props: Props, 
    public flatlist,
  ) {
    super(props)
    this.flatlist = createRef()
  }

  state = {
    curPage: 1,
    datalist: [],
    refresh: false
  }

  static contextType = navigationContext

  _onPress = (name: string) => {
    return () => {
      if (this.context) {
        this.context.navigation.push('Detail', {name})
      } else {
        this.props.navigation.push('Detail', {name})
      }
    }
  }
  
  // 渲染 Flatlist 组件数据
  _renderItem(item) {
    let {img, name, burdens, all_click, favorites} = item.item.data   
    return (
      <TouchableOpacity
        onPress={this._onPress(name)}
      >
        <View style={styles.listWrap}>
          <View style={styles.imgWrap}>
            <Image style={styles.image} source={{uri: img}}></Image>
          </View>
          <View style={styles.descWrap}>
            <Text style={styles.title}>{name}</Text>
            <Text style={styles.subtitle} numberOfLines={1}>{burdens}</Text>
            <Text style={styles.desc}>{all_click} {favorites}</Text>
          </View>
        </View>
      </TouchableOpacity>
    )
  }

  // 处理用户拉到底端的响应函数
  _handleReachEnd() {
    // 如果还有数据，一直加载
    // 加载更多，由于Mock数据问题，有ID重复问题
    // if (this.state.curPage < Math.ceil(this.props.store.list.length / pageSize)) {
    //   console.log(this.state.curPage)
    //   this.setState((state) => {
    //     return {
    //       curPage: state.curPage + 1
    //     }
    //   }, () => {
    //     this._loadData()
    //   })
    // }
  }

  // 下拉刷新的响应函数
  _handleRefresh() {
    this.setState({
      refresh: true
    })

    // 此处可以异步获取后端接口数据，具体实现思路见上拉加载。
    setTimeout(() => {
      this.setState({
        refresh: false
      })
    }, 2000)
  }

  // 加载数据
  _loadData() {
    let data = this.props.store.list.slice(0, this.state.curPage * pageSize)    
    let flatListData = data.map((value, index) => ({
        data: value,
        key: value.id
      })
    )
    this.setState({
      datalist: flatListData
    })
  }

  // 执行第一次数据加载
  componentDidMount() {
    setTimeout((params) => {
      this._loadData()
    }, 0)
  }

  render() {
    return (
      <View style={styles.container}>
        <FlatList
          ref={this.flatlist}
          renderItem={this._renderItem.bind(this)}
          data={this.state.datalist}
          refreshing={this.state.refresh}
          onEndReached={this._handleReachEnd.bind(this)}
          onEndReachedThreshold={1}
          onRefresh={this._handleRefresh.bind(this)}
        ></FlatList>
      </View>
    )
  }
}
```