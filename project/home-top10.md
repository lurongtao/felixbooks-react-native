# 构建 Top10.tsc 组件

Top10组件渲染的数据和Swiper组件可以使用同一个接口的数据，因此我们决定应用Mobx来管理这个数据。

## 安装 Mobx 相关模块

```
npm i mobx mobx-react -S
```

## 构建 store

在项目根目录下创建 store 文件夹，在这个文件下创建 index.js 文件：

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
}

export default new Store()
```

## 开始构建 Top.tsx 组件

在 pages/home 下创建 Top.tsx 文件：

```ts
// pages/home/Top.tsx
import React, { Component } from 'react'
import { Grid } from '@ant-design/react-native'
import { observer, inject } from 'mobx-react'

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

  renderTop10(el, index) {
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
            renderItem={this.renderTop10}
          />
        </View>
      </View>
    )
  }
}

export default Top10
```

注意：expo-cli 构建的项目，默认 ts 配置不支持装饰器，会给出如下警告：

```
Experimental support for decorators is a feature that is subject to change in a future release. Set the 'experimentalDecorators' option in your 'tsconfig' or 'jsconfig' to remove this warning.
```

需要修改项目根目录下的 tsconfig.json，添加：

```
"experimentalDecorators": true
```

如果不能起作用，重新启动VSCode即可。

## 添加 top10 样式

```js
// pages/home/style_home.js
import { StyleSheet } from 'react-native'
export default StyleSheet.create({
  // top10
  top10Container: {
    paddingBottom: 44,
    backgroundColor: '#eee'
  },

  top10Head: {
    height: 50,
    paddingLeft: 10,
    justifyContent: 'flex-end',
  },

  top10HeadText: {
    fontSize: 18
  },

  top10ItemContainer: {
    flex: 1,
    paddingRight: 10
  },

  top10DesContainter: {
    marginLeft: 10,
    paddingTop: 10,
    paddingBottom: 10,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#fff'
  },

  top10ImgContainer: {
    paddingLeft: 10,
    paddingTop: 10,
    flex: 1
  },

  Top10Img: {
    width: '100%',
    height: '100%',
  },

  top10Titie: {
    fontSize: 20
  },

  Top10Desc: {
    color: '#666'
  }
})
```