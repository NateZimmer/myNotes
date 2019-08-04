# myNotes


# PI Tips


## Samba Server 

To access drive over windows native network share / SMB see, 

Install: 

```console
sudo apt-get install samba samba-common-bin
```

Edit config @: 

```console
sudo nano /etc/samba/smb.conf
```


See https://www.raspberrypi.org/forums/viewtopic.php?t=13590

Config homes to:

```

[homes]
   comment = Home Directories
   browseable = yes

# By default, the home directories are exported read-only. Change the
# next parameter to 'no' if you want to be able to write to them.
   read only = no

# File creation mask is set to 0700 for security reasons. If you want to
# create files with group=rw permissions, set next parameter to 0775.
   create mask = 0775

# Directory creation mask is set to 0700 for security reasons. If you want to
# create dirs. with group=rw permissions, set next parameter to 0775.
   directory mask = 0775

# By default, \\server\username shares can be connected to by anyone
# with access to the samba server.
# The following parameter makes sure that only "username" can connect
# to \\server\username
# This might need tweaking when using external authentication schemes
   valid users = %S

   path = /

```


Run : 

```console
sudo smbpasswd -a pi
```

Log in via :

```
//YourPiIP/pi/
```


## Install Docker: 

https://www.freecodecamp.org/news/the-easy-way-to-set-up-docker-on-a-raspberry-pi-7d24ced073ef/

```console
curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
```
