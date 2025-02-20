
---
title: 设置音频属性
description: How to set audio profile for iOS
platform: iOS
updatedAt: Fri Aug 16 2019 08:12:15 GMT+0800 (CST)
---
# 设置音频属性
## 功能描述
 在一些比较专业的场景里，用户对声音的效果尤为敏感，比如语音电台，此时就需要对双声道和高音质的支持。
 所谓的高音质指的是我们提供采样率为 48 Khz、码率 192 Kbps 的能力，帮助用户实现高逼真的音乐场景，这种能力在语音电台、唱歌比赛类直播场景中应用较多。

本文指导开发者根据对音质、声道、场景等的不同需求，选择不同的音频属性，获得最佳实时互动效果。
## 实现方法
Agora SDK 提供 [`setAudioProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) 方法给开发者根据场景需求灵活配置适合的音质属性。这个方法有 2 个参数：
<table>
<tr>
<th>参数</th>
<th>描述</th>
</tr>
<tr>
<td>profile</td>
<td>代表不同的音频参数配置（音质），比如采样率、码率和编码模式等：
	<li>AgoraAudioProfileDefault(0)：默认设置。通信模式下为 AgoraAudioProfileSpeechStandard(1)，直播模式下为 AgoraAudioProfileMusicStandard(2)</li>
	<li>AgoraAudioProfileSpeechStandard(1)：指定 32 kHz 的采样率，语音编码，单声道，编码码率最大值为 18 kbps</li>
	<li>AgoraAudioProfileMusicStandard(2)：指定 48 kHz 的采样率，音乐编码，单声道，编码码率最大值为 48 kbps</li>
	<li>AgoraAudioProfileMusicStandardStereo(3)：指定 48 kHz 的采样率，音乐编码，双声道，编码码率最大值为 56 kbps</li>
	<li>AgoraAudioProfileMusicHighQuality(4)：指定 48 kHz 的采样率，音乐编码，单声道，编码码率最大值为 128 kbps</li>
	<li>AgoraAudioProfileMusicHighQualityStereo(5)：指定 48 kHz 的采样率，音乐编码，双声道，编码码率最大值为 192 kbps</li>
	</td>
</tr>
<tr>
<td>scenario</td>
<td>设置音频的使用场景，如娱乐、教学和游戏直播等。声音的流畅度、噪声抑制、音质等会根据不同的场景做出优化:
 <li>AgoraAudioScenarioDefault(0)：默认的音频应用场景</li>
 <li>AgoraAudioScenarioChatRoomEntertainment(1)：娱乐应用，适用于需要频繁上下麦的场景</li>
 <li>AgoraAudioScenarioEducation(2)：教育场景</li>
 <li>AgoraAudioScenarioGameStreaming(3)：游戏直播以及高音质互动应用，音质优先，适用于聊天房唱歌表演的场景</li>
 <li>AgoraAudioScenarioShowRoom(4)：秀场应用，连麦下音质仅次于 GameStreaming，拥有更好的专业外设支持</li>
 <li>AgoraAudioScenarioChatRoomGaming(5)：游戏应用，适用于游戏开黑场景，如吃鸡</li>
	</td>
</tr>
</table>

### 参数搭配
你可以参考下图，根据应用场景对音质、设备的不同需求，选择不同的参数搭配。
<table>
<tr>
<th>参数</th>
<th>场景需求</th>
<th>选项</th>
</tr>
<tr>
<td rowspan="3">profile</td>
<td>高音质</td>
<td><li>MusicHighQuality</li>
	<li>MusicHighQualityStereo</li></td>
</tr>
	<tr>
<!--<td></td>-->
<td>有一定音质要求</td>
<td><li>MusicStandard</li>
	<li>MusicStandardStereo</li></td>
</tr>
		<tr>
<!--<td></td>-->
<td>对音质无要求</td>
<td><li>Default</li>
	<li>SpeechStandard</li></td>
</tr>
<tr>
<td rowspan="5">scenario</td>
	<td>音质/音效优先</td>
<td>GameStreaming</td>
	</tr>
		<tr>
<!--<td></td>-->
	<td>频繁上下麦</td>
<td>ChatRoomEntertainment</td>
</tr>
	<tr>
<!--<td></td>-->
	<td>支持外放设备</td>
<td>ShowRoom</td>
</tr>
		<tr>
<!--<td></td>-->
	<td>开黑少杂音</td>
<td>ChatRoomGaming</td>
</tr>
		<tr>
<!--<td></td>-->
	<td>传输质量稳定</td>
<td>Default</td>
</tr>
</table>

也可以根据下表中提供的场景，直接选择参数进行搭配。

| 场景 | profile | scenario | 特性 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| 1 v 1 小班课 | Default | Default | 传输流畅、音质高清，优先保证通话质量 |
| 游戏开黑 | SpeechStandard | ChatRoomGaming | 只保留语音，非语音部分（键盘声、外放音乐等）不会被传输。节省码率的同时减少杂音，适合多人团战 |
| 剧本杀 | MusicStandard | ChatRoomEntertainment | 优秀的音乐编解码技术，提供更为丰富的声音呈现。上下麦没有音量、音质变化 |
| KTV 练歌房 | MusicHighQuality | GameStreaming | 高音质搭配丰富音效，适用于对音质要求高的场景 |
| 语音电台 | MusicHighQualityStereo | ShowRoom | 高音质，立体声，支持专业外放设备 |
| 音乐教学 | MusicStandardStereo | GameStreaming | 优先保证音质。适用于外放音效也能直播出去的场景 |
| 双师课堂 | MusicStandardStereo | ChatRoomEntertainment | 保证音质的同时，呈现更丰富的声音效果。上下麦没有音量、音质变化 |

### 示例代码

```swift
// swift
// 开黑聊天室
agoraKit.setAudioProfile(.speechStandard, scenario: .chatRoomGaming)
// 娱乐聊天室
agoraKit.setAudioProfile(.musicStandard, scenario: .chatRoomEntertainment)
// K 歌房
agoraKit.setAudioProfile(.musicHighQuality, scenario: .chatRoomEntertainment)
// FM 超高音质
agoraKit.setAudioProfile(.musicHighQuality, scenario: .gameStreaming)
```

```objective-c
// objective-c
// 开黑聊天室
[agoraKit setAudioProfile: AgoraAudioProfileSpeechStandard scenario: AgoraAudioScenarioChatRoomGaming];
// 娱乐聊天室
[agoraKit setAudioProfile: AgoraAudioProfilemusicStandard, scenario: AgoraAudioScenarioChatRoomEntertainment];
// K 歌房
[agoraKit setAudioProfile: AgoraAudioProfileMusicHighQuality, scenario: AgoraAudioScenarioChatRoomEntertainment];
// FM 超高音质
[agoraKit setAudioProfile: AgoraAudioProfilemusicHighQuality, scenario: AgoraAudioScenarioGameStreaming]
```

### API 参考

- [`setAudioProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:)

