
---
title: Demo 使用指南
description: v1.1.1
platform: Linux
updatedAt: Thu Aug 15 2019 03:54:45 GMT+0800 (CST)
---
# Demo 使用指南
本文指导开发者在正式将 RTSA SDK 集成到项目中之前，编译并运行模拟数据 Demo 对进行初步了解。

## 前提条件
请确保开发环境满足以下要求：

- gcc 4.4 及以上
- kernel 3.0 及以上

兼容性：如你的硬件环境无法集成声网 SDK，请联系声网提供你的编译工具链。

请获取以下文件：
- 最新版本的 Agora RTSA SDK for Linux。
- 模拟数据 Demo。

## <a name = "appid"></a>创建 Agora 账号并获取 App ID
1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/RTSA/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 编译运行模拟数据 Demo
步骤如下：

1. 打开 Terminal，并进入 RTD_DATA_DEMO/bin 目录，将 bin/PLACEHOLDER 重命名为 bin/app_id_and_cert.h。
 ~~~shell
cd RTD_DATA_DEMO/bin
mv PLACEHOLDER app_id_and_cert.h
~~~

2. 在 bin/app_id_and_cert.h 文件中填写你的 App ID。

 >如果启用了 Agora License 机制，还需要填写 Certificate，用于绑定设备。详见  [Agora License 机制文档](../../cn/Agora%20Platform/license_mechanism_v3.md)。

3. 将 SDK 包中的 include 文件夹和 libagora-rtc-sdk.so 复制至 lib 文件夹下。

4. 进行编译：
 ~~~shell
mkdir build && cd build
cmake ..
make -j8
~~~

5. 编译成功后，在 bin 文件夹中会生成一个可执行程序 demo。输入以下命令行获取帮助：
 ~~~shell
./bin/demo --help
~~~

 可设置的运行参数列表如下：
 
 | 参数                        | 描述                                                       |
|-----------------------------|------------------------------------------------------------|
| app_id                      | App ID，项目的唯一标识。                                   |
| cert [Optional]             | Certificate for License，用于绑定 IoT 设备。               |
| audio_bps [Optional]        | 音频码率，int32 类型。                                     |
| audio_fps [Optional]        | 音频帧率，int32 类型。                                     |
| channel [Optional]          | 频道名，string 类型。                                      |
| duration [Optional]         | 频道时长（秒），int32 类型。                               |
| key_interval [Optional]     | 关键帧间隔（毫秒），uint32 类型。                          |
| round [Optional]            | Demo 运行的次数，uint32 类型。从加入频道到退出频道为一轮。 |
| video_fps [Optional]        | 视频帧率，int32 类型。                                     |
| video_target_bps [Optional] | 视频目标码率，int32 类型。                                 |
| verbose [Optional]          | 啰嗦模式。启用该模式后会打印更多信息。                     |


6. 设置参数并运行：

 ~~~shell
./bin/demo --channel hello_world_opps
~~~


