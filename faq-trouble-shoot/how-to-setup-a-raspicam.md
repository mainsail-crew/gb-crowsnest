# How to setup a Raspicam?

## What do I have to do?

You are in luck, if you are using a Raspicam.

Simply do nothing. Crowsnest is designed with Raspicams in mind, so the Installer does all needed steps for you, including some "Workarounds" to prevent changing 'Raspicams' away from `/dev/video0`

That means, simply set in `crowsnest.conf`

```yaml
device: /dev/video0
```

## But my Raspicam does'nt show up!

Run `vcgencmd get_camera` . This should show something similar to

```
supported=1 detected=1, libcamera interfaces=0
```

If this shows

```
supported=1 detected=0
```

check your wiring, in most cases the ribbon cable is damaged or the cam itself.

{% hint style="info" %}
Raspicam devices that relie on libcamera interfaces are **not** supported at this point!

May change in the future.
{% endhint %}

