---
description: >-
  Crowsnest is just a webcam streamer wrapper and uses different backends in the
  background. These will be explained here, and how to use them.
---

# Backends from Crowsnest

### µStreamer - pikvm

[µStreamer](https://github.com/pikvm/ustreamer) is a lightweight and very quick server to stream MJPEG video from any V4L2 devices from [PiKVM](https://github.com/pikvm/pikvm) and maintained by [Maxim Devaev](https://github.com/mdevaev).

* [Link to the GitHub Project](https://github.com/pikvm/ustreamer)

This Backend is active, when you use `mode: ustreamer` (legacy `mode: mjpg`) in your `crowsnest.conf`.

### camera-streamer - ayufan

[camera-streamer](https://github.com/ayufan/camera-streamer) is primarily focused on supporting a fully hardware-accelerated streaming of MJPEG streams and H264 video streams for minimal latency and maintained by [Kamil Trzciński](https://github.com/ayufan).

* [Link to the GitHub Project](https://github.com/ayufan/camera-streamer)

This Backend will be used, when you use `mode: camera-streamer` in your `crowsnest.conf`.

{% hint style="info" %}
This backend is only available on Raspberry Pi SBCs right now. Ayufan is currently working on supporting more SBCs.
{% endhint %}
