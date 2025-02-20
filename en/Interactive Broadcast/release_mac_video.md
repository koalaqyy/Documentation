
---
title: Release Notes
description: 
platform: macOS
updatedAt: Thu Aug 22 2019 07:04:46 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for macOS.

## Overview

The Video SDK supports the following scenarios:

- Voice/Video Communication
- Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Video Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

#### Known Issues and Limitations

A USB device driver issue occurs when you do not hear any audio or the audio is corrupted with a USB headset. USB is not user-friendly on macOS, and we recommend using higher quality headsets.

## v2.9.0
v2.9.0 is released on Aug. 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
- [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)

For how to implement the new methods, see [Push Streams to the RTMP](../../en/Interactive%20Broadcast/push_stream_android2.0.md).

#### 2. Reporting the state of the remote video

This release extends the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with more states of the remote video: Stopped(0), Starting(1), Decoding(2), Frozen(3), and Failed(4). It adds a reason parameter to the callback to indicate why the remote video state changes. The original `remoteVideoStateChangedOfUid` callback is deleted. If you upgrade your Native SDK to the latest version, ensure that you re-implement the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback.

The new callback reports most of the remote video states, and therefore deprecates the following callbacks. You can still use them, but we do not recommend doing so.

- [`didVideoEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoEnabled:byUid:)
- [`didLocalVideoEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalVideoEnabled:byUid:)
- [`firstRemoteVideoDecodedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteVideoDecodedOfUid:size:elapsed:)

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannelByToken`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method to enable the audience in a Live Broadcast channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the [`leaveChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) and [`joinChannelByToken`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) methods. After the audience successfully switches to another channel by calling the [`switchChannelByToken`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method, the SDK triggers the [`didLeaveChannelWithStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLeaveChannelWithStats:) and [`didJoinChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinChannel:withUid:elapsed:) callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different Live Broadcast channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)

During the media stream relay, the SDK reports the states and events of the relay with the  [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:) and [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host Across Channels](../../en/Interactive%20Broadcast/media_relay_mac.md).

#### 3. Reporting the local and remote audio state

This release adds the [`localAudioStateChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) and [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: Stopped(0), Recording(1), Encoding(2), or Failed(3). When the state is Failed(3), see the `error` parameter for troubleshooting.
- The remote audio: Stopped(0), Starting(1), Decoding(2), Frozen(3), or Failed(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`localAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

#### 5. Pulling the remote audio data

To improve the experience in audio playback, this release adds the following methods to pull the remote audio data. After getting the audio data, you can process it and play it with the audio effects that you want.

- [`enableExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

The difference between the `onPlaybackAudioFrame` callback and the `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte` method is as follows:

- `onPlaybackAudioFrame`: The SDK sends the audio data to the app once every 10 ms. Any delay in processing the audio frames may result in an audio delay.
- `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte`: The app pulls the remote audio data. After setting the audio data parameters, the SDK adjusts the frame buffer and avoids problems caused by jitter in external audio playback.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the [`AgoraChannelStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html), [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html), and [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) classes:

- `AgoraChannelStats`: The total number of the sent audio bytes, sent video bytes,  received audio bytes, and received video bytes during a session.
- `AgoraRtcLocalVideoStats`: The encoding bitrate, the width and height of the encoding frame, the number of frames, and the codec type of the local video.
- `AgoraRtcRemoteVideoStats`: The packet loss rate of the remote video.

#### 2. Improving the live broadcast video quality

This release minimizes the video freeze rate under poor network conditions, improves the video sharpness, and optimizes the video smoothness when the packet loss rate is high.

#### 3. Improving the screen sharing quality

This release improves the sharpness of text during screen sharing in the Communication profile, particularly when the network condition is poor. Note that this improvement takes effect only when you set `contentHint` as Details(2).

#### 4. Other improvements

- Improves the audio quality when the audio scenario is set to `GameStreaming`.
- Improves the audio quality after the user disables the microphone in the Communication profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Crashes occur when testing the microphone.

#### Video

- Video freezes.

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming. 

**API changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added
- [`enableExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)
- [`localAudioStateChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:)
- [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:)
- [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:)
- [`localAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:)
- [`switchChannelByToken`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 
- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)
- [`AgoraChannelStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html): `txAudioBytes`, `txVideoBytes`, `rxAudioBytes` and `rxVideoBytes`
- [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html): `encodedBitrate`, `encodedFrameWidth`, `encodedFrameHeight`, `encodedFrameCount` and `codedType`
- [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html): `packetLossRate`

#### Deprecated

- [`didMicrophoneEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didMicrophoneEnabled:). Use AgoraAudioLocalStateStopped(0) or AgoraAudioLocalStateRecording(1) in the [`localAudioStateChange`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) callback instead.
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback instead.
- [`videoTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStats:) callback instead.
- [`didVideoEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoEnabled:byUid:). Use the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with the following parameters instead:
	- AgoraVideoRemoteStateStopped(0) and AgoraVideoRemoteStateReasonRemoteMuted(5).
	- AgoraVideoRemoteStateDecoding(2) and AgoraVideoRemoteStateReasonRemoteUnmuted(6).
- [`didLocalVideoEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalVideoEnabled:byUid:). Use the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with the following parameters instead:
	- AgoraVideoRemoteStateStopped(0) and AgoraVideoRemoteStateReasonRemoteMuted(5).
	- AgoraVideoRemoteStateDecoding(2) and AgoraVideoRemoteStateReasonRemoteUnmuted(6).
- [`firstRemoteVideoDecodedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteVideoDecodedOfUid:size:elapsed:). Use AgoraVideoRemoteStateStarting(1) or AgoraVideoRemoteStateDecoding(2) in the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback instead.

#### Deleted

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`
- `remoteVideoStateChangedOfUid`

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string usernames

Many apps use string usernames. This release adds the following methods to enable apps to join an Agora channel directly with string usernames as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. For details, see [Use String User Accounts](../../en/Interactive%20Broadcast/string_mac.md).

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio and video statistics

To monitor the audio and video transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) and [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) classes, to report the audio and video freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class.


**Improvements**

This release adds a `AgoraConnectionChangedKeepAliveTimeout(14)` member to the `AgoraConnectionChangedReason` parameter of the [connectionChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.


**Issues fixed**

#### Video

- Occasional deadlocks when calling the `MediaIO` methods.

#### Miscellaneous

- Occasional crashes.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class
- The `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class

#### Deprecated

- The `lowLatency` member in the [AgoraLiveTranscoding](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) class


## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

**Compatibility changes**

Ensure that you read the following SDK behavior changes if you migrate from an earlier SDK version.

#### 1. Publishing streams to the RTMP

To improve the usability of the RTMP streaming service, v2.4.1 defines the following parameter limits:

| Class **/** Interface  | Parameter Limit                                              |
| ---------------------- | ------------------------------------------------------------ |
| [AgoraLiveTranscoding](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html)        | <li>[videoFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoFramerate): Frame rate (fps) of the RTMP live output video stream. The default value is 15. We recommend not setting it to a value higher than 30.<li>[videoBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoBitrate): Bitrate (Kbps) of the RTMP live output video stream. The default value is 400. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.<li>[videoCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoCodecProfile): The video codec profile. Set it as **BASELINE**, **MAIN**, or **HIGH** (default). If you set this parameter to other values, Agora adjusts it to the default value of **HIGH**.<li>[size](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/size): Pixel of the video. The minimum value of size is **16 x 16**.</li> |
| [AgoraImage](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraImage.html)             | url: The maximum length of this parameter is **1024** bytes. |
| [addPublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)     | url: The maximum length of this parameter is **1024** bytes. |
| [removePublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) | url: The maximum length of this parameter is **1024** bytes. |

This release also adds the  [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) parameter in the `LiveTranscoding` class to set the audio codec profile type. The default type is LC-AAC, which means the low-complexity audio codec profile.

v2.4.1 also adds five error codes to the error parameter in the [streamPublishedWithUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) method for quick troubleshooting.

#### 2. Renaming the receivedFrameRate parameter in the RemoteVideoStats class

v2.4.1 renames the `receivedFrameRate` parameter to [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) in the [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class to more accurately describe the statistics of the remote video stream.

**New features**

#### 1. Adding media metadata

In live broadcast scenarios, the host can send shopping links, digital coupons, and online quizzes to the audience for more diversified live broadcast interactions. v2.4.1 adds the [setMediaMetadataSource](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) and the [setMediaMetadataDelegate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) interface and the [AgoraMediaMetadataDataSource](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) and the [AgoraMediaMetadataDelegate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html) protocol, allowing the host to add metadata to the output video and to send media attached information.

#### 2. Optimized screen sharing

To avoid image cropping and distortion in screen sharing, v2.4.1 optimizes the encoding algorithms. In this release Agora applies the following encoding algorithms:

Suppose the value of **dimensions** is 1920 x 1080 pixels, that is, 2073600 pixels:

- If the value of the screen `dimensions` is lower than that of the encoding dimensions, for example, 1000 x 1000 pixels, the SDK uses 1000 x 1000 pixels for encoding.
- If the value of the screen `dimensions` is higher than that of the encoding dimensions, for example, 2000 x 2000 pixels, the SDK uses the maximum value under 1920 x 1080 pixels with the aspect ratio of the screen dimension (1:1) for encoding, that is, 1440 x 1440 pixels.

Agora uses the [dimensions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/dimensions) value in the  [AgoraScreenCaptureParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html)  class to calculate the charges. If you do not set the value of **dimensions**, the SDK uses the default value of 1920 x 1080 to calculate the charges.

You can also choose whether or not to capture the mouse cursor when sharing the screen. v2.4.1 adds the  [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/captureMouseCursor) parameter in the `AgoraScreenCaptureParameters` class and captures the mouse by default.

#### 3. State of the local video

v2.4.1 adds the [localVideoStateChange](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback to indicate the local video state. In this callback, the SDK returns the `Stopped`,` Capturing`, `Encoding`, or `Failed` state. When the state is `Failed`, you can use the error code for troubleshooting. This callback indicates whether or not the interruption is caused by capturing or encoding. This release deprecates the `rtcEngineCameraDidReady` and `rtcEngineVideoDidStop` callbacks.

#### 4. State of the RTMP streaming

v2.4.1 adds the [rtmpStreamingChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) callback to indicate the state of the RTMP streaming and help you troubleshoot issues when exceptions occur. In this callback, the SDK returns the `Idle`, `Connecting`, `Runing`, `Recovering`, or `Failure` state. When the state is `Failure`, you can use the error code for troubleshooting. You can still use the `streamPublishedWithUrl` and `streamUnpublishedWithUrl` callbacks, but we do not recommend using them.

#### 5. More reasons for a network connection state change

In the onConnectionStateChanged callback, v2.4.1 adds error codes to the reason parameter to help you troubleshoot issues when exceptions occur. The SDK returns the [connectionChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback whenever the connection state changes. This release also deprecates `AgoraWarningCodeLookupChannelRejected(105)`, `AgoraErrorCodeTokenExpired(109)`, and `AgoraErrorCodeInvalidToken(110)`.

#### 6. State of the local network type 

v2.4.1 adds the [networkTypeChangedToType](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:) callback to indicate the local network type. In this callback, the SDK returns the `Unknown`, `Disconnected`, `Lan`, `Wifi`, `2G`, `3G`, or `4G` type. When the network connection is interrupted, this callback indicates whether or not the interruption is caused by a network type change or poor network conditions.

#### 7. Getting the audio mixing volume

v2.4.1 adds the  [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods, which respectively gets the audio mixing volume for local playback and remote playback, to help you troubleshoot audio volume related issues.

#### 8. Reporting when the first remote audio frame is received and decoded

To get the more accurate time of the first audio frame from a specified remote user, v2.4.1 adds the [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:) callback to report to the app that the SDK decodes first remote audio. This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the audio stream.
- The remote user stops sending the audio stream and re-sends it after 15 seconds.

The difference between the onFirstRemoteAudioDecoded and `onFirstRemoteAudioFrame` callbacks is that the `onFirstRemoteAudioFram`e callback occurs when the SDK receives the first audio packet. It occurs before the `onFirstRemoteAudioDecoded` callback.


**Improvements**

#### 1. Reporting more statistics

- v2.4.1 adds the [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) and  [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) parameters in the  [AgoraChannelStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html) class. These parameters return the packet loss rate from the local client to the server and vice versa.

- To provide more accurate statistics of the local and remote video, v2.4.1 makes the following changes to the following classes:
  - [AgoraRtcLocalVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html): Adds the  [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) parameters
  - [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html): Adds the [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) parameter, and renames the receivedFrameRate parameter to the [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) parameter

#### 2. Miscellaneous

- Improved the sound quality of the GameStreaming audio scenario.
- Reduced the audio and video latency.
- Reduced the SDK package size by 0.5 M.
- Improved the accuracy of the network quality after users change the video bitrate.
- Enabled the audio quality notification callback by default, that is, enabled the [remoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback without calling the `enableAudioVolumeIndication` method.
- Improved the stability of video services.
- Improved the stability of RTMP streaming.

**Issues fixed**

#### Video

- The user cannot switch between the screen sharing stream and the camera stream.

#### Miscellaneous

- Users still receive the `onNetworkQuality` callback after leaving the channel. 
- Occasional crashes. 


**API changes**

To improve your experience, we made the following changes to the APIs:

#### Unified the C++ interface for all platforms

v2.4.1 unifies the behavior of the C++ interfaces across different platforms so that you can apply the same code logic on different platforms. v2.4.1 implements the methods of the `RtcEngineParameters` class in the `IRtcEngine` class. Refer to [Agora C++ API Reference for All Platforms home page](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/index.html) for the applicable platforms and considerations of each interface.

#### Added

- [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPlayoutVolume)
- [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPublishVolume)
- [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:)
- [localVideoStateChange](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:)
- [networkTypeChangedToType](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:)
- [rtmpStreamingChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)
- [setMediaMetadataSource](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) 
- [setMediaMetadataDelegate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) 
-  [AgoraMediaMetadataSource](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) 
- [AgoraMediaMetadataDelegate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html)
- The [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) parameter in the `LiveTranscoding` class
- The  [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/captureMouseCursor) parameter in the `AgoraScreenCaptureParameters` class
- The [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) and [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) parameters in the `AgoraChannelStats` class
- The [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) parameters in the `AgoraRtcLocalVideoStats` class
- The [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) (to replace `receivedRemoteRate`) parameters in the `AgoraRtcRemoteVideoStats` class

#### Deprecated

- `enableAudioQualityIndication`
- `rtcEngineCameraDidReady`. Use  AgoraLocalVideoStreamStateCapturing(1) in the [localVideoStateChange](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.
- `rtcEngineVideoDidStop`. Use AgoraLocalVideoStreamStateStopped(0) in the [localVideoStateChange](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.
- The `AgoraWarningCodeLookupChannelRejected(105)` warning code. Use AgoraConnectionChangedRejectedByServer(10) in the [connectionChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeTokenExpired(109)` error code. Use AgoraConnectionChangedTokenExpired(9) in the [connectionChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeInvalidToken(110)` error code. Use  AgoraConnectionChangedInvalidToken(8) in the [connectionChangedToState](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeStartCamera(1003)` error code. Use AgoraLocalVideoStreamErrorCaptureFailure(4) in the [localVideoStateChange](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.

## v2.4.0

v2.4.0 is released on April 1, 2019.

**Compatibility changes**

If you integrate the SDK by using CocoaPods，ensure that you run `pod update` in your Terminal before `pod install`. If you prefer to specify the SDK version to obtain the latest release, ensure that you specify it as `'AgoraRtcEngine_macOS', '2.4.0.1'` in the Podfile.

**New features**

#### 1. Advanced screen sharing

v2.4.0 upgrades screen sharing and provides the following advanced functions:

- Shares the whole or part of a specified screen in a multi-display environment ([`startScreenCaptureByDisplayId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)).
- Shares the whole or part of a specified window ([`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)).
- Sets the content hint of the screen sharing to prioritize motion or detail ([`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)).
- Sets the dimensions, frame rate and bitrate for screen sharing ([`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)).

v2.4.0 deprecates the `startScreenCapture` method. We recommend using the new methods for screen sharing. With the new methods, developers need to design the code logic to obtain the `displayId` and `windowId`. For more information, see [Share the Screen](../../en/Interactive%20Broadcast/screensharing_mac.md).

#### 2. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:) methods, allowing you to change your voice or reverberation by choosing from the preset options. See Adjust the pitch and tone.

#### 3. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) methods. Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) method to track the position of a remote user.

#### 4. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:), [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest), and [`lastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

#### 5. Audio device loopback test

v2.4.0 adds the [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:) and [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest) methods for testing whether the local audio devices are working properly. The test involves only the local audio devices and does not report the network condition.

#### 6. Setting the priority of a remote user's stream

v2.4.0 adds the [`setRemoteUserPriority`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:) method for setting the priority of a remote user's media stream. You can use this method with the [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) method. If the fallback function is enabled for a remote stream, the SDK ensures the high-priority user gets the best possible stream quality.

#### 7. State of an audio mixing file 

v2.4.0 adds the [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

#### 8. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:) method for setting the log file size (KB).

**Improvements**

#### 1. Accuracy of call quality statistics

- v2.4.0 replaces the [`startEchoTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTest:) method with the [`startEchoTestWithInterval`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:) method. The `intervalInSeconds` parameter of `startEchoTestWithIntervals` allows you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html) class: [`sentTargetBitrate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetBitrate) for setting the target bitrate of the current encoder, [`sentTargetFrameRate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetFrameRate) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/qualityAdaptIndication) for reporting the quality of the local video since last count.

#### 2. Video encoder preferences

v2.4.0 provides the following options for setting video encoder preferences:

- Setting preferences under limited bandwidth. v2.4.0 adds two parameters to the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) class: [`minFrameRate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minFrameRate) and [`degradationPreference`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/degradationPreference). You can use these parameters together to set the minimum video encoder frame rate and the video encoding degradation preference under limited bandwidth. For more information, see [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_mac.md).
- Setting the camera capture preference. v2.4.0 adds the [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:) method, allowing you to set the camera capture preference. You can choose system performance over video quality or vice versa as needed. For more information, see the [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:).

#### 3. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability.
- Shortens the time to render the first remote video frame. 
- Improves the video smoothness and reduces the time delay when sharing a screen under poor network conditions. 
- Optimizes the usage of CPU and RAM resources.

**Issues fixed**

#### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- The SDK does not return YES after the `pushExternalAudioFrameSampleBuffer` method call succeeds.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.
- The app cannot get the virtual sound card information with the `getAudioPlaybackDevices` method.

#### Video

- If you skip the `renderMode` setting, the video stretches due to a mismatch with the display.
- Video freezes on some lower-end devices.
- It takes too long to render the first received video frame.
- The Electron SDK crashes if the virtual camera does not support 640 x 480.
- The cursor on the local screen is not accurately projected onto the remote screen.

#### Miscellaneous:

- The SEI information does not synchronize with the media stream when publishing transcoded streams to the RTMP.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:)
- [`startScreenCaptureByDisplayId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)
- [`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)
- [`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:)
- [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:)
- [`startEchoTestWithInterval`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)
- [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)
- [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)
- [`lastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

#### Deprecated

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

#### Miscellaneous

v2.4.0 changes the type of the [`frameRate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/frameRate) parameter in the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) class from `enum` to `int`.

## v2.3.3

v2.3.3 is released on January 24, 2019. 

**Improvements**

v2.3.3 optimizes the screen-sharing algorithm for different scenarios. The video smoothness and quality are enhanced when a user presents slides or browses websites. v2.3.3 also improves the initial image quality in the Communication profile.

**Issues fixed**

Occasional inaccurate statistics returned in the `networkQuality` callback.

## v2.3.2

v2.3.2 is released on January 16, 2019. 

**Compatibility changes**

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.

Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

**New features**

#### 1. Video quality in a live broadcast

v2.3.2 adds the [minBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minBitrate) parameter (minimum encoding bitrate) in the [setVideoEncoderConfiguration](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 2. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing.

This release also changes the behavior of the [adjustPlaybackSignalVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) method to control only the voice volume. Therefore, to mute the local audio playback, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods.

See [Adjust the Volume](../../en/Interactive%20Broadcast/volume_mac.md) for the scenarios and corresponding APIs.

#### 3. Fallback options for a live broadcast under unreliable network conditions

Unreliable network conditions affect the overall quality of a live broadcast. v2.3.2 adds the [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:) and [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) methods to allow the SDK to:

- Automatically disable the video stream when the network conditions cannot support both audio and video, or
- Enable the video when the network conditions improve. 

The SDK triggers the [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:) or [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:) callback when the stream falls back to audio-only or switches back to the video.

#### 4. Upstream and downstream statistics of each remote user/host

v2.3.2 adds the [`audioTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:) and [`videoTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:) callbacks to provide the upstream and downstream statistics of each remote user/host. During a call or live broadcast, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks return the user ID, received audio/video bitrate, packet loss rate, and network time delay (ms).

#### 5. New video encoder configuration

To support video rotation scenarios and improve the quality of the custom video source, v2.3.2 deprecates the [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:) method and replaces it with the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method to set the video encoder configurations. The [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. You can still use the `setVideoProfile` method, but we recommend using the `setVideoEncoderConfiguration` method to set the video profile.

#### 6. Virtual sound card

v2.3.2 adds the `deviceName` parameter in the [enableLoopbackRecording](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLoopbackRecording:deviceName:) method, allowing you to use a virtual sound card for audio recording:

- To use the current sound card, set `deviceName` as NULL.
- To use a virtual card, set `deviceName` as the name of the virtual card.

**Improvements**

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `audioQualityOfUid` callback and replaces it with the `remoteAudioStats` callback to improve the accuracy of the call quality statistics. The `remoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real-user experience. In addition, v2.3.2 optimizes the algorithm of the `networkQuality` callback for the uplink and downlink network qualities.

- [`remoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`networkQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`lastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Interactive%20Broadcast/in_call_statistics_macos.md).

#### 2. New network connection policy

v2.3.2 adds the following API method and callback to get the current network connection state and the reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState): Gets the connection state of the SDK.
- [`connectionChangedToState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) and [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `connectionChangedToState` callback when the network connection state changes. The SDK also triggers the `rtcEngineConnectionDidInterrupted` and `rtcEngineConnectionDidBanned` callbacks under certain circumstances, but we do not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. You can use this feedback for future product improvement. We strongly recommend integrating this method in your application.

#### 4. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Optimizes video capture methods on macOS and reduces performance loss.

**Issues fixed**

The following issues are fixed in v2.3.2:

#### SDK

- Crashes on macOS.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 

#### Video

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)
- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:)
- [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:)
- [`remoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)
- [`videoTransportStatsOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:)

#### Deprecated

- [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:)
- [`audioQualityOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

## v2.2.3

v2.2.3 is released on July 5, 2018. 

> The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

**Issues Fixed**

- Occasional online statistics crashes.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Occasional video freeze after a view size change.
- Failing to report the uid and volume of the speaker in a channel.

## v2.2.2

v2.2.2 is released on June 21, 2018.

**Issues fixed**

- Fixed occasional online statistics crashes.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of occasional video freeze after a view size change.

## v2.2.1

v2.2.1 is released on May 30th, 2018 and improves the internal code implementation.

## v2.2.0

v2.2.0 is released on May 4, 2018. 

**New features**

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method to enable the remote user in the channel to hear the audio effect played locally. 

> If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

We provide a proxy package for enterprise users with corporate firewalls to deploy before accessing our services. 

#### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Adds the watermark function for users to add a PNG file to the local or RTMP broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast. Adds the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to add watermarks in RTMP broadcasts. 

**Improvements**

#### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method improves its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes its detection from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every 2 seconds. 

#### 4. Audio Quality Enhancement

Improves the audio quality in scenarios that involve music playback.

**Issues fixed**

- Occasional crashes on the macOS device.
- Occasional screen display abnormalities when a large number of audience members join as the host in a live-broadcast channel.

## v2.1.3

v2.1.3 is released on April 19, 2018. 

In v2.1.3, we updated the bitrate values of the <code>setVideoProfile</code> method in the Live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

**Issues fixed**

- Block callbacks are occasionally not received if the delegate is not set.
- NSAssertionHandler appears in external links to the SDK.
- Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

**Improvements**

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to the normal communication or live-broadcast mode.

## v2.1.2

v2.1.2 is released on April 2, 2018. 

> If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth. 

**New features**

Extends the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

**Issues fixed**

The video resolution of the shared screen is worse in the Communication profile than in Live-broadcast profile.

## v2.1.1

v2.1.1 is released on March 16, 2018. 

We identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using the Agora SDK v2.1.

## v2.1.0

V2.1.0 is released on March 7, 2018. 

**New features**

#### 1. Voice optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhances the audio effect input from the built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Adds RESTful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

- Voice or video calls: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
- Interactive broadcasts: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).

#### 4. 17-way Video

Adds the support of 17-way video in interactive broadcasts, see:

- [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_mac.md)
- [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_iosmac.md)

#### 5. Video source customization

Supports the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer customization

Supports the default functions provided by the renderers to display the local and remote videos to meet your requirements. We provide a set of interfaces for customized renderers. 

#### 7. Screen sharing for interactive broadcast

- Before v2.1.0: The Agora SDK only supported the screen-sharing function in video calls
- From v2.1.0: The Agora SDK added the screen-sharing function in interactive broadcasts.

**Improvements**

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Video Freeze Rate</td>
<td>Reduces the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to voice-only mode. For example, 16 x 16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modifies a set of names for the API attributes and enumeration values.</td>
</tr>
</tbody>
</table>



## v2.0.2

v2.0.2 is released on December 15, 2017 and fixes the FFmpeg symbol conflict.

## v2.0 and Earlier

**v2.0**

v2.0 is released on December 6, 2017. 

#### New features

- Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the Communication profile to support dual streams.

- Updates the following callbacks for audio mixing and sound effects:

  <table>
  <colgroup>
  <col/>
  <col/>
  </colgroup>
  <thead>
  <tr><th>Name</th>
  <th>Description</th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
  <td>Removed. Replaced by rtcEngineLocalAudioMixingDidFinish.</td>
  </tr>
  <tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
  <td>Added. Notifies the app when the local audio effect playback stops.</td>
  </tr>
  <tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
  <td>Added. Notifies the app when the remote user starts audio mixing.</td>
  </tr>
  <tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
  <td>Added. Notifies the app when the remote user stops audio mixing.</td>
  </tr>
  </tbody>
  </table>

- Adds the camera management function in the Communication and Live-broadcast profiles by adding the following API methods:

  <table>
  <colgroup>
  <col/>
  <col/>
  </colgroup>
  <tbhead>
  <tr><th>Name</th>
  <th>Description</th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>isCameraZoomSupported</code></td>
  <td>Checks whether the device supports camera zoom.</td>
  </tr>
  <tr><td><code>isCameraTorchSupported</code></td>
  <td>Checks whether the device supports camera flash.</td>
  </tr>
  <tr><td><code>isCameraFocusPositionInReviewSupported</code></td>
  <td>Checks whether the device supports camera manual focus.</td>
  </tr>
  <tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
  <td>Checks whether the device supports camera auto-face focus.</td>
  </tr>
  <tr><td><code>setCameraZoomFactor</code></td>
  <td>Sets the camera zoom ratio.</td>
  </tr>
  <tr><td><code>setCameraFocusPositionInPreview</code></td>
  <td>Sets the position for manual focus and activates focusing.</td>
  </tr>
  <tr><td><code>setCameraTorchOn</code></td>
  <td>Sets the device to turn on the camera flash.</td>
  </tr>
  <tr><td><code>setCameraAutoFocusFaceModeEnabled</code></td>
  <td>Sets the device to start auto-face focusing.</td>
  </tr>
  </tbody>
  </table>



- Supports the external audio source in the Communication and Live-broadcast profiles by adding the following API methods:

  <table>
  <colgroup>
  <col/>
  <col/>
  </colgroup>
  <thead>
  <tr><th>Name</th>
  <th>Description</th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
  <td>Enables the external audio source.</td>
  </tr>
  <tr><td><code>disableExternalAudioSource</code></td>
  <td>Disables the external audio source.</td>
  </tr>
  <tr><td><code>pushExternalAudioFrameRawData</code></td>
  <td>Pushes the external audio frame to the Agora SDK.</td>
  </tr>
  </tbody>
  </table>



- Provides a set of RESTful APIs to ban a peer user from the server in the Communication and Live-broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function, if required.

#### Issues fixed

Audio routing and Bluetooth issues.

**v1.14**

v1.14 is released on October 20, 2017. 

#### New features

- Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
- Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.
- Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.

#### Improvements

- Optimizes the audio at high bitrates.
- Live Broadcast: The audience can view the host within one second in a single-stream mode (858 ms on average, and 625 ms in good network conditions).
- Adds the ability to reduce the bandwidth.
  - Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
  - Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
- Accurate control over the bitrate:
  - Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
  - Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.

**v1.13.1**

v1.13.1 is released on September 28, 2017 and optimizes the echo issue under certain circumstances.

**v1.13**

v1.13 is released on September 4, 2017. 

#### New features

- Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
- Adds the function to disable the audio playback.
- Adds the module map for the SDK, which means bridging header files are not necessary for Swift projects.
- Supports the profile configuration for stream-pushing on the client side.
- Adds the <code>didClientRoleChanged</code> callback to indicate a user role change between the host and audience in a live broadcast.
- Supports the push-stream failure callback on the server side.

#### Improvements:

- Screen Sharing: Enhances the video definition and fluency.
- Screen Sharing: Supports updating the captured region dynamically.
- The video profile is controllable by the software codec.

#### Issues fixed:

Occasional crashes on some devices.

**v1.12**

v1.12  is released on July 25, 2017. 

#### New functions:

- Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in the Live-broadcast profile.
- Adds the <code>aes-128-ecb</code> encryption mode in the `setEncryptionMode` method.
- Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
- Adds a set of API methods to manage the audio effect.
- Adds the <code>ActiveSpeaker</code> method to report on the active speaker in the current channel.
- Removes the <code>setScreenCaptureWindow</code> method, and updates the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the Communication profile.
- Adds displaying the mouse function when the screen-sharing function is enabled in the Communication profile.

#### Improvements:

In the Communication profile, the 320 &times; 180 resolution profile is improved.

- Keeps the video smooth under poor network and equipment conditions.
- Enhances the image quality to be better than 180p under good network and equipment conditions.

#### Issues fixed:

Occasional crashes.
