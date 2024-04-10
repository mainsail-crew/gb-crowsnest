# Installation

In order to install Crowsnest, it is required that `git` is already installed. Once this is done, please execute the commands below and follow the instructions carefully.
If you are installing Crowsnest on a device with a low amount of ram (<1GB), consider increasing swap space to hit a minimum of 1GB of memory before running the commands.

```bash
cd ~
git clone https://github.com/mainsail-crew/crowsnest.git
cd ~/crowsnest
sudo make install
```

After a successful installation, you should update your `moonraker.conf` as shown below to keep Crowsnest up to date. You will be asked during the installation if you want the script to do this for you.

```ini
[update_manager crowsnest]
type: git_repo
path: ~/crowsnest
origin: https://github.com/mainsail-crew/crowsnest.git
install_script: tools/pkglist.sh
```
