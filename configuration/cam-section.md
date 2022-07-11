# \[cam] section

You have to create a \[cam] section per camera and name your cameras by placing the name inside the square brackets.

```
[cam raspi]
mode: mjpg
...

[cam logiC270]
mode: mjpg
...
```



## **mode**

The streaming mode used by Crowsnest.\
Default: `mode: mjpg`\
Available options:

<details>

<summary>mjpg</summary>

This mode uses the mjpg protocol and streams with ustreamer. It's basically a series of jpeg images. This mode uses a lot of bandwidth depending on the resolution and frame rate set.

</details>

<details>

<summary>rtsp</summary>

This mode uses the rtsp protocol, which uses h.264 as format. The advantage is the low bandwidth. The disadvantage is that it cannot be displayed in Mainsail. The stream can be watched in external programmes such as [VLC](https://www.videolan.org/).\
**The stream's URL becomes `rtsp://<printer-ip-or-name>:8554/<your-camera-section-name>`**\
Example: `rtsp://mainsailos.local:8554/raspi`

</details>

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



## **device**

The path of the video device (camera) to be used by the configured streaming service. All available devices are listed in the log file every time Crowsnest is started. You can copy the path from there.\
Default: `device: /dev/video0`

A path in this format is also valid:

```
device: /dev/v4l/by-id/usb-PixArt_Imaging_Inc._USB2.0_Camera-video-index0
```



## **resolution**

The desired streaming resolution, formatted as `height`x`width` without spaces.\
Default: `resolution: 640x480`

**Note:**

* It is necessary to research which resolution in combination with which FPS your camera supports. For detailed information run `v4l2-ctl --list-formats-ext` in your SSH session.
* The resolution has a big impact on the bandwidth, especially in mjpg mode.
* This setting is mostly ignored in rtsp mode.



## **max\_fps**

The desired streaming FPS (frames per second).\
Default `max_fps: 15`

**Note:**

* It is necessary to research which FPS in combination with which resolution your camera supports. For detailed information run `v4l2-ctl --list-formats-ext` in your SSH session.
* The FPS has a big impact on the bandwidth, especially in mjpg mode.



## **custom\_flags**

This setting allows you to pass advanced parameters to ustreamer. It is possible to adjust image parameters such as brightness, contrast and saturation, or to flip the image. The passed parameters are appended to the already configured parameters. However, your camera needs to support these advanced parameters as well.\
Default: `custom_flags:`

**Note:** The flags are separated by a single space, not by a comma.

You need to take a closer look at the documentation of the [ustreamer project](https://github.com/pikvm/ustreamer). As a guide, we have placed [ustreamer's manpage](https://github.com/lixxbox/crowsnest/blob/readme/ustreamer\_manpage.md) in this repo.



## **v4l2ctl**

This option allows to pass parameters for the configured device to V4L2 "the driver". Depending on the camera model, it is possible to pass different parameters, but not all parameters are supported by every camera.\
Default: `v4l2ctl:`

**Note:** It is a very tough topic and is only recommended for advanced users.

To determine which parameters your webcam supports, set `log_level: verbose` and restart Crowsnest. This will give you a list of available parameters in the log file. Alternatively, you can run `v4l2-ctl --list-....` in your SSH session.

<details>

<summary>Example log output:</summary>



</details>



## **Example of an application**

A Logitech C920 camera has autofocus activated by default. To get a nice picture, the manual focus should be activated. Until now, one had to use a cronjob that executes a script with v4l2-ctl commands. This was very inconvenient and a lot of set-up.

In Crowsnest you can simply specify these options in the config:

```
v4l2ctl: focus_auto=0,focus_absolute=30
```

Restart Crowsnest service via Mainsail and you're good to go.

Take a look at [alexz' crowsnest.conf](https://github.com/zellneralex/klipper\_config/blob/master/crowsnest.conf) file.
