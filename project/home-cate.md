# 构建 HotCate.tsx 文件

在 pages/home 文件夹里构建 HotCate.tsx 文件，内容为：

```js
import React, { Component } from 'react'
import { Grid } from '@ant-design/react-native'
import { get } from '../../utils/http'

import styles from './style_home'

import {
  View,
  Text,
  Image,
  StyleSheet
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
        style={styles.container}
      >
        {el.img ? <Image source={{uri: el.img}} style={styles.gridImg}></Image> : null}
        <Text style={styles.gridText}>{el.title}</Text>
      </View>
    )
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
      <View>
        <Grid
          data={this.state.hotCate}
          renderItem={this._renderItem}
          hasLine={false}
        ></Grid>
      </View>
    )
  }
}
```

修改 pages/home/style_home.js 文件，样式如下：

```js
import { StyleSheet } from 'react-native'

export default StyleSheet.create({
  // hotcate
  container: {
    paddingTop: 20,
    paddingBottom: 10
  },

  gridContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  },

  gridText: {
    fontSize: 16,
    margin: 6
  },

  gridImg: {
    width: 70,
    height: 70,
    borderRadius: 5
  },
})
```