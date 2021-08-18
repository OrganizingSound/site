# Static IP for server
<!-- 
For nmcli:
```
sudo apt install network-manager -y
```
 -->

```
enp0s8:             
      dhcp4: no
      dhcp6: no
      addresses: [192.168.56.110/24, ]
      gateway4:  192.168.56.1
      nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
```

```
network:
    version: 2
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.56.110/24]
            gateway4: 192.168.56.1
            match:
                macaddress: b8:27:eb:dc:90:de
            set-name: eth0
            nameservers:
                addresses: [8.8.8.8, 8.8.4.4]
```

#

```
sudo apt install apache2 -y
```

