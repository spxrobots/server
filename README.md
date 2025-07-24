# Server

Login details can be found (by staff) here: <https://github.com/spxrobots/server-secret>

## Setup

### Hardware and Operating System

Format external USB drive to NTFS, and name "spxrobots".

### Preparing the SD Card

Install "Raspberry Pi Imager" from https://www.raspberrypi.com/software/

Choose Device: "Raspberry Pi 4"

Choose Operating System: "Raspberry Pi OS (other) > Raspberry Pi OS Lite (64-bit)"
This will let you connect a keyboard, mouse and display via the micro-HDMI port.

Connect SD Card to computer and select it under "Choose Storage".

Click "Next", and then "Edit Settings".

Under General...
> Hostname: "spxrobots.local"
>
> Username: "spx-staff"
> Password: "\*\*\*\*\*\*\*\*"
>
> SSID: "Robots Bootstrap"
> Password: "\*\*\*\*\*\*\*\*"
> Country: "AU"
>
> Time zone: "Australia/Sydney"
> Keyboard: "us"

Under Services...
> Check "Use password authentication"

Under Options...
> Uncheck (disable) "Enable telemetry"

### Configuration and Initial Setup

#### 1. Public Internet Access

Share a Wi-Fi hotspot (from laptop or phone):
SSID (or rename your device temporarily): "Robots Bootstrap"
Password: "\*\*\*\*\*\*\*\*"

Power on the Pi. It will attempt to connect up to 3 times.

#### 2. System Upgrades

```sh
sudo apt update
sudo apt upgrade
sudo systemctl reboot
```

#### 3. External Drive

Add the following single line to /etc/fstab (by running sudoedit /etc/fstab)

```fstab
LABEL=spxrobots  /mnt/spxrobots  ntfs-3g  defaults,noatime,nofail,uid=1000,gid=1000,umask=0002  0  0
```

#### 5. Wi-Fi Access Point

```sh
sudo nmcli device wifi hotspot ifname wlan1 ssid "Robots" password "********"
sudo nmcli connection modify Hotspot connection.autoconnect yes
sudo nmcli connection modify Hotspot 802-11-wireless.ap-isolation 1
sudo nmcli connection modify Hotspot ipv4.addresses "10.37.0.1/16"
```

#### 6. UART / Serial Terminal Cable

https://forums.raspberrypi.com/viewtopic.php?t=307094
Add this line to the end of /boot/firmware/config.txt

```
enable_uart=1
```

And double check that /boot/firmware/cmdline.txt contains the following:
(if it doesn't, add it to the start with a space afterwards)

```
console=serial0,115200
```

#### 7. Bash Auto-Logout

Edit this file: sudo -e /etc/bash.bashrc
and add a line at the end:

```sh
export TMOUT=600
```

#### 8. User Accounts

```sh
sudo adduser --disabled-password --comment "" spx-student
sudo usermod --apend --groups spx-student spx-staff
```

## Installing Software

```sh
sudo apt install git
```
