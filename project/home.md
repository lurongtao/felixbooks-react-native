# Home.tsx 构建

在项目根目录下创建 pages/home 文件夹，在这个文件夹下创建 Home.tsx 文件，内容如下：

```ts
import React, { Component } from 'react'
import Swiper from './Swiper'
import HotCate from './HotCate'

interface Props {

}

interface State {
  
}

class Home extends Component<Props, State> {
  render() {
    return (
      <>
        <Swiper></Swiper>
        <HotCate></HotCate>
      </>
    )
  }
}

export default Home
```

此时在 Home.tsx 中引入 Swiper 和 HotCate 两个组价。