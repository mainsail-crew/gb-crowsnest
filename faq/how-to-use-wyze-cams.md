---
description: >-
  WYZE-Cams are not supported by Crowsnest, but there exists a tool to create a
  WebRTC stream and this WebRTC stream can be used in Mainsail.
---

# How to use WYZE-Cams?

{% hint style="warning" %}
This tool is not part of Crowsnest, so we will not provide support for it! This is just a guide with all the informations we have about this tool to use it with Mainsail.\
\
Link: [github.com/mrlt8/docker-wyze-bridge](https://github.com/mrlt8/docker-wyze-bridge)
{% endhint %}

{% hint style="info" %}
Minimum requirement for embedding this streamer in Mainsail is version 2.8.0.
{% endhint %}

### Install WYZE-Bridge

WYZE-Bridge is a Docker container with all software parts you need, to convert a WYZE-Cam to WebRTC. There are multiple possibilities to install it.

* [docker-compose](https://github.com/mrlt8/docker-wyze-bridge#docker-compose-recommended)
* [Portainer ](https://github.com/mrlt8/docker-wyze-bridge/wiki/Portainer)
* [Home Assistant](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant)

{% hint style="info" %}
If you use a Raspberry PI for your docker host, you have to enable the hardware acceleration.\
\
Link: [https://github.com/mrlt8/docker-wyze-bridge/wiki/Hardware-Acceleration#raspberry-pi](https://github.com/mrlt8/docker-wyze-bridge/wiki/Hardware-Acceleration#raspberry-pi)
{% endhint %}

### Enable WebRTC in WYZE-Bridge

Per default, WebRTC is not enabled. To enable it, you only have to add some ports and add an ENV variable in your container config. Here you can find the guide to enable it: [https://github.com/mrlt8/docker-wyze-bridge#webrtc](https://github.com/mrlt8/docker-wyze-bridge#webrtc)

### First login in WYZE-Bridge

At first open the WYZE-Bridge web interface with the following URL:

* http://\<ip>:5000

When the web interface is loaded, you have to login with the same credentials as you use in the WYZE app. If all worked and your WYZE account is connected to your cam, you should see something like this:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Dashboard from WYZE-Bridge</p></figcaption></figure>

### Setup WYZE-Cam in Mainsail

Now, the WYZE bridge is ready to be set up, and you only have to configure the settings in Mainsail, and the setup is complete.

At first you have to grab the right WebRTC streaming URL. To get this, click on the `Streams` button below of the webcam and then on `WebRTC`. Then a new tab should be opened. Copy the URL of this tab. This is the your WebRTC-Stream URL.

Open Mainsail > Interface Settings (gears on the right top) > Webcams and then create a new webcam. Here you have to paste the URL in the `URL Stream` field and change the `Service` to `WebRTC (MediaMTX)`. This should look like that:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Webcam client settings for the WYZE Cam</p></figcaption></figure>
