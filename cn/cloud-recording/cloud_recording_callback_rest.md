
---
title: 云端录制 RESTful API 回调服务
description: Cloud recording restful api callback
platform: All Platforms
updatedAt: Thu Jun 13 2019 07:47:46 GMT+0800 (CST)
---
# 云端录制 RESTful API 回调服务
云端录制 RESTful API 提供回调服务，你可以配置一个接收回调的 HTTP/HTTPS 服务器地址来接收云端录制的事件通知。当事件发生时，Agora 服务器会通过 HTTP/HTTPS 请求将事件投递给你的服务器。

## 开通回调服务

你需要联系 sales@agora.io 开通回调服务，并提供以下信息：

- 用于接收回调的 HTTP/HTTPS 服务器地址
- 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id)
- 需要接收通知的事件类型

## 数据格式

### 消息组织格式

通知消息以 HTTP/HTTPS POST 请求发送给你的服务器，请求包体为 JSON 格式。

POST 请求中 `ContentType` 为 `application/json`。

### 响应格式

你的服务器接收到事件通知后，需要通过响应告知 Agora 服务器，响应内容格式为 `application/json`。

以下情况 Agora 服务器会认为通知发送失败：

- 超过 20 秒未响应
- 响应的 HTTP 状态码不为 200

通知发送失败后 Agora 服务器会立即重新发送，最多尝试发送 5 次。

## 公共的回调信息

每种类型的事件通知消息都会包含以下字段：

