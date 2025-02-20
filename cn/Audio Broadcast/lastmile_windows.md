
---
title: 通话前检测网络质量
description: 通话前的网络质量检测
platform: Windows
updatedAt: Thu Jul 18 2019 03:42:38 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

在加入频道或切换角色为主播前，进行网络质量探测，可以判断或预测用户当前的网络状况是否良好，可以满足音频码率或者当前选定的视频属性的目标码率。

在对网络质量要求高的场景下，Agora 建议在加入频道前进行探测，保证通信顺畅。

> 纯语音产品使用 48 Kbps 的固定探测码率；视频产品会根据当前选定的视频属性调整探测码率。

## 实现方法

请确保你已完成环境准备、安装包获取等步骤，详见集成客户端。

在用户加入频道或上麦前，调用 `startLastmileProbeTest` 进行网络质量探测，向用户反馈上下行网络的带宽、丢包、网络抖动和往返时延。

启用该方法后，SDK 会依次返回如下 2 个回调：

- `onLastmileQuality`：约 2 秒内返回。该回调通过打分反馈上下行网络质量，更贴近主观感受
- `onLastmileProbeResult`：约 30 秒内返回。该回调通过客观数据反馈上下行网络质量，更客观

```cpp
// 注册回调接口
// 开始 Last-mile 网络探测后，约 2 秒后发生该回调
void onLastmileQuality(int quality) {
}

// 开始 Last-mile 网络探测后，约 30 秒后发生该回调
void onLastmileProbeResult(LastmileProbeResult) {
  // (1)可以选择在回调内部结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
  lpAgoraEngine->stopLastmileProbeTest();
}

// 配置一个 LastmileProbeConfig 实例
LastmileProbeConfig config;
// 确认探测上行网络质量
config.probeUplink = true;
// 确认探测下行网络质量
config.probeDownlink = true;
// 期望的最高发送码率，单位为 Kbps，范围为 [100, 5000]
config.expectedUplinkBitrate = 1000;
// 期望的最高接收码率，单位为 Kbps，范围为 [100, 5000]
config.expectedDownlinkBitrate = 1000;
// 加入频道前开始 Last-mile 网络探测
lpAgoraEngine->startLastmileProbeTest(config);

// (2)也可以选择在其他时候结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
lpAgoraEngine->stopLastmileProbeTest();
```



### API 参考
* [`startLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)
* [`stopLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593)
* [`onLastmileQuality`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)
* [`onLastmileProbeResult`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5)

## 开发注意事项

- Last-mile 测试必须在加入通话频道之前。在结束测试之前，Agora 不建议调用其他 API 方法。
- `onLastmileQuality` 回调第一次报告的结果有一定概率是 unknown, 可通过之后的几次回调获得结果。
