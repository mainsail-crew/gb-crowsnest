# Sample config

During the installation of Crowsnest a sample configuration named `crowsnest.conf` was already placed in Klipper's configuration folder. In Mainsail you can edit the file in the config section.

Sample config:

```ini
[crowsnest]
log_path: ~/klipper_logs/crowsnest.log
log_level: quiet

[cam 1]
mode: mjpg
port: 8080
device: /dev/video0
resolution: 640x480
max_fps: 15
```
