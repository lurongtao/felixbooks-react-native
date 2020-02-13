# 起步
本节将帮助您安装和构建第一个 React Native 应用程序。如果您已经安装了 React Native，那么可以跳到本教程。

如果你是移动开发新手，最简单的入门方法是使用Expo CLI。Expo是一套围绕React Native构建的工具，虽然它有很多功能，最基础的功能是它可以让你在几分钟内编写一个React Native应用程序。你只需要Node.js的最新版本和一个手机或模拟器。如果您想在安装任何工具之前直接在web浏览器中试用React Native，可以试用[Snack](https://snack.expo.io/ Snack)。

如果您已经熟悉移动开发，那么可能需要使用React Native CLI。它需要Xcode或Android Studio才能启动。如果你已经安装了其中一个工具，您应该能够在几分钟内启动并运行。如果没有安装，您应该花大约一个小时来安装和配置它们。

## 使用 Expo CLI
假设已安装 Node.js 10 LTS或更高版本，则可以使用npm安装Expo CLI命令行实用程序：

```
npm install -g expo-cli
``` 

然后运行以下命令，创建一个名为“rn-basics”本地项目：

```
expo init rn-basics

cd rn-basics
npm start # 也可以使用命令: expo start
```

此时会启动一个开发服务器。

## 使用 React Native CLI
*** 待完善 ***

## 运行 React Native 应用程序
在iOS或Android手机上安装[Expo](https://docs.expo.io/versions/v36.0.0/get-started/installation/ Expo)客户端应用程序，并连接到与计算机相同的无线网络（Wifi热点）。在Android上，使用Expo应用程序从终端扫描二维码以打开项目。在iOS上，按照屏幕上的说明（一般为使用相机扫描）获取链接。

### 修改你的程序
现在你已经成功运行了应用程序，让我们修改一下代码试试。在文本编辑器中打开 App.js 并编辑一些行。保存更改后，应用程序会自动重新加载。


