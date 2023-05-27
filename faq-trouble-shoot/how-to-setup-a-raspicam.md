# How to setup a Raspicam?

## What do I have to do?

First of all, this procedure is tested with Raspicam v1.3, v2.1 and v3.\
But should work in the same manner with Arducams (not tested yet).

For this example I used the v3 model.

### Step 1: Get your device path

Open `crowsnest.log` and look for an entry like this

```
[05/25/23 19:12:07] crowsnest: Detected 'libcamera' device -> /base/soc/i2c0mux/i2c@1/imx708@1a
```

### Step 2: Set your device path

Open your crowsnest.conf in Mainsail and look for the `device:` entry.

Now set the device to the one, grabbed from your log.

```yaml
device: /base/soc/i2c0mux/i2c@1/imx708@1a
```

### Step 3: Change mode to camera-streamer

Now you have to use `camera-streamer`  as your stream service with

```yaml
mode: camera-streamer
```

After you finished these steps, please click on `SAVE & RESTART`.

Now your camera stream should show up.

## But my Raspicam doesn't show up!

Run `libcamera-hello --list-cameras` . This should show something similar to

```
Available cameras
-----------------
0 : imx708 [4608x2592] (/base/soc/i2c0mux/i2c@1/imx708@1a)
    Modes: 'SRGGB10_CSI2P' : 1536x864 [120.13 fps - (768, 432)/3072x1728 crop]
                             2304x1296 [56.03 fps - (0, 0)/4608x2592 crop]
                             4608x2592 [14.35 fps - (0, 0)/4608x2592 crop]
```

If you get an output other than that, check the wiring and connectors.

## Raspicam V1 configuration example

Here is a good `crowsnest.conf`example for a Raspicam v1.\
The whole explaination can be found in [this](https://github.com/mainsail-crew/crowsnest/issues/85#issuecomment-1561191087) post on github.

```yaml
[cam 1]
mode: camera-streamer                     # ustreamer - Provides mjpg and snapshots. (All devices)
                                          # camera-streamer - Provides webrtc, mjpg and snapshots. (rpi + Raspi OS based only)
enable_rtsp: false                        # If camera-streamer is used, this enables also usage of an rtsp server
rtsp_port: 8554                           # Set different ports for each device!
port: 8080                                # HTTP/MJPG Stream/Snapshot Port
device: /base/soc/i2c0mux/i2c@1/ov5647@36 # See Log for available ...
resolution: 1280x720                      # widthxheight format
max_fps: 15                               # If Hardware Supports this it will be forced, otherwise ignored/coerced.
#custom_flags:                            # You can run the Stream Services with custom flags.
#v4l2ctl:                                 # Add v4l2-ctl parameters to setup your camera, see Log what your cam is capable of.
```

Thanks to [ytugarev](https://github.com/ytugarev) for spending time on this example
