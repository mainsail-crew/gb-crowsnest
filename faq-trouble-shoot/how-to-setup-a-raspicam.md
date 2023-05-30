# How to setup a Raspicam?

## Setup/Configure Raspicam

First of all, this procedure is tested with Raspicam v1.3, v2.1 and v3. But should work in the same manner with Arducams (not tested yet). For this example I used the v3 model.

### Step 1: Get your device path

Open `crowsnest.log` and look for an entry like this:

```
[05/25/23 19:12:07] crowsnest: Detected 'libcamera' device -> /base/soc/i2c0mux/i2c@1/imx708@1a
```

{% hint style="warning" %}
See the [Troubleshooting](how-to-setup-a-raspicam.md#troubleshooting) section if such an entry does not exist.
{% endhint %}

### Step 2: Set your device path

Open your `crowsnest.conf` in Mainsail and look for the `device:` entry. Now set the device to the one, grabbed from your log.

```yaml
device: /base/soc/i2c0mux/i2c@1/imx708@1a
```

### Step 3: Change mode to camera-streamer

Now you have to use `camera-streamer`  as your stream service with:

```yaml
mode: camera-streamer
```

After you finished these steps, please click on `SAVE & RESTART`. Now your camera stream should show up.

## Configuration example

Here is a good `crowsnest.conf`example for a Raspicam v1. The whole explaination can be found in [this](https://github.com/mainsail-crew/crowsnest/issues/85#issuecomment-1561191087) post on github.

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

## Troubleshooting

### Raspicam not detected

Login to your Pi (or Linux machine) and type:

```
libcamera-hello --list-cameras
```

If your output looks like this, your `/boot/config.txt` is probably misconfigured:

```
pi@mainsailos:~ $ libcamera-hello --list-cameras
No cameras available!
```

Run this command via terminal to open your `/boot/config.txt` with `nano` as editor:

```
sudo nano /boot/config.txt
```

Look for the following entries and change it:

* `start_x=1` this value is deprecated. If you have it in your `/boot/config.txt`, delete this line.
* `camera_auto_detect=0` this value is deprecated. If you have it in your `/boot/config.txt`, delete this line.
* Double-check if you have `camera_auto_detect=1` in your `/boot/config.txt`. If you don't have it in your config, please add it at the bottom of this file.
* Check if you have `dtoverlay=vc4-kms-v3d` or `dtoverlay=vc4-fkms-v3d` in your `/boot/config.txt`. If you don't have one of these options in your `/boot/config.txt` without a `#` in front of the line, please add `dtoverlay=vc4-kms-v3d` to your `/boot/config.txt`.

{% hint style="warning" %}
`start_x=1` and `dtoverlay=vc4-(f)kms-v3d` are also important für DSI displays. Please double-check the compatibility with your display.
{% endhint %}

Press `CTRL-s` to save and `CTRL-x` to close the nano editor. Then `reboot` the machine.

After the reboot, please reconnect your ssh client with your machine. Type `libcamera-hello --list-cameras` again. You should see something like this (a Raspicam V2 was used in this example):

```
pi@mainsailos:~ $ libcamera-hello --list-cameras
Available cameras
-----------------
0 : imx219 [3280x2464] (/base/soc/i2c0mux/i2c@1/imx219@10)
    Modes: 'SRGGB10_CSI2P' : 640x480 [103.33 fps - (1000, 752)/1280x960 crop]
                             1640x1232 [41.85 fps - (0, 0)/3280x2464 crop]
                             1920x1080 [47.57 fps - (680, 692)/1920x1080 crop]
                             3280x2464 [21.19 fps - (0, 0)/3280x2464 crop]
           'SRGGB8' : 640x480 [103.33 fps - (1000, 752)/1280x960 crop]
                      1640x1232 [41.85 fps - (0, 0)/3280x2464 crop]
                      1920x1080 [47.57 fps - (680, 692)/1920x1080 crop]
                      3280x2464 [21.19 fps - (0, 0)/3280x2464 crop]
```

If you get `No cameras available!` a second time, please check connections and the ribbon cable. Also avoid running the ribbon near stepper motors, stepper drivers and/or power supplies. They can interfere with the signal.



