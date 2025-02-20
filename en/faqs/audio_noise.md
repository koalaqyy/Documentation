
---
title: Noise occurs in a call.
description: 
platform: All Platforms
updatedAt: Mon Jul 01 2019 15:09:54 GMT+0800 (CST)
---
# Noise occurs in a call.
Noise may be caused by the physical environment or recording and playback devices rather than the SDK.

## Step 1: Self-check

Check the following:
* Ensure that all users are in separated physical environments (no near-field tests).
* Check if an external audio source is used and if the captured external audio source is normal. In this case, noise cancellation is not supported and the noise may be caused by data loss during the audio transmission to the SDK.
* Check if any user is talking in a noisy environment. 
* Check if the recording device is working properly. For example, check whether the headset is plugged in correctly, use another headset, or switch to another audio route.

## Step 2: Contact Agora Customer Support

If the issue persists, contact Agora customer support and submit the issue with the following information:

<table>
  <tr>
    <th>Information</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Mandatory</td>
    <td><li>The name of the channel where the noise occurs.</li><li>The uid of the user who causes the noise.</li><li>The recording files, if available.</li></td>
  </tr>
  <tr>
    <td>Additional</td>
    <td><li>The time frame during which the users hear the noise.<</li><li>If the issue remains after rejoining the channel.</li><li>If the device test result is normal on macOS or Windows.</li><li>If the noise is consistent or only when you speak, and disappears when the remote user speaks.</li></td>
  </tr>
</table>

## Step 3: Monitor the Quality of Experience in Agora Analytics in Dashboard

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Dashboard. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).
