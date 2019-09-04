
---
title: 媒体播放器组件
description: 
platform: Windows
updatedAt: Wed Sep 04 2019 07:00:13 GMT+0800 (CST)
---
# 媒体播放器组件
## 功能描述

当你使用 Agora Native SDK 实现音视频互动直播时，如果你想播放本地的视频文件以及在线的视频流、以及由 SD-RTN 分发给远端用户观看，那么你需要使用媒体播放器组件播放器组件。
媒体播放器组件支持将本地视频文件以及在线视频流本地播放，或者发送到对端、对端接收流播放；支持播放器的播放、暂停、恢复、跳转是视频文件某一位置等功能。


<a name="format"></a>
### 支持格式

媒体播放器组件支持播放以下视频：
- 本地视频：文件格式为 AVI、MP4、MKV 和 FLV。
- 在线视频：RTMP 流和 RTSP 流。

<div class="alert note">不管是本地视频还是在线视频，只有采样率为 32 kHz、44100 Hz 或 48 kHz 的单/双声道视频才能在媒体播放器中正常播放。</div>

### 使用指南

使用媒体播放器组件时，你可以开始/暂停播放视频，调节播放进度，调节播放音量，选择是否将播放的视频发布给远端用户观看：

- 你可以以主播身份进入频道并将媒体播放器组件播放的视频也一同分发给远端用户，让远端用户同时看到两个视频窗口。在这种场景下，你需要使用双进程。一个进程里采集并发送你作为主播的直播视频，另一个进程里采集并发送播放器播放的视频。如果你只以一个进程加入频道，那么远端用户只能看到与这个进程对应的一个视频窗口。
<div class="alert note">因为 AgoraRtcEngine 是单实例，所以在这种场景下，你需要使用双进程。在一个进程里，创建 AgoraRtcEngine，采集并发布主播的直播视频；在另一个进程里，创建 AgoraRtcEngine 和 MediaPlayerKit，发布媒体播放器组件播放的视频流。本文仅介绍后一个进程的使用。</div>

- 你也可以将媒体播放器组件作为你的本地播放器使用，本文对这种情况不多加讨论。

## 快速使用媒体播放器组件

### 前提条件

- 操作系统 Windows 7 及以上。
- 开发工具 Microsoft Visual Studio 2015 及以上。

### 使用 MediaPlayerKitQuickstart

