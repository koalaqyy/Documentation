
---
title: Push Streams to the CDN
description: 
platform: iOS,macOS
updatedAt: Mon Jun 10 2019 07:19:53 GMT+0800 (CST)
---
# Push Streams to the CDN
## Introduction

The process of publishing streams into the CDN (Content Delivery Network) is called CDN live streaming, where users can view the live broadcast through a web browser.

When multiple hosts are in the channel in the CDN live streaming, [transcoding](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#transcoding) is used to combine the streams of all the hosts into a single stream. Transcoding sets the audio/video profiles and the picture-in-picture layout for the stream to be pushed to the CDN.

The following figure shows a typical CDN-pushing scenario.

<img alt="../_images/live_ios_publishing_stream_en.png" src="https://web-cdn.agora.io/docs-files/en/live_ios_publishing_stream_en.png"/>

## Prerequisites
Ensure that you contact sales@agora.io to enable Agora's transcoding service before using this function.

## Implementation

Agora's CDN publishing solution is based on the following API methods to publish streams to the CDN, inject external video streams, transcode, and set the output layout.

-   [`setLiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:): Sets the live transcoding configuration.
-   [`addPublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:): Adds a stream to the CDN.
-   [`removePublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:): Removes a stream from the CDN.

In which:

-  The host calls the `setLiveTranscoding` method to set the transcoding parameters, for example, the canvas settings, after joining a channel. The host still needs to set a 16 &times; 16 view when only publishing an audio stream to CDN.
-  The host adds or removes a URL with the `addPublishStreamUrl` and `removePublishStreamUrl` methods after joining the channel.

### Sample Code

```objective-c
// CDN transcoding settings.
AgoraLiveTranscodingUser *user = [[AgoraLiveTranscodingUser alloc] init];
user.uid = 12345;
user.rect = CGRectMake(0, 0, 640, 720);
user.audioChannel = 0;

AgoraRtcEngineKit *rtcEngine = [AgoraRtcEngineKit sharedEngineWithAppId:@"" delegate:nil];
AgoraLiveTranscoding *transcoding = [[AgoraLiveTranscoding alloc] init];
transcoding.audioSampleRate = AgoraAudioSampleRateType44100;
transcoding.audioChannels = 2;
transcoding.size = CGSizeMake(640, 720);
transcoding.videoFramerate = 30;
transcoding.videoCodecProfile = AgoraVideoCodecProfileTypeHigh;
transcoding.transcodingUsers = @[user];

[rtcEngine setLiveTranscoding:transcoding];
```

```objective-c
// Adds a URL to which the host pushes a stream.
// Set the transcodingEnabled parameter as YES to enable the transcoding service. Once transcoding is enabled, you nee to set the live transcoding configurations by calling the setLiveTranscoding method. We do not recommend transcoding in the case of a single host.
[rtcEngine addPublishStreamUrl:streamUrl transcodingEnabled:YES];
```

```objective-c
// Removes a URL to which the host pushes a stream.
[rtcEngine removePublishStreamUrl:streamUrl];
```

### Adjusting the Picture-in-picture Layout

Use transcodingUser to adjust the picture-in-picture layout when a channel has multiple hosts.

> A host can set transcoding and the layout, for example, the canvas settings and multiple-host window settings, only after joining a channel.


**Example 1: Two-host Tile Horizontally**

To display the following layout:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/en/sei_2host.png" style="width: 420px;"/>


Set the parameters as follows:

```objective-c
Canvas:
     width: 640
     height: 360
     backgroundColor: #FFFFFF

User0:
      userId: 123
      x: 0
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0

User1:
      userId: 456
      x: 320
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0
```

**Example 2: Three-host Tile Vertically**

To display the following layout:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/en/sei_3host.png" style="width: 420px;"/>


Set the parameters as follows:

```objective-c
Canvas:
      width: 360
      height: 640
      backgroundColor: #FFFFFF

   User0:
      userId: 123
      x: 0
      y: 0
      width: 360
      height: 640
      zOrder: 1
      alpha: 1.0

   User1:
       userId: 456
       x: 0
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0

   User2:
       userId: 789
       x: 180
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0
```

**Example 3: One Full Screen + Floating Thumbnails**

To display the following layout:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/en/sei_random.png" style="width: 840px;"/>


Set the parameters as follows:

```objective-c
Canvas:
   width: 360
   height: 640
   backgroundColor: #FFFFFF

User0:
   userId: 123
   x: 0
   y: 0
   width: 360
   height: 640
   zOrder: 1
   alpha: 1.0

User1:
    userId: 456
    x: 45
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0

User2:
    userId: 789
    x: 185
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0
```

## Considerations

Ensure that you contact sales@agora.io to enable Agora's transcoding service before using this function.
