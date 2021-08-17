# Linux/Unix Commands - Quick reference

List used ports on OS
```sh
$ netstat -an  | grep LISTEN
```

List disks and their available / free size
```sh
$ df -h
```

Get OS version
```sh
$ cat /etc/os-release
```

## Arch linux

**Prepare SD CARD for raspberry pi 2/3**:


List pacman packages
```sh
pacman -Q
```

Remove pacman packages
```sh
pacman -R <package name>
```

### Raspberry pi 
[SD card prep instructions](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3)

Config WIFI via terminal:
```sh
wifi-menu
# Select network and enter password
# Save profile with [profile_name]

# Enable profile at start up
netctl enable [profile_name]
```

If RPI does not boot without hdmi attached:
```
# Add hdmi_force_hotplug=1 to /boot/config.txt
```

If RPI does not get DNS server
```
# Set DNSSEC=false in /etc/systemdb/resolved.conf
systemctl restart systemd-resolved.service
```