## 开发注意事项
不同的 Audio scenario 下，设备的系统音量是不同的。

系统音量分通话音量和媒体音量两种：
- 通话音量指的是进行语音、视频通话时的音量。通话音量有较好的回声消除。
- 媒体音量指的是播放音乐、视频或游戏的音效、背景音的音量。媒体音量有较好的声音表现力。

两者的差异在于媒体音量可以调整到 0，而通话音量不可以。

SDK 在 [`setAudioProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) 中提供 6 种不同的 Audio scenario，其中不同的 Audio scenario 使用的音量不同。如果需要将音量调整到 0，建议使用媒体音量控制的 Audio scenario。
<table>
<tr>
<th> Audio scenario</th>
<th>音量</th>
</tr>
<tr>
<td>GameStreaming</td>
<td>媒体音量</td>
</tr>
	<tr>
<td>Default</td>
<td  rowspan="3">	<li>通信模式下，所有用户使用通话音量</li>
	<li>直播模式下，主播及连麦主播使用通话音量，观众使用媒体音量</li></td>
</tr>
		<tr>
<td>Education</td>
<!--<td></td>-->
</tr>
<tr>
<td>ShowRoom</td>
<!--<td></td>-->
	</tr>
		<tr>
<td>ChatRoomEntertainment</td>
	<td>通话音量</td>
</tr>
	<tr>
<td>ChatRoomGaming</td>
	<td>通话音量</td>
</tr>
</table>
