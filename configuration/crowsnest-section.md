# \[crowsnest] section

## **log\_path**

The path where Crowsnest stores the log file.\
Default: `log_path: ~/printer_data/logs/crowsnest.log`

{% hint style="warning" %}
This path will be set during Installation and should not be modified afterwards!
{% endhint %}

&#x20;:warning:_**Do not change after installation! This will prevent logrotate properly handling the log file rotation!**_ :warning:

## **log\_level**

Defines how much information should be written to the log file.\
Default: `log_level: verbose`

Available options:

<details>

<summary>quiet</summary>

Some basic information.

```
log_level: quiet
```

Example output:

```
[06/16/22 09:57:01] crowsnest: crowsnest - A webcam Service for multiple Cams and Stream Services.
[06/16/22 09:57:01] crowsnest: Version: v2.4.0-15-ge42799b
[06/16/22 09:57:01] crowsnest: Prepare Startup ...
[06/16/22 09:57:01] crowsnest: INFO: Checking Dependencys
[06/16/22 09:57:01] crowsnest: Dependency: 'crudini' found in /usr/bin/crudini.
[06/16/22 09:57:02] crowsnest: Dependency: 'find' found in /usr/bin/find.
[06/16/22 09:57:02] crowsnest: Dependency: 'logger' found in /usr/bin/logger.
[06/16/22 09:57:02] crowsnest: Dependency: 'xargs' found in /usr/bin/xargs.
[06/16/22 09:57:02] crowsnest: Dependency: 'ffmpeg' found in /usr/bin/ffmpeg.
[06/16/22 09:57:02] crowsnest: Dependency: 'ustreamer' found in bin/ustreamer/ustreamer.
[06/16/22 09:57:02] crowsnest: Dependency: 'rtsp-simple-server' found in bin/rtsp-simple-server/rtsp-simple-server.
[06/16/22 09:57:02] crowsnest: INFO: Detect available Devices
[06/16/22 09:57:02] crowsnest: INFO: Found 1 total available Device(s)
[06/16/22 09:57:02] crowsnest: Detected 'Raspicam' Device -> /dev/video0
[06/16/22 09:57:02] crowsnest: INFO: No usable CSI Devices found.
[06/16/22 09:57:02] crowsnest: V4L2 Control:
[06/16/22 09:57:02] crowsnest: No parameters set for [cam 1]. Skipped.
[06/16/22 09:57:02] crowsnest: Try to start configured Cams / Services...
[06/16/22 09:57:03] crowsnest: INFO: Configuration of Section [cam 1] looks good. Continue...
[06/16/22 09:57:03] crowsnest: Starting ustreamer with Device /dev/video0 ...
[06/16/22 09:57:05] crowsnest: ... Done!
```

</details>

<details>

<summary>verbose</summary>

For setup & troubleshooting.

```
log_level: verbose
```

It contains your existing crowsnest.conf and displays detailed information about the configured and connected cameras. It also gives information about their capabilities.

Turn it off when it is not needed.

Check out this [example](https://github.com/mainsail-crew/crowsnest/blob/master/log-example.md).

</details>

<details>

<summary>debug</summary>

For debugging only.

```
log_level: debug
```

In addition to the 'verbose' output, you also get information about the configured startup parameters (and default settings), plus it shows the output of the streamer you selected.

This option is useful for debugging purposes and should be turned off when it is not needed.

</details>

{% hint style="info" %}
After you set up your configuration to your needs, consider to set to `quiet`

This will minimize logging volume and may speed up starting times.
{% endhint %}

## **delete\_log**

Default: `delete_log: false`

If this option is set to true, the existing log file will be deleted with every restart of crowsnest.\
It could help to determine issues quicker, because the log will contain only informations about the current state since restart of crowsnest.

{% hint style="info" %}
This is useful if you ask for help in Discord Forums.

Please be always prepared to upload your logfile.
{% endhint %}

## no\_proxy

If this is set to `true`, it forces ustreamer to listen on all available network interfaces.

Useful if you want to use crowsnest in 'standalone' mode. Not recommended if you used MainsailOS or KIAUH to setup.
