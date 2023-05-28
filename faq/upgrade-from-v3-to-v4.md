---
description: >-
  There is a huge different between Crowsnest v3 and v4. Unfortunately, it also
  means that there are major breaking changes, and you'll need to do some manual
  steps to upgrade.
---

# Upgrade from v3 to v4

### Step 1: Uninstall Crowsnest v3

At first, you have to uninstall the old version of Crowsnest. Please connect your printer/SBC via SSH and execute these commands:

```bash
cd ~/crowsnest
make uninstall
```

{% hint style="warning" %}
If you already updated Crowsnest to v4 via Webinterface, you have to change it back to v3 to uninstall it! Follow this commands:

```bash
cd ~/crowsnest
git checkout legacy/v3
make uninstall
```
{% endhint %}

And answer the following questions this way:

1. `Do you REALLY want to remove existing 'crowsnest'? (y/N)` > delete the `N` and type `y` for "Yes" and hit enter.
2. `do you want to remove crowsnest.conf` > answer with `n` for "No"
3. enter your password, if the uninstaller ask for it
4. Ignore the instrunction to remove the crowsnest folder and to remove the update section in the `moonraker.conf`.

### Step 2: Update Crowsnest

Just open Mainsail and click on the update button in the update manager on the Crowsnest entry.

{% hint style="warning" %}
If you already updated Crowsnest to v4 via Webinterface, but have to change back to v3 via terminal, you have to execute the following commands to switch back to the main branch of Crowsnest:

```bash
cd ~/crowsnest
git checkout master
```
{% endhint %}

### Step 3: Reinstall Crowsnest

Now you are able to reinstall all Crowsnest components/backends with:

```bash
cd ~/crowsnest
sudo make install
```

{% hint style="info" %}
This will need some time, because dependencies have to be installed and the streamer camera-streamer have to be compiled.
{% endhint %}

At the end of the installation you will be ask:

* `Do you want to add 'update manager' entry to your moonraker.conf?` > it  doesn't matter what you answer, because you have already the update\_manager section in your `moonraker.conf`. So this step will be skipped if you enter `y`.
* `Reboot NOW?` > enter `y` and press enter

### Step 4: Update moonraker.conf

Open your `moonraker.conf` in Mainsail and change this line:

* `install_script: tools/install.sh` to `install_script: tools/pkglist.sh`

{% hint style="info" %}
The complete update\_manager section should look like this:

```yaml
[update_manager crowsnest]
type: git_repo
path: ~/crowsnest
origin: https://github.com/mainsail-crew/crowsnest.git
install_script: tools/pkglist.sh
```
{% endhint %}

### Step 5: Modify crowsnest.conf

Crowsnest had backup your old `crowsnest.conf` to `crowsnest.conf.date-of-backup` and create a new `crowsnest.conf` with the new default values. If you changed anything in it before, please copy your old settings to the new `crowsnest.conf`.

{% hint style="info" %}
Pay attention, that the mode names are changed and `mjpg` is now `ustreamer.`
{% endhint %}

### Step 6: Switch to camera-streamer (optional)

{% hint style="warning" %}
This mode is only available on Raspberry Pi SBCs! If you use an other Host, skip this step!
{% endhint %}

Open your `crowsnest.conf` in Mainsail and change the mode of your webcam to `camera-streamer`. Hier is an example cam setting after the switch to `camera-streamer`:

```yaml
[cam 1]
mode: camera-streamer                   # ustreamer - Provides mjpg and snapshots. (All devices)
                                        # camera-streamer - Provides webrtc, mjpg and snapshots. (rpi + Raspi OS based only)
enable_rtsp: false                      # If camera-streamer is used, this enables also usage of an rtsp server
rtsp_port: 8554                         # Set different ports for each device!
port: 8080                              # HTTP/MJPG Stream/Snapshot Port
device: /dev/video0                     # See Log for available ...
resolution: 1280x720                    # widthxheight format
max_fps: 25                             # If Hardware Supports this it will be forced, otherwise ignored/coerced.
#custom_flags:                          # You can run the Stream Services with custom flags.
#v4l2ctl:                               # Add v4l2-ctl parameters to setup your camera, see Log what your cam is capable of.
```

Now click on `SAVE & RESTART`.

After the restart of Crowsnest, you have to change the Settings in Mainsail to WebRTC to use the full power of the new streamer. Open `Interface Settings` > `Webcams` > and click on the `pencil` to change the webcam settings. There you have to change following settings:

* **URL stream:** `/webcam/?action=stream` to `/webcam/webrtc`
* **Service:** to `WebRTC(camera-streamer)`

{% hint style="warning" %}
Don't change the `URL Snapshot`! This will be the same as before.
{% endhint %}

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Webcam settings for WebRTC (camera-streamer)</p></figcaption></figure>

### Step 7: Remove outdated settings in /boot/config.txt (optional)

{% hint style="info" %}
This is only necessary, if you use a Raspicam / CSI cameras, but its recommended to be up-to-date with all settings.
{% endhint %}

Open your `/boot/config.txt` with this command:

```bash
sudo nano /boot/config.txt
```

and search for the following lines:

* `camera_auto_detect=0`
* `start_x=1`

You have to disable both lines. To do this, just add a # in front of these lines. This should look like this then:

```yaml
## Disable libcamera (interferes with ustreamer, when using raspicams)
#camera_auto_detect=0

## Enable VideoCore at boot, needed for Crowsnest (Raspicams and DSI devices).
#start_x=1
```

After changing these lines, press `CTRL-s` to save and `CTRL-x` to close the editor. Then reboot your Pi to reload the settings with `sudo reboot`.

### Troubleshooting

* **Raspicam don't work any more:**\
  Please follow this [guide](../faq-trouble-shoot/how-to-setup-a-raspicam.md) to fix it.
