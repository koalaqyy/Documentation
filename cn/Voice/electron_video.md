
---
title: 集成客户端
description: 
platform: Electron
updatedAt: Thu Aug 15 2019 08:35:43 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora SDK for Electron 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

请确保满足以下开发环境要求：

- Node.js 6.9.1 及以上
- Electron 1.8.3 及以上

> 使用 Windows 平台进行开发时，请运行 `npm install -D —arch = ia32 electron` 安装 32 位的 Electron。

## 创建项目并获取 App ID

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 集成 SDK

你可以通过 npm 直接安装并导入 SDK，也可以前往官网下载 SDK 后再进行导入：

**通过 npm 直接安装 SDK**

1. 在你的项目文件路径，运行如下命令行安装最新版的 Agora SDK for Electron：

	`npm install agora-electron-sdk`

2. 然后通过如下代码将 SDK 引入至你的项目中

	`import AgoraRtcEngine from 'agora-electron-sdk'`
	
**官网下载 SDK 并引入**

1. 前往官网 [SDK Downloads](https://docs.agora.io/cn/Agora%20Platform/downloads) 页面下载 Agora SDK for Electron
2. 将下载下来的 SDK 拷贝至你的项目根目录下
3. 通过如下代码将 SDK 引入至你的项目中

	`import AgoraRtcEngine from 'agora-electron-sdk'`

> 如果你选择官网下载并引入的方式，请务必使用 Eletron 3.0.6。

## 修改 .npmrc 文件切换预编译版本

Agora 默认使用 1.8.3 版本的 Electron 进行编译。请根据你的 Electron 版本修改 .npmrc 文件，切换预编译版本：

```
// Downloads a prebuilt add-on with Electron 1.8.3
AGORA_ELECTRON_DEPENDENT = 2.0.0
// Downloads a prebuilt add-on with Electron 3.0.6
AGORA_ELECTRON_DEPENDENT = 3.0.6
// Downloads a prebuilt add-on With Electron 4.0.0
AGORA_ELECTRON_DEPENDENT = 4.0.0
```

## 安装依赖项

在你的项目文件夹下运行 `nmp install` 安装依赖项。安装会自动触发 `npm run download`，你也可以到对应路径下手动安装。
如果你想用 Xcode 或 Visual Studio 调试，可以执行 `npm run debug` 命令行生成项目文件及带符号表的 SDK 文件。

至此，你已经将 Agora SDK for Electron 集成到你的项目中了。请参考 [Agora Electron Github Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) 在你的项目中实现相关的实时音视频功能。

## SDK 开源

[Agora SDK for Electron](https://www.npmjs.com/package/agora-electron-sdk) 在 Github 上开源，你可以前往参考或查阅源代码。Agora 也欢迎开发者贡献代码，以提高 Electron SDK 的易用性。
