---
description: >-
  Crowsnest v4 is not able to run on Debian Buster based systems. Debian Buster
  is "out of life" already and there are missing dependencies, but you can run
  the v3 legacy version on it without issues.
---

# Use legacy on Buster

### Step 1: Switch to legacy/v3 branch

Login your Host via SSH and use these commands to switch to legacy/v3 branch:

```bash
cd ~/crowsnest
git fetch
git checkout legacy/v3

sudo systemctl restart crowsnest
```

### Step 2: Update moonraker.conf

You have to inform moonraker, that you want to use this branch. Open your `moonraker.conf` via Mainsail and update your update\_manager section of Crowsnest to this:

```yaml
[update_manager crowsnest]
type: git_repo
path: ~/crowsnest
origin: https://github.com/mainsail-crew/crowsnest.git
primary_branch: legacy/v3
install_script: tools/install.sh
```