1. 下载并解压 [MediaPlayerKitQuickstart](https://github.com/AgoraIO/Advanced-Video/tree/master/MediaPlayer/Mediaplayer-Windows) 文件。
2. 使用 Microsoft Visual Studio 打开 `MediaPlayerKitQuickstart.sln` 文件。
3. 点击**生成** > **生成解决方案**。
4. 编译成功后，点击**调试** > **开始调试**。
5. 调试成功后，你可以看到一个播放着视频的窗口。

你可以在 “`.cpp`” 文件如下代码里修改视频路径，让媒体播放器组件播放你想要的视频：

``` C++
//视频路径
std::string path = "F://1080.mp4";
  char *videopath = (char *)path.data();
  mediaPlayerKit->load(videopath, true);
```

## 实现媒体播放器组件

### 前提条件

- 操作系统 Windows 7 及以上。
- 开发工具 Microsoft Visual Studio 2015 及以上。

### 集成媒体播放器组件

**1. 准备工作**

- 下载并解压 Agora Native SDK（详见[下载](https://docs.agora.io/cn/Agora%20Platform/downloads)专区的**视频通话/视频互动直播 SDK**）
- 请确保你已完成 Agora Native SDK 的集成工作，详见[集成客户端](../../cn/Interactive%20Broadcast/windows_video.md)。
- 下载并解压 MediaPlayerKit 文件。

**2. 创建项目**

<div class="alert warning"> 在此步，你应该基于已有的集成了 Agora Native SDK 的项目的继续集成 MediaPlayerKit 的项目，而不是从头创建一个新项目。此处仅是展现一个新建项目的步骤，如果你不需要参考，可以略过此步。</div>

- 新建一个基于 **Visual C++** 的 **Windows 桌面应用程序**，点击**确定**。
- 在这个空项目中点击**生成** > **生成解决方案**。
- 编译成功后，点击**调试** > **开始调试**。
- 调试成功后，开始下一步。

**3. 导入文件**
- 将 `MediaPlayerKit` 和 Agora Native SDK 的 `sdk` 文件夹拷贝到你的项目文件中。
- 点击**源文件**，右键**添加** > **现有项目**，导入 `MediaPlayerKit/videokit` 文件。

**4. 导入路径**
点击你的项目文件，右键点击**属性**。
- 点开 **C/C++** > **常规**，在**附加包含目录** 中填入以下路径：
	- `./sdk/include;`
	- `./MediaPlayerKit/include;`
	- `./MediaPlayerKit/videokit;`
- 点开**链接器** > **常规**，在**附加库目录**中填入以下路径：
	- `./MediaPlayerKit/lib;`
	- `./sdk/lib;`
- 点开**链接器** > **输入**，在**附加依赖项**中填入以下路径：
	- `./MediaPlayerKit/lib/re_sampler.lib;`
	- `./MediaPlayerKit/lib/MediaPlayerKit.lib;`
	- `./sdk/lib/agora_rtc_sdk.lib;`

**5. 编译项目**
- 点击你的项目文件，右键点击**属性**，点开 **C/C++** > **预编译头**，在**预编译头**中选择**不使用预编译头**。
- 点击**生成** > **生成解决方案**

编译成功后，你可以参考[调用媒体播放器组件接口](#1)或 [API 文档](#2)完成你的项目。

<a name="1"></a>
### 调用媒体播放器组件接口


**API 时序图**

![](https://web-cdn.agora.io/docs-files/1567498507910)

> - 图中仅展示最基本的接口，如果你有更复杂的需求，也可调用 Agora Native SDK 里更多的接口，比如 [`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a72846f5bf13726e7a61497e2fef65972)。
> - 因为 MediaPlayer Kit 接口中的 `setVideoView` 可以设置本地视频窗口，所以你无需事先调用 Agora Native SDK 接口中的 [`setupLocalVideo`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a744003a9c0230684e985e42d14361f28)。

1. 调用 Agora Native SDK 接口完成初始化和基本的视频设置。

	- 调用 `create` 方法创建一个 AgoraRtcEngine 实例，并调用 `initialize` 方法完成初始化。详见[初始化实现方法](https://docs.agora.io/cn/Interactive%20Broadcast/initialize_windows_live?platform=Windows#实现方法)。
	- 调用 `setChannelProfile` 方法设置频道模式为直播模式。详见[设置频道模式实现方法](https://docs.agora.io/cn/Interactive%20Broadcast/join_live_windows?platform=Windows#设置频道模式为直播)。
	- 调用 `setClientRole` 方法，根据需要将用户设置为主播。

	```C++
	int nRet = m_lpAgoraEngine->setClientRole(role);
	```
	- 调用 `enableVideo` 方法打开视频模式。详见[打开视频模式](https://docs.agora.io/cn/Interactive%20Broadcast/publish_windows_live?platform=Windows#打开视频模式)。

2. 调用 MediaPlayer Kit 接口完成初始化和其他准备工作。
	- 调用 `createMediaPlayerKitWithRtcEngine` 方法创建引擎。
    ```C++
	 mediaPlayerKit = createMediaPlayerKitWithRtcEngine(rtcEngine,sampleRate);
	 ```
	- 调用 `setEventHandler` 和 `setMediaInfoCallback` 方法设置事件回调和媒体信息回调。
> 你可以参考 [API 文档](#2) 了解 MediaPlayer Kit 接口的四个回调。

	```C++
	mediaPlayerKit->setEventHandler(handler);
	mediaPlayerKit->setMediaInfoCallback(infoCallback);
	```

	- 调用 `setVideoView` 方法设置本地视图。
	
	```C++
	mediaPlayerKit->setVideoView(hwnd);
	```

3. 调用 Agora Native SDK 接口完成频道管理。
	- 调用 `joinChannel` 方法加入频道。详见[加入主播频道](https://docs.agora.io/cn/Interactive%20Broadcast/join_live_windows?platform=Windows#加入直播频道)。
> 你需要在 `setVideoView` 后调用本方法。

	- 调用 [`setupRemoteVideo`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac166814787b0a1d8da5f5c632dd7cdcf) 方法设置远端用户视图。

4. 调用 MediaPlayer Kit 接口载入视频。
	- 调用 `load` 方法将视频载入内存。
> 你可以按照自己的需求，从本地路径或 URL 路径载入视频。只要它符合视频格式，详见[支持格式](#format)。

	```C++
	mediaPlayerKit->load(url.toUtf8().data(),true);
	```

<a name="3"></a>
5. 调用 MediaPlayerKit 接口完成媒体播放器功能。
	- 调用 `play`、`adjustPlaybackSignalVolume`、`seekTo`、`getCurrentPosition`、`pause` 和 `resume` 等方法实时调节播放器。
> 你可以参考 [API 文档](#2)了解这些方法。

	```C++
	mediaPlayerKit->play();
	mediaPlayerKit->adjustPlaybackSignalVolume(volume);
	mediaPlayerKit->seekTo(msec);
	mediaPlayerKit->getCurrentPosition();
	mediaPlayerKit->pause();
	mediaPlayerKit->resume();
	```

6. 调用 MediaPlayer Kit 接口将音视频发布给远端用户观看。

	- 如果你只想将音频发布给远端用户：
		- 调用 `publishAudio` 方法发布音频。
	```C++
	mediaPlayerKit->publishAudio();
	```

		- 使用[步骤 5](#3) 中的接口实时调节播放器。
	
		- 调用 `adjustPublishSignalVolume` 方法调节远端用户接收到的音量。
	```C++
	mediaPlayerKit->adjustPublishSignalVolume(volume);
	```
	
		-  调用 `unpublishAudio` 方法即可停止把音频发布给远端用户。
	```C++
	mediaPlayerKit->unpublishAudio();
	```

	- 如果你想将视频发布给远端用户：
		- 调用 `publishVideo` 方法发布视频。
 ```C++
mediaPlayerKit->publishVideo();
```
		- 使用[步骤 5](#3) 中的接口实时调节播放器。
		- 调用 `adjustPublishSignalVolume` 方法调节远端用户接收到的音量。
	```C++
	mediaPlayerKit->adjustPlaybackSignalVolume(volume);
	```
	
		- 调用 `unpublishVideo` 方法即可停止把视频发布给远端用户。
	```C++
	mediaPlayerKit->unpublishVideo();
	```

7. 调用 MediaPlayer Kit 接口停止播放并退出媒体播放器。
	- 调用 `stop` 方法停止播放视频。

	```C++
	mediaPlayerKit->stop();
	```

	- 调用 `unload` 方法从释放加载到内存的视频。
	
	```C++
	mediaPlayerKit->unload();
	```

	- 调用 `destroy` 方法销毁一个 MediaPlayerKit 实例并退出媒体播放器组件。
> 调用 `destroy` 方法后， 取消绑定本地视频流的显示视图。

	```C++
	mediaPlayerKit->destroy();
	```

<a name="2"></a>
## API 文档
详见 [API 文档](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/mediaplayer_cpp/index.html)

## 注意事项

如果你用媒体播放器组件播放 URL 路径的视频，遇到因为网络问题而导致的媒体播放器组件播放错误后，你需要在网络情况好转后，调用 `load` 方法重新载入视频。
