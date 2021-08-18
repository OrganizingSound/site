

# Install Ubuntu

Download Ubuntu from 
https://www.ubuntu.com/download/iot/raspberry-pi-2-3
```
Link:
Ubuntu Server image for Raspberry Pi 3
```

Insert SD card. Rename from Finder ("UBUNTU RPI"). Run
```
diskutil list
```
and look for your name.  You'll se an entry like:
```
/dev/disk4 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *32.0 GB    disk4
   1:             Windows_FAT_32 UBUNTU RPI              32.0 GB    disk4s1
```

```
diskutil unmountDisk /dev/disk4
```

prints:
```
Unmount of all volumes on disk4 was successful
```


You can now copy the image to the SD card, using the following command (sub your downloaded Ubuntu filename for <filename> and disk address for <diskaddress> below:

```
sudo sh -c 'xzcat ~/Downloads/<filename> | sudo dd of=<diskaddress> bs=32m'
```

For example:
```
sudo sh -c 'xzcat ~/Downloads/ubuntu-18.04.2-preinstalled-server-arm64+raspi3.img.xz | sudo dd of=/dev/disk4 bs=32m'
```

This part could take a while (in my test run it took 883 seconds).  You can see if it's working by running
```
iostat -d 1
```
which will print out transfer rates every second.  Under the column for your SD disk, you should see a value for MB/s if it's still transferring.

When it's done, you'll see a message like:

```
0+36963 records in
0+36963 records out
2422361088 bytes transferred in 883.967910 secs (2740327 bytes/sec)
```

Then run
```
diskutil eject <diskaddress>
```
to eject the SD drive.


# Boot Up RPi

Take the SD card out and put it in your RPi.

Plug a monitor into the HDMI port and a usb keyboard into a usb port. Plug the ethernet cable into the ethernet port to get an internet connection. When you login, use
```
ubuntu login: ubuntu
Password: ubuntu
```
You'll then have to type the password `ubuntu` in again in order to then change the password to a new one.  You'll now see
```
ubuntu@ubuntu:~$ 
```

Run 
```
sudo apt update
```
to update your packages.


# Remove snapd

```
sudo apt autoremove --purge snapd -y
```

# Remove automatic daily upgrades
```
sudo apt remove unattended-upgrades -y
```

Maybe [doesn't seem necessary]:
```
sudo systemctl stop apt-daily.timer
sudo systemctl disable apt-daily.timer
sudo systemctl disable apt-daily.service
sudo systemctl daemon-reload
```





PROBABLY DON'T USE THIS
# Wifi
Run
```
sudo apt install wireless-tools -y
```
to get `iwconfig` and other tools.

Run
```
iwconfig
```
to find out the name of your wireless card, which will probably be `wlan0`. It will start with `w`.

Run
```
sudo ifconfig <wifi-name> up
```
using your wifi name (possibly `wlan0`) for `<wifi-name>`. 

Make sure your local network is available by running
```
sudo iwlist <wifi-name> scan | grep ESSID
```
and looking for your router name.




WAS ALREADY THERE FOR ME:
```
sudo apt install wpasupplicant
```


```
network:
  version: 2
  renderer: networkd
  wifis:
    wlan0:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.0.21/24]
      gateway4: 192.168.0.1
      nameservers:
        addresses: [192.168.0.1, 8.8.8.8]
      access-points:
        "network_ssid_name":
          password: "**********"
```






We're going to use `ifupdown` to get our network started. First we need to install it:
```
sudo apt install ifupdown
```

Now we need to setup our network config. Type:
```
sudo nano /etc/network/interfaces
```
to open the config.

At the bottom of the file, add:
```
auto lo
iface lo inet loopback

auto <wifi-name>
iface <wifi-name> inet dhcp
wpa-ssid <your_router_name>
wpa-psk <your_router_wpa_password>
```
Then hit `ctrl-x`, type `y`, and hit `Enter`.

Use your wifi name (possibly `wlan0`) for `<wifi-name>`. Use the name of your router (the local network name from above) for `<your-router-name>` and the password for your router for `<your_router_wpa_password>`.

Then unplug your ethernet cable and run:
```
sudo reboot
```

When you get back to the login, use username `ubuntu` and whatever password you chose.




PROBABLY NOT
Now run:
```
sudo ifup -v <wifi-name>
```
again using your own wifi name.


