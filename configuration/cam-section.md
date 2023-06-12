# \[cam] section

You have to create a \[cam] section per camera and name your cameras by placing the name inside the square brackets.

```
[cam raspi]
mode: camera-streamer
...

[cam logiC270]
mode: ustreamer
...
```

{% hint style="danger" %}
The word `cam` must be set!

&#x20;This is a keyword to determine that this section belongs to a camera setup.
{% endhint %}

## **mode**

The streaming mode used by Crowsnest.\
Default: `mode: ustreamer`\
Available options:

<details>

<summary>ustreamer</summary>

This mode uses the mjpg protocol and streams with ustreamer. It's basically a series of jpeg images. This mode uses a lot of bandwidth depending on the resolution and frame rate set.

</details>

<details>

<summary>camera-streamer</summary>

This mode uses camera-streamer as backend.

camera-streamer is only available on Raspberry Pi's currently, more SBC's will follow.\
The greatest advantage of camera-streamer is, it uses the inbuilt GPU of the Pi SBC\
to deliver hardware encoded h.264 as format. This allows you to stream your video feed in webrtc, which has the advantage of using less bandwith without loosing quality and/or framerates/resolution.\
It also provides simultaniously stream of rtsp (if enabled through \`enable\_rtsp: true\`),\
mjpg and snapshots.

</details>

## enable\_rtsp

If \`mode: camera-streamer\` is used, this will enable the RTSP Server of camera-streamer.

{% hint style="info" %}
**The stream's URL becomes `rtsp://<printer-ip-or-name>:<rtsp_port>`**/stream.h264\
Example: `rtsp://mainsailos.local:8554`/stream.h264
{% endhint %}

## rtsp\_port

```properties
rtsp_port: 8554
```

This setting allows you to set a port for the rtsp server. Only available with \`mode: camera-streamer\`

## **port**

The network port through which the camera is accessible. This setting only affects the mjpg mode (ustreamer).\
Default: `port: 8080`

**Note:** In MainsailOS, by default four webcam ports are mapped to URLs. Notice that the first URL doesn't contain a number. You can simply add these to the Mainsail settings.

| Port | Stream URL              | Snapshot URL              |
| ---- | ----------------------- | ------------------------- |
| 8080 | /webcam/?action=stream  | /webcam/?action=snapshot  |
| 8081 | /webcam2/?action=stream | /webcam2/?action=snapshot |
| 8082 | /webcam3/?action=stream | /webcam3/?action=snapshot |
| 8083 | /webcam4/?action=stream | /webcam4/?action=snapshot |

#### A small note on WebRTC

In order to use `WebRTC`, please replace `?action=stream` with `webrtc`. Keep in mind this will only work if you use `mode: camera-streamer`.

{% hint style="warning" %}
Don't change the `URL Snapshot`! This will be the same as before.
{% endhint %}

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Webcam settings for WebRTC (camera-streamer)</p></figcaption></figure>

## **device**

The path of the video device (camera) to be used by the configured streaming service. All available devices are listed in the log file every time Crowsnest is started. You can copy the path from there.\
Default: `device: /dev/video0`

A path in this format is also valid:

```
device: /dev/v4l/by-id/usb-PixArt_Imaging_Inc._USB2.0_Camera-video-index0
```

{% hint style="info" %}
Always prefer full paths as shown above, especcially in Mutlicam setups using `/dev/video* will` lead to errors or unexpected behaviours.
{% endhint %}

## **resolution**

The desired streaming resolution, formatted as `height`x`width` without spaces.\
Default: `resolution: 640x480`

**Note:** This is case sensitive! Do not use capital 'X' !

{% hint style="info" %}
The resolution has a big impact on the bandwidth, especially in mjpg mode.
{% endhint %}

There are two practical ways to determine, which resolutions are supported by your device:

* from crowsnest.log

<details>

<summary>Example log file</summary>

```shell
[11/16/22 20:16:26] crowsnest: Supported Formats:
[11/16/22 20:16:26] crowsnest: 		[0]: 'MJPG' (Motion-JPEG, compressed)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 1920x1080
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 3840x2160
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 640x480
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 160x120
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 176x144
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 320x180
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 320x240
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:26] crowsnest: 		Size: Discrete 352x288
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:26] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 340x340
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 424x240
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 440x440
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 480x270
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 640x360
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 800x448
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 800x600
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 848x480
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 960x540
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1024x576
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1280x720
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1600x896
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 2560x1440
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 3840x3104
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 3264x2448
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 2592x1944
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 2048x1536
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1600x1200
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1024x768
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.017s (60.000 fps)
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		[1]: 'YUYV' (YUYV 4:2:2)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 1920x1080
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.200s (5.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 3840x2160
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 640x480
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 160x120
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 176x144
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 320x180
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 320x240
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 352x288
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 340x340
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 424x240
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:27] crowsnest: 		Size: Discrete 440x440
[11/16/22 20:16:27] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 480x270
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 640x360
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 800x448
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 800x600
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.100s (10.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 848x480
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.033s (30.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 960x540
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.100s (10.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 1024x576
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.100s (10.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 1280x720
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.100s (10.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 1600x896
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.200s (5.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 2560x1440
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 3840x3104
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 3264x2448
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 2592x1944
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 2048x1536
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 1600x1200
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 1.000s (1.000 fps)
[11/16/22 20:16:28] crowsnest: 		Size: Discrete 1024x768
[11/16/22 20:16:28] crowsnest: 		Interval: Discrete 0.100s (10.000 fps)
```

</details>

* using `dev-helper.sh` from `tools` directory

```bash
cd ~/crowsnest
tools/dev-helper.sh -c
```

<details>

<summary>Sample output</summary>

```bash
crowsnest - dev-helper.sh

v4l2-ctl supported camera(s):

Device /dev/video1:

Symbolic links to /dev/video1:

/dev/v4l/by-id/usb-XCG-220921-J_3DO_Nozzle_Camera_01.00.00-video-index0
/dev/v4l/by-path/platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.1:1.0-video-index0


Supported formats:

	[0]: 'MJPG' (Motion-JPEG, compressed)
		Size: Discrete 1920x1080
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 3840x2160
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 640x480
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 160x120
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 176x144
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 320x180
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 320x240
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 352x288
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 340x340
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 424x240
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 440x440
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 480x270
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 640x360
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 800x448
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 800x600
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 848x480
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 960x540
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 1024x576
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 1280x720
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 1600x896
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 2560x1440
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 3840x3104
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 3264x2448
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 2592x1944
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 2048x1536
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 1600x1200
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 1024x768
			Interval: Discrete 0.017s (60.000 fps)
			Interval: Discrete 0.033s (30.000 fps)
	[1]: 'YUYV' (YUYV 4:2:2)
		Size: Discrete 1920x1080
			Interval: Discrete 0.200s (5.000 fps)
		Size: Discrete 3840x2160
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 640x480
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 160x120
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 176x144
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 320x180
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 320x240
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 352x288
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 340x340
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 424x240
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 440x440
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 480x270
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 640x360
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 800x448
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 800x600
			Interval: Discrete 0.100s (10.000 fps)
		Size: Discrete 848x480
			Interval: Discrete 0.033s (30.000 fps)
		Size: Discrete 960x540
			Interval: Discrete 0.100s (10.000 fps)
		Size: Discrete 1024x576
			Interval: Discrete 0.100s (10.000 fps)
		Size: Discrete 1280x720
			Interval: Discrete 0.100s (10.000 fps)
		Size: Discrete 1600x896
			Interval: Discrete 0.200s (5.000 fps)
		Size: Discrete 2560x1440
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 3840x3104
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 3264x2448
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 2592x1944
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 2048x1536
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 1600x1200
			Interval: Discrete 1.000s (1.000 fps)
		Size: Discrete 1024x768
			Interval: Discrete 0.100s (10.000 fps)

Supported Controls:

                     brightness 0x00980900 (int)    : min=-64 max=64 step=1 default=-15 value=-15
                       contrast 0x00980901 (int)    : min=0 max=95 step=1 default=4 value=2
                     saturation 0x00980902 (int)    : min=0 max=100 step=1 default=70 value=70
                            hue 0x00980903 (int)    : min=-2000 max=2000 step=1 default=0 value=0
 white_balance_temperature_auto 0x0098090c (bool)   : default=1 value=1
                          gamma 0x00980910 (int)    : min=100 max=300 step=1 default=115 value=115
           power_line_frequency 0x00980918 (menu)   : min=0 max=2 default=2 value=2
				0: Disabled
				1: 50 Hz
				2: 60 Hz
      white_balance_temperature 0x0098091a (int)    : min=2800 max=6500 step=1 default=4600 value=4600 flags=inactive
                      sharpness 0x0098091b (int)    : min=1 max=7 step=1 default=1 value=2
         backlight_compensation 0x0098091c (int)    : min=0 max=1 step=1 default=0 value=0
                  exposure_auto 0x009a0901 (menu)   : min=0 max=3 default=3 value=3
				1: Manual Mode
				3: Aperture Priority Mode
              exposure_absolute 0x009a0902 (int)    : min=3 max=2047 step=1 default=166 value=166 flags=inactive
                 focus_absolute 0x009a090a (int)    : min=0 max=1023 step=1 default=0 value=634 flags=inactive
                     focus_auto 0x009a090c (bool)   : default=0 value=1

```



</details>

## **max\_fps**

The desired streaming FPS (frames per second).\
Default `max_fps: 15`

**Note:**

* It is necessary to research which FPS in combination with which resolution your camera supports. For detailed information run `v4l2-ctl --list-formats-ext` in your SSH session.
* The FPS has a big impact on the bandwidth, especially in mjpg mode.
* This setting is mostly ignored in rtsp mode.

## **custom\_flags**

This setting allows you to pass advanced parameters to ustreamer. It is possible to adjust image parameters such as brightness, contrast and saturation, or to flip the image. The passed parameters are appended to the already configured parameters. However, your camera needs to support these advanced parameters as well.\
Default: `custom_flags:`

**Note:** The flags are separated by a single space, not by a comma.

You need to take a closer look at the documentation of the [ustreamer project](https://github.com/pikvm/ustreamer). As a guide, we have placed [ustreamer's manpage](https://github.com/mainsail-crew/crowsnest/blob/master/ustreamer\_manpage.md) in [crowsnest repository](https://github.com/mainsail-crew/crowsnest).

{% hint style="info" %}
Please prefer ustreamer's project page. Our manpage may not accurate
{% endhint %}

## **v4l2ctl**

This option allows to pass parameters for the configured device to V4L2 "the driver". Depending on the camera model, it is possible to pass different parameters, but not all parameters are supported by every camera.\
Commented out by default.

**Note:** It is a very tough topic and is only recommended for advanced users.

{% hint style="warning" %}
All parameters have to be seperated by using a comma !
{% endhint %}

**Example:** `v4l2ctl: paramter1=value1,parameter2=value2,parameter3=....`

There are also two ways to determine what your device is capable of

* via crowsnest.log

<details>

<summary>Example log output:</summary>

```shell
[11/16/22 20:16:28] crowsnest: Supported Controls:
[11/16/22 20:16:28] crowsnest: 		brightness 0x00980900 (int) : min=-64 max=64 step=1 default=-15 value=-15
[11/16/22 20:16:28] crowsnest: 		contrast 0x00980901 (int) : min=0 max=95 step=1 default=4 value=2
[11/16/22 20:16:28] crowsnest: 		saturation 0x00980902 (int) : min=0 max=100 step=1 default=70 value=70
[11/16/22 20:16:28] crowsnest: 		hue 0x00980903 (int) : min=-2000 max=2000 step=1 default=0 value=0
[11/16/22 20:16:28] crowsnest: 		white_balance_temperature_auto 0x0098090c (bool) : default=1 value=1
[11/16/22 20:16:28] crowsnest: 		gamma 0x00980910 (int) : min=100 max=300 step=1 default=115 value=115
[11/16/22 20:16:28] crowsnest: 		power_line_frequency 0x00980918 (menu) : min=0 max=2 default=2 value=2
[11/16/22 20:16:28] crowsnest: 		0: Disabled
[11/16/22 20:16:28] crowsnest: 		1: 50 Hz
[11/16/22 20:16:28] crowsnest: 		2: 60 Hz
[11/16/22 20:16:28] crowsnest: 		white_balance_temperature 0x0098091a (int) : min=2800 max=6500 step=1 default=4600 value=4600 flags=inactive
[11/16/22 20:16:28] crowsnest: 		sharpness 0x0098091b (int) : min=1 max=7 step=1 default=1 value=2
[11/16/22 20:16:28] crowsnest: 		backlight_compensation 0x0098091c (int) : min=0 max=1 step=1 default=0 value=0
[11/16/22 20:16:28] crowsnest: 		exposure_auto 0x009a0901 (menu) : min=0 max=3 default=3 value=3
[11/16/22 20:16:28] crowsnest: 		1: Manual Mode
[11/16/22 20:16:28] crowsnest: 		3: Aperture Priority Mode
[11/16/22 20:16:28] crowsnest: 		exposure_absolute 0x009a0902 (int) : min=3 max=2047 step=1 default=166 value=166 flags=inactive
[11/16/22 20:16:28] crowsnest: 		focus_absolute 0x009a090a (int) : min=0 max=1023 step=1 default=0 value=634 flags=inactive
[11/16/22 20:16:28] crowsnest: 		focus_auto 0x009a090c (bool) : default=0 value=1
```

</details>

* or as earlier mentioned using `dev-helper.sh` script

## **Example of an application**

A Logitech C920 camera has autofocus activated by default. To get a nice picture, the manual focus should be activated. Until now, one had to use a cronjob that executes a script with v4l2-ctl commands. This was very inconvenient and a lot of set-up.

In Crowsnest you can simply specify these options in the config:

```
v4l2ctl: focus_auto=0,focus_absolute=30
```

Restart Crowsnest service via Mainsail and you're good to go.

Take a look at [alexz' crowsnest.conf](https://github.com/zellneralex/klipper\_config/blob/master/crowsnest.conf) file.
