# Swiper.tsx 组价构建

在根目录下创建 utils 文件夹，在这个文件夹里创建 http.js 文件，内容如下：

```js
// utils/http.js
export const get = (url) => {
  return fetch(url, {
    method: 'get'
  })
  .then(response => response.json())
  .then(result => {
    return result.data
  })
}
```

在 pages/home 文件夹里再创建一个 Swiper.tsx 组件，内容如下：

```ts
import React, { Component } from 'react'
import { Carousel } from '@ant-design/react-native'
import { get } from '../../utils/http'

import {
  View,
  Image
} from 'react-native'

import styles from './style_home'

interface Props {

}

interface State {
  list: Array<any>
}

class Swiper extends Component<Props, State> {
  state = {
    list: []
  }
  async componentDidMount() {
    let list = await get('http://localhost:9000/api/swiper')
    this.setState({
      list
    })
  }

  render() {
    return (
      <Carousel
        style={styles.wrapper}
        selectedIndex={0}
        autoplay
        infinite
      >
        {
          this.state.list.slice(0, 5).map((value, index) => {
            return (
              <View
                style={styles.containerHorizontal}
                key={value.id}
              >
                <Image
                  source={{uri: value.img}}
                  style={styles.slideImg}
                  resizeMode='cover'
                ></Image>
              </View>
            )
          })
        }
      </Carousel>
    )
  }
}

export default Swiper
```

在 page/home 文件里创建 style_home.js 文件，编辑样式如下：

```js
import { StyleSheet } from 'react-native'

export default StyleSheet.create({
  // swiper
  wrapper: {
    height: 170
  },

  containerHorizontal: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    height: 170
  },

  slideImg: {
    height: 170,
    width: '100%'
  },
})
```