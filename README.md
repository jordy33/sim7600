## Raspbian kernel 4.19.88-v7+ with drivers for SIM7600

### Tested with Raspbian Buster

How to install

* Clone this repository at home directory

```
cd ~
git clone https://github.com/jordy33/sim7600.git
```

* To install mount the sd card from raspbian in a PC with ubuntu

* Copy boot folder into boot partition
```
Using Nautilus replace all the contents from the boot directory into the SD card on the boot partition
```

* Copy rootfs folder into rootfs partition

You cant use Nautilus. The rootfs only can be write by root
Do the following commands

```
sudo -i
cd /media/$USER/rootfs/lib/modules
cp -fr /home/$USER/sim7600/rootfs/lib/modules/* .
``` 
