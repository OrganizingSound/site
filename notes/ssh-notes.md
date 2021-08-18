

# Server
sudo apt install ssh-client -y
sudo apt install ssh-server -y
ifconfig | grep "inet addr"

RPi:
```
sudo apt install ssh-client -y
#Seems this is already installed. If not, might need to be openssh-server
#sudo apt install ssh-server
ifconfig | grep "inet"
```

# Client
ssh username@ipaddress

# Set Up Security

In /etc/ssh/sshd_config on server:
```
PasswordAuthentication yes
```
to
```
PasswordAuthentication no
```

```
PermitRootLogin prohibit-password
```
to
```
PermitRootLogin no
```

Limit no. of concurrent connections:
```
#MaxStartups 10:30:100
```
to
```
MaxStartups 4
```

Then
```
cd ~
mkdir .ssh
cd .ssh
touch authorized_keys
```

You need to get the public key id_rsa.pub or whatever from potential clients ot the server.
In `authorized_keys`, for each client, add an entry:
```
# whatever metadata, client name, whatever you want
...publickeypaste...
```

Then
Find Linux version:
```
lsb_release -a
```
Restart server:
For Linux before 16.04:
```
sudo service ssh restart
```
OR for systemd based Ubuntu Linux 16.04 LTS or above server:
```
sudo systemctl restart ssh
```

Now from client, you do
```
ssh username@ipaddress
```
If you chose a passphrase when creating the keys, you'll need to type that in.

If a client doesn't have keys, do the whole generate keys thing on it first.
