# Multi Instance Install

## How do I install crowsnest if I use Multi Instance?

{% hint style="info" %}
If you are not using Multi Instance, skip that part and continue with [Configuration](broken-reference)
{% endhint %}

Crowsnest was not designed with Multi Instance support in mind and never will. Maybe one day through [KIAUH](https://github.com/th33xitus/KIAUH).

But, to support that kinda, you could use `make config`

### Using `make config`, how?&#x20;

```shell-session
cd ~/crowsnest
make config
```

This launches a Wizard that let you set up your paths according to your Multi Instance Setup.shel

<figure><img src="../../.gitbook/assets/asset-make-config (1).png" alt=""><figcaption><p>Crowsnest Install Configurator</p></figcaption></figure>

Choose a path as a _Master_ path. This path will be the Instance where files are located, needed by Crowsnest, such as `crowsnest.conf`, `crowsnest.env` and `crowsnest.log` .

After successful generation of a configuration file (`toos/.config`) for the Installer please run

```shell-session
sudo make install
```

Follow the instructions which the Installer presents to you.

{% hint style="info" %}
Don't forget to reboot after installation!
{% endhint %}

In Mainsail this Instance is used to set up crowsnest and control the service via moonraker.