- `notificationId`：String 类型，通知 ID，是通知的唯一标识。
- `eventType`：Number 类型，事件类型，详见[回调事件](#event)。
- `eventMs`：Number 类型，Agora 云端录制触发事件的时间（Unix 时间戳），单位 ms。
- `notifyMs`：Number 类型，Agora 服务器通知你的服务器的时间（Unix 时间戳），单位 ms，重新发送通知时该字段会更新。
- `payload`：JSON 类型，事件具体内容。每种类型的事件通知中 `payload` 都会包含以下字段：
  - `cname`：String 类型，录制的频道名称。
  - `uid`：String 类型，录制使用的 UID。
  - `sid`：String 类型，录制 ID，一次云端录制的唯一标识。
  - `sequence`：Number 类型，消息序列号，从 0 开始计数。消息可能乱序到达或者丢失重发，可以通过该参数标识消息。
  - `sendts`：Number 类型， Agora 服务器开始发送消息的时间 （UTC 时间）。
  - `serviceType`：Number 类型，Agora 回调服务的类型，0 代表云端录制。
  - `noticeLevel`：Number 类型，消息级别，数字越大代表需要关注的程度越高。
    - `1`：debug 级别
    - `2`：info 级别
    - `3`：notice 级别
    - `4`：important 级别
    - `5`：critical 级别
  - `details`：JSON 类型，具体的消息内容，详见下面每个事件的描述。

## <a name="event"></a>回调事件

云端录制回调的事件类型（`eventType`）共有以下几种：

- [`1`](#1)：云端录制服务发生错误
- [`2`](#2)：云端录制服务发生警告
- [`3`](#3)：云端录制服务状态发生变化
- [`4`](#4)：生成录制索引文件
- [`30`](#30)：上传服务已启动
- [`31`](#31)：所有录制文件已上传至指定的第三方云存储
- [`32`](#32)：所有录制文件已经全部上传完成，但至少有一片上传到 Agora 备份云
- [`33`](#33)：当前上传的进度
- [`40`](#40)：录制服务已启动
- [`41`](#41)：录制服务已退出

### <a name="1"></a>cloud_recording_error

`eventType` 为 1 表示云端录制服务发生错误， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `"cloud_recording_error"`。
- `module`：Number 类型，发生错误的模块。
  - `0`：录制模块
  - `1`：上传模块
  - `2`：云端录制服务
- `errorLevel`：Number 类型，错误级别。
  - `1`：debug
  - `2`：minor
  - `3`：medium
  - `4`：major
  - `5`：fatal。fatal 级别的错误很可能导致录制退出，如果收到该级别的消息请及时调用 [query](../../cn/cloud-recording/cloud_recording_api_rest.md) API 查询当前状态，并结合错误消息的内容进行处理。
- `errorCode`：Number 类型，错误码。如果错误发生在录制模块，请参考[错误代码和警告代码](https://docs.agora.io/cn/Recording/the_error_native)；如果错误发生在上传模块，请参考[上传错误码](#uploaderr)；如果错误发生在云端录制平台模块，请参考[云端录制平台错误码](#clouderr)；如果错误码未列出，请联系 Agora 技术支持。
- `stat`：Number 类型，事件状态，0 表示正常，其他值表示异常。
- `errorMsg`：String 类型，具体的错误信息。

### <a name="2"></a>cloud_recording_warning

`eventType` 为 2 表示云端录制服务发生警告， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_warning`。
- `module`：Number 类型，发生警告的模块名。
  - `0`：录制模块
  - `1`：上传模块
- `warnCode`：Number 类型，警告码。如果警告发生在录制模块，请参考[错误代码和警告代码](https://docs.agora.io/cn/Recording/the_error_native)；如果警告发生在上传模块，请参考[上传警告码](#uploadwarn)。

### <a name="3"></a>cloud_recording_status_update

`eventType` 为 3 表示云端录制服务状态发生改变， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_status_update`。
- `status`：Number 类型，云端录制当前的状态，请参考[云端录制服务状态码](#state)。
- `recordingMode`：Number 类型，录制模式，目前仅支持合流模式，该值为 0。
- `fileList`：String 类型，录制生成的 M3U8 索引文件名。

### <a name="4"></a>cloud_recording_file_infos

`eventType` 为 4 表示已生成录制索引文件并上传。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的切片 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。

 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_file_infos`。
- `fileList`：String 类型，生成的 M3U8 文件名。

### <a name="30"></a>uploader_started

`eventType` 为 30 表示上传服务已启动， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploader_started`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="31"></a>uploaded

`eventType` 为 31 表示所有录制文件已上传至指定的第三方云存储， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploaded`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="32"></a>backuped

`eventType` 为 32 表示所有录制文件已经全部上传完成，但至少有一片上传到 Agora 备份云， Agora 备份云会自动将这部分文件上传到指定的第三方云存储。 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `backuped`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="33"></a>uploading_progress

`eventType` 为 33 表示当前上传的进度，录制开始后每分钟会通知一次。 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploading_progress`。
- `progress`：Number 类型，0 到 10000 之间的数字，当前已上传文件与已录制的文件的比例乘以 10000。这个数字是不断变动的，录制退出后，到达 10000 表示上传完成。

### <a name="40"></a>recorder_started

`eventType` 为 40 表示录制服务已启动， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `recorder_started`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="41"></a>recorder_leave

`eventType` 为 41 表示录制服务已退出， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `recorder_leave`。
- `leaveCode`：Number 类型，退出码，为以下多个原因的组合。
  - 0：初始化失败
  - 1：由信号触发的退出
  - 2：频道内除录制 App 外，没有其他用户
  - 4：捕获到信号错误
  - 8：wrapper 层主动退出

## 参考

### <a name="uploaderr"></a>上传错误码

| 警告码 | 描述                 |
| :----- | :------------------- |
| 32     | 第三方云存储信息错误 |
| 47     | 文件上传失败         |
| 51     | 上传时文件操作发生错误     |

### <a name="uploadwarn"></a>上传警告码

| 警告码 | 描述               |
| :--- | :------------------ |
| 31   | 重传到指定的云存储    |
| 32   | 重传到 Agora 备份云 |

### <a name="clouderr"></a>云端录制平台错误码

| 错误码 | 描述              |
| :--- | :------------------- |
| 50   | 上传超时             |
| 52   | 云端录制服务启动超时 |

### <a name="state"></a>云端录制服务状态码

| 状态码 | 描述                   |
| :--- | :----------------------- |
| 0    | 没有开始云端录制           |
| 1    | 云端录制初始化完成        |
| 2    | 录制组件开始启动           |
| 3    | 上传组件已启动       |
| 4    |录制组件启动完成           |
| 5    | 已成功上传第一个文件  |
| 6    | 已经停止录制       |
| 7    | 云端录制服务全部停止     |
| 8    | 云端录制准备退出         |
| 20   | 云端录制异常退出           |