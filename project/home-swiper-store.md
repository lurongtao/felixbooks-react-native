# 改造 Swiper.tsx 组件

Swiper 组件和 Top10 组件共享了数据，因此在 store 构建好后，需要改造一下：

```ts
// pages/home/Swiper.tsx
import React, { Component } from 'react'
import { Carousel } from '@ant-design/react-native'
import { get } from '../../utils/http'

import { observer, inject } from 'mobx-react'

import {
  View,
  Image
} from 'react-native'

import styles from './style_home'

interface Props {
  // store 作为组件的 props
  store?: any
}

interface State {
  
}

// 注入 store 与 将类变为可观察的对象
@inject('store')
@observer
class Swiper extends Component<Props, State> {
  state = {
    list: []
  }
  async componentDidMount() {
    let list = await get('http://localhost:9000/api/swiper')
    this.props.store.setList(list)
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
          this.props.store.swiper.map((value, index) => {
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

# 改造 Home.tsx 组件

在 Home.tsx 组件引入 Top10 组件，同时添加 ScrollView 组件，实现页面滚动效果。

```js
// page/home/Home.tsx

import React, { Component } from 'react'
import { ScrollView } from 'react-native'

import Swiper from './Swiper'
import HotCate from './HotCate'
import Top10 from './Top10'

interface Props {

}

interface State {
  
}

class Home extends Component<Props, State> {
  render() {
    return (
      <ScrollView>
        <Swiper></Swiper>
        <HotCate></HotCate>
        <Top10></Top10>
      </ScrollView>
    )
  }
}

export default Home
```