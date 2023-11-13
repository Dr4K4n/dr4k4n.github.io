# Installing Firefox via APT instead of snap on Ubuntu 22.04

> WARNING! You may want to do a backup of your firefox profile (see below)

## remove firefox snap
```sh
snap remove firefox
```

## Install firefox via APT
1. Add the ["Mozilla Team" PPA](https://launchpad.net/~mozillateam/+archive/ubuntu/ppa)
```sh
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update
```
3. Prevent ubuntu from installing firefox via snap when you try to install it via APT by pinning the snap version. Read more about pinning [here](https://help.ubuntu.com/community/PinningHowto).

```sh
nano /etc/apt/preferences.d/firefox
```

Fill it with the following content
```
Package: firefox
Pin: version 1:1snap1-0ubuntu2
Pin-Priority: -1
```
this will de-prioritize the snap version.

check which if your hack worked:
```sh
sudo apt-cache policy firefox
```

it should show something like this:
```
firefox:
  Installed: 115.0+build2-0ubuntu0.22.04.1~mt1
  Candidate: 115.0+build2-0ubuntu0.22.04.1~mt1
  Version table:
     1:1snap1-0ubuntu2 -1
        500 http://de.archive.ubuntu.com/ubuntu jammy/main amd64 Packages
     115.0+build2-0ubuntu0.22.04.1~mt1 500
        500 https://ppa.launchpadcontent.net/mozillateam/ppa/ubuntu jammy/main amd64 Packages
        100 /var/lib/dpkg/status
```

4. Install firefox via APT
```sh
sudo apt install firefox
```

5. Profit!!!

## Profile Backup

Snap Firefox profiles are saved in `~/snap/firefox/common/.mozilla/`
I've copied this over to `~/.mozilla/`. 
Sadly firefox wasn't picking up my old profile on first start, I needed to modify the `Default` in `~/.mozilla/firefox/profiles.ini`.
