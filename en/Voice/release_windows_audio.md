
---
title: Release Notes
description: 
platform: Windows
updatedAt: Mon Aug 19 2019 01:46:23 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice communication
- Live voice broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

The Windows Voice SDK supports the X86 and  X64 architecture.

## v2.9.0

v2.9.0 is released on Aug 16, 2019.

**Before getting started**

In this release, we deleted the following methods:

- `configPublisher`

If your app implements CDN streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab) method to enable the audience in a Live Broadcast channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the `leaveChannel` and `joinChannel` methods. After the audience successfully switches to another channel by calling the `switchChannel` method, the SDK triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different Live Broadcast channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)

During the media stream relay, the SDK reports the states and events of the relay with the [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22) and [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host across Channels](../../en/Voice/media_relay_windows.md).

#### 3. Reporting the local and remote audio state

This release adds the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) and [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: STOPPED(0), RECORDING(1), ENCODING(2), or FAILED(3). When the state is FAILED(3), see the `error` parameter for troubleshooting.
- The remote audio: STOPPED(0), STARTING(1), DECODING(2), FROZEN(3), or FAILED(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the `RtcStats` class:

- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): The total number of the sent audio bytes and  received audio bytes during a session.

#### 2. Other Improvements

- Improves the audio quality when the audio scenario is set to Game Streaming.
- Improves the audio quality after the user disables the microphone in the Communication profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Invalid call of the `muteRemoteAudioStream` method.
- Occasionally no audio. 

#### Miscellaneous

- Occasionally mixed streams in CDN streaming. 
- Occasional crashes occur.
- Failure to join the channel.

**API Changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added

- [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
- [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3)
- [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)
- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429)
- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): `txAudioBytes` and `rxAudioBytes`

#### Deprecated

- `onMicrophoneEnabled`. Use LOCAL_AUDIO_STREAM_STATE_CHANGED(0) or LOCAL_AUDIO_STREAM_STATE_RECORDING(1) in the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) callback instead. 
- `onRemoteAudioTransportStats`. Use the [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) callback instead.


#### Deleted

- `configPublisher`


## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string usernames

Many apps use string usernames. This release adds the following methods to enable apps to join an Agora channel directly with string usernames as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. For details, see [Use String User Accounts](../../en/Voice/string_windows.md).

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio statistics

To monitor the audio transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class.

**Improvements**

This release adds a `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` member to the `reason` parameter of the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

**API Changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [getUserInfoByUid](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [onLocalUserRegistered](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [onUserInfoUpdated](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class

#### Deprecated

- The `lowLatency` member in the [LiveTranscoding](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) class

## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

This is the first release of the Agora Voice SDK for Windows. Refer to the following guides to quickly integrate the SDK and enable real-time voice communication in your project.

- [Quick start](../../en/Voice/windows_video.md)
- [Use security keys](../../en/Voice/token.md)
- [Report in-call statistics](../../en/Voice/in_call_statistics_windows_audio.md)
- [Adjust the volume](../../en/Voice/volume_windows.md)
- [Play audio effects/audio mixing](../../en/Voice/effect_mixing_windows.md)
- [Set the voice changer and reverberation effects](../../en/Voice/voice_effect_windows.md)
- [Push Streams to the CDN](../../en/Voice/push_stream_windows2.0_audio.md)
- [Test or select a media device](../../en/Voice/switch_audio_device_windows.md)

If you migrate to this SDK from the Windows Video SDK, refer to the [Release notes for the Windows video SDK](../../en/Voice/release_windows_video.md) for audio improvements.
