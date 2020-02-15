# ant-design-mobile-rn 快速上手

> 在开始之前，推荐先学习 [React](http://facebook.github.io/react/) 和 [ES2015](http://babeljs.io/docs/learn-es2015/)。我们使用了 `babel`，试试用 [ES2015](http://babeljs.io/blog/2015/06/07/react-on-es6-plus) 的写法来提升编码的愉悦感。
>
> 确认 [Node.js](https://nodejs.org/en/) 已经升级到 v4.x 或以上。

### 1. 创建一个项目

可以是已有项目，或者是使用 create-react-native-app 新创建的空项目，你也可以从 [官方示例](https://github.com/ant-design/antd-mobile-samples/tree/master/rn-web) 脚手架里拷贝并修改

> 参考更多[官方示例集](https://github.com/ant-design/antd-mobile-samples)
> 或者你可以利用 React 生态圈中的 [各种脚手架](https://github.com/enaqx/awesome-react#boilerplates)

完整步骤请查看此处文档： [antd-mobile-sample/create-react-native-app](https://github.com/ant-design/antd-mobile-samples/tree/master/create-react-native-app)

### 2. 安装

```bash
npm install @ant-design/react-native --save
```

or

```bash
yarn add @ant-design/react-native
```

### 链接字体图标

```bash
react-native link @ant-design/icons-react-native
```

### 3. 使用

#### 按需加载

下面两种方式都可以**只加载**用到的组件，选择其中一种方式即可。

- 使用 [babel-plugin-import](https://github.com/ant-design/babel-plugin-import)（推荐）。

  ```js
  // .babelrc or babel-loader option
  {
    "plugins": [
      ["import", { libraryName: "@ant-design/react-native" }] // 与 Web 平台的区别是不需要设置 style
    ]
  }
  ```

  然后改变从 @ant-design/react-native 引入模块方式即可。

  ```jsx
  import { Button } from '@ant-design/react-native';
  ```