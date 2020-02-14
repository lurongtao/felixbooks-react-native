# 编写 Index组件 基本结构

在根目录下创建 `pages/index` 文件夹，在里面创建一个 `Index.tsx` 文件，编辑内容：

```ts
// pages/index/Index.tsx
import React, { Component } from 'react'

import {
  View,
  Text,
  StyleSheet
} from 'react-native'

interface Props {
  
}

interface State {
  
}

export default class Index extends Component<Props, State> {
  constructor(props) {
    super(props)
  }
  
  state: State = {
    
  }

  componentDidMount() {
    
  }

  render() {
    return (
      <View style={styles.container}>
        <Text>
          Index 组件内容
        </Text>
      </View>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
})
```
修改根目录下的 `App.tsx`：

```ts
import React from 'react'
import Index from './pages/index/Index'

export default function App() {
  return (
    <Index></Index>
  )
}
```