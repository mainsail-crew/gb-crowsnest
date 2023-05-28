# How to use identical USB Cam models twice?

If you want to use twice, the identical model of a USB cam, you have to use it's \`by-path\` path. In this example, a YGTek Webcam is used twice. Login to your Pi (or Linux machine) using ssh.

Type `ls /dev/v4l/by-id`.

```
pi@mainsailos:~ $ ls /dev/v4l/by-id/
usb-YGTek_Webcam_YG_U700_D.2021.0104.1403-video-index0
usb-YGTek_Webcam_YG_U700_D.2021.0104.1403-video-index1
```

As you can see there is only on device showing up. This is the only one that you can find in `crowsnest.log`.

Now please type `ls /dev/v4l/by-id`.

```
pi@mainsailos:~ $ ls /dev/v4l/by-path
platform-bcm2835-codec-video-index0
platform-bcm2835-isp-video-index0
platform-bcm2835-isp-video-index1
platform-bcm2835-isp-video-index2
platform-bcm2835-isp-video-index3
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.1:1.0-video-index0
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.1:1.0-video-index1
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.2:1.0-video-index0
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.2:1.0-video-index1
```

The output should look something like this. Now grab the two entrys with trailing `index0`.

In this example:

```
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.1:1.0-video-index0
platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.2:1.0-video-index0
```

Now open your crowsnest.conf and use these paths as `device:`.

```
[cam 1]
...
device: /dev/v4l/by-path/platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.1:1.0-video-index0
...

[cam 2]
...
device: /dev/v4l/by-path/platform-fd500000.pcie-pci-0000:01:00.0-usb-0:1.2:1.0-video-index0
...
```

When you are done, please click `SAVE & RESTART`. Now your camera streams should appear.
