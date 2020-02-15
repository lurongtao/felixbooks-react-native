# 构建 List.tsx 组件

接下来构建另一个页面，首先在 pages 目录下创建 list 文件夹，在此文件夹里创建 List.tsx 组件文件和 style_list.js 样式文件。

## List.tsx
```js
// pages/list/List
import React, { Component, createRef } from 'react'

import {
  inject,
  observer
} from 'mobx-react'

import {
  View,
  Text,
  Image,
  FlatList
} from 'react-native'

import styles from './style_list'

interface Props {
  store?: any
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

  // 渲染 Flatlist 组件数据
  _renderItem(item) {
    let {img, name, burdens, all_click, favorites} = item.item.data   
    return (
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
    )
  }

  // 处理用户拉到底端的响应函数
  _handleReachEnd() {
    // 如果还有数据，一直加载
    if (this.state.curPage < Math.ceil(this.props.store.list.length / pageSize)) {
      this.setState((state) => {
        return {
          curPage: state.curPage + 1
        }
      }, () => {
        this._loadData()
      })
    }
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
      <FlatList
        ref={this.flatlist}
        renderItem={this._renderItem.bind(this)}
        data={this.state.datalist}
        refreshing={this.state.refresh}
        onEndReached={this._handleReachEnd.bind(this)}
        onEndReachedThreshold={1}
        onRefresh={this._handleRefresh.bind(this)}
      ></FlatList>
    )
  }
}
```

## style_list.js 样式

```js
import { StyleSheet } from 'react-native'

export default StyleSheet.create({
  listWrap: {
    flexDirection: 'row',
    padding: 10,
    borderBottomWidth: 1,
    borderStyle: 'solid',
    borderBottomColor: '#eee'
  },

  imgWrap: {
    width: 135,
    paddingRight: 10
  },

  image: {
    width: 115,
    height: 75
  },

  descWrap: {
    flex: 1
  },

  title: {
    fontSize: 20,
    lineHeight: 30
  },

  subtitle: {
    fontSize: 16,
    color: '#666',
    lineHeight: 30,
    overflow: 'hidden'
  },

  desc: {
    fontSize: 12,
    color: '#666'
  }
})
```