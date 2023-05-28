# Error 127 in line 31

## My log says...&#x20;

```
[11/19/22 17:06:08] crowsnest: ERROR: Error 127 occured on line 31
```

### What should I do?&#x20;

This error indicates that, in the build process of ustreamer something went wrong. Sometimes an update of ustreamer dependencies might also lead to that error message. That is an easy fix, you have to do.&#x20;

Simply copy & pasta the following commands:

```shell-session
sudo systemctl stop crowsnest.service
cd ~/crowsnest
make buildclean
make build
sudo systemctl start crowsnest.service
```

This should fix your error.
