
---
title: Integrate the SDK
description: 
platform: Electron
updatedAt: Thu Aug 15 2019 08:07:30 GMT+0800 (CST)
---
# Integrate the SDK
This page contains information on how to prepare the development environment before enabling a call/video broadcast with the Agora SDK for Electron.

## Prerequisites

Development environment:

- Node.js 6.9.1 or later
- Electron 1.8.3 or later

> If you use Windows for development, ensure that you run npm install -D —arch = ia32 electron to install  a 32-bit Electron.

## Create an Agora Project and Get an App ID
1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Video/sign_in_and_sign_up.md).

2. Click **Get Started** under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523371446)

3. Input your project name in the pop-up window and click **Create**. Follow the on-screen instructions to get to know the basic steps to start a video call. Once the project is created, you can find it under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523478084)
	
4. Click the **Edit** button behind the new project, or the **Project Management** button ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to go to the **Project Management** page.

 ![](https://web-cdn.agora.io/docs-files/1563523678240)

5. On the **Project Management** panel, find the **App ID** of your project.

 ![](https://web-cdn.agora.io/docs-files/1563523737158)

## Add the Agora SDK to Your Project

You can directly install the SDK through npm or download it from Agora's developer portal.

**Install the SDK through npm**

1. Go to the project path, and run the following command line to install the latest version of the Electron SDK:

	`npm install agora-electron-sdk`
	
2. Import the SDK into your project with the following code:

	`import AgoraRtcEngine from 'agora-electron-sdk'`

**Download the SDK from the developer portal**

1. Go to [SDK downloads](https://docs.agora.io/en/Agora%20Platform/downloads) to download the Agora SDK for Electron.
2. Save the downloaded SDk package into the root directory of your project file.
3. Import the SDK into your project with the following code:

	`import AgoraRtcEngine from 'agora-electron-sdk'`
	
> Ensure that you use Electron 3.0.6 if you want to get the SDK by downloading it from the developer portal.

## Switch the Prebuilt add-on version

By default, Agora uses Electron 1.8.3 to build the project. Switch the prebuilt add-on version in the .npmrc file according to your Electron version:

```
// Downloads a prebuilt add-on with Electron 1.8.3
AGORA_ELECTRON_DEPENDENT = 2.0.0
// Downloads a prebuilt add-on with Electron 3.0.6
AGORA_ELECTRON_DEPENDENT = 3.0.6
// Downloads a prebuilt add-on With Electron 4.0.0
AGORA_ELECTRON_DEPENDENT = 4.0.0
```

## Install the Dependency
Under the project path, run ` nmp install` to install the dependency and trigger the `npm run download` command. You can also install the dependency manually.
If you want to debug with Xcode or Visual Studio, run `npm run debug` to generate project files and SDK files for the debug environment. 

You have now integrated the Agora SDK for Electron into your project. Refer to  [Agora Electron Github Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) to implement various real-time communication functions in your project.

## Open-source SDK

The [Agora SDK for Electron](https://www.npmjs.com/package/agora-electron-sdk) is open source in Github. You can download it and refer to the source code. Agora welcomes contributions from developers to improve the usability of the Electron SDK.
