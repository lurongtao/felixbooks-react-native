# 项目发布

本项目发布利用expo发布功能，详细可参考 [构建独立的应用程序](https://docs.expo.io/versions/v36.0.0/distribution/building-standalone-apps/)

## 安装 Expo CLI

此步骤已经完成。

## 配置 app.json

```json
{
  "expo": {
    "name": "rn-cookbooks",
    "slug": "rn-cookbooks",
    "privacy": "public",
    "sdkVersion": "36.0.0",
    "platforms": [
      "ios",
      "android",
      "web"
    ],
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "updates": {
      "fallbackToCacheTimeout": 0
    },
    "assetBundlePatterns": [
      "**/*"
    ],
    "ios": {
      "bundleIdentifier": "com.qianfeng.felixlu",
      "buildNumber": "1.0.0"
    },
    "android": {
      "package": "com.qianfeng.felixlu",
      "versionCode": 1
    }
  }
}
```

## 开始Build

```
expo build:android
```

或：

```
expo build:ios
```

## build 过程监测

输入以上命令后，会在控制台看到下边信息：

```
Build started, it may take a few minutes to complete.
You can check the queue length at https://expo.io/turtle-status

You can monitor the build at

https://expo.io/dashboard/felixlurt/builds/15b2ae11-c98d-48dc-879e-9ff05fb0b9f1
```
可以通过访问 `https://expo.io/dashboard/felixlurt/builds/15b2ae11-c98d-48dc-879e-9ff05fb0b9f1` 来监控build过程。

build 成功后，点击 “Download” 按钮即可下载打完的APP安装包了。

> 注：iOS 需要有开发者账号，没有账号的同学建议运行 `expo build:android`进行试验