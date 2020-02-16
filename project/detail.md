# 构建 Detail 页面

在路由信息定义好后，就可以构建详情页了。

## Detail.tsx 

```ts
// pages/detail/Detail.tsx

import React, { Component } from 'react'
import { get } from '../../utils/http'
import {
  View,
  ScrollView,
  Text,
  Image,
  StatusBar,
  TouchableOpacity,
  Alert
} from 'react-native'

import styles from './style_detail'

interface Props {
  navigation?: any,
  route?: any
}
interface State {
  detail: {}
}

export default class Detail extends Component<Props, State> {
  state = {
    detail: null
  }

  async componentDidMount() {
    let result = await get('http://localhost:9000/api/detail')
    this.setState({
      detail: result
    })
    // 根据路由传递过来参数，修改本页的 title
    this.props.navigation.setOptions({ title: this.props.route.params.name })
  }
  
  render() {
    let detail = this.state.detail
    return (
      <>
        {
          detail && (
            <ScrollView>
              <View style={styles.container}>
                <StatusBar backgroundColor="blue" barStyle="light-content" />
                <Image
                  source={{uri: detail.img}}
                  style={styles.mainImg}
                ></Image>
                <View style={styles.mainInfo}>
                  <View>
                    <Text style={styles.mainInfoName}>{detail.name}</Text>
                  </View>
                  <View>
                    <Text style={styles.mainInfoSubTitle}>{detail.all_click}浏览/{detail.favorites}收藏</Text>
                  </View>
                  <TouchableOpacity
                    onPress={() => Alert.alert('已经收藏.')}
                  >
                    <View style={styles.mainInfoButtonWrap}>
                      <Text style={styles.mainInfoButton}>收藏</Text>
                    </View>
                  </TouchableOpacity>
                </View>
                <View style={styles.infoWrap}>
                  <View>
                    <Text style={styles.infoTitle}>心得</Text>
                  </View>
                  <View>
                    <Text style={styles.infoText}>
                      {detail.info}
                    </Text>
                  </View>

                  <View>
                    <Text style={[styles.infoTitle, {marginTop: 20}]}>做法</Text>
                  </View>
                  <View>
                    {
                      detail.makes.map((value) => {
                        return (
                          <View
                            key={value.num}
                          >
                            <View>
                              <Text style={styles.makesTitle}>{value.num} {value.info}</Text>
                            </View>
                            <View>
                              <Image 
                                source={{uri: value.img}}
                                style={styles.makesImg}
                              ></Image>
                            </View>
                          </View>
                        )
                      })
                    }
                  </View>
                </View>
              </View>
            </ScrollView>
          )
        }
      </>
    )
  }
}
```

## style_detail.js 页面样式

```js
// pages/detail/style_detail.js

import { StyleSheet } from 'react-native'

export default StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#eee',
    paddingBottom: 34
  },

  // main
    mainImg: {
      width: '100%',
      height: 250
    },

    mainInfo: {
      height: 170,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#fff'
    },

    mainInfoName: {
      fontSize: 24
    },
    
    mainInfoSubTitle: {
      marginTop: 10,
      fontSize: 12,
      color: '#666'
    },

    // button
    mainInfoButtonWrap: {
      width: 140,
      height: 40,
      backgroundColor: '#df7b42',
      marginTop: 20,
      borderRadius: 6
    },

    mainInfoButton: {
      textAlign: 'center',
      lineHeight: 40,
      color: '#fff',
      fontSize: 16
    },

  // info
    infoWrap: {
      marginTop: 15,
      backgroundColor: '#fff',
      paddingTop: 25,
      paddingLeft: 15,
      paddingBottom: 25
    },
    
    infoTitle: {
      fontSize: 20,
      fontWeight: 'bold',
      marginBottom: 20
    },

    infoText: {
      fontSize: 16,
      lineHeight: 20,
      paddingRight: 20
    },
  
  // makes
    makesTitle: {
      fontSize: 16,
      paddingRight: 30
    },

    makesImg: {
      width: 300,
      height: 210,
      margin: 20
    }
})
```