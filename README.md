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

Install Serial interfase
```
sudo nano /media/$USER/boot/config.txt
```
At the last line insert:
```
enable_uart=1
```

* Copy rootfs folder into rootfs partition

You cant use Nautilus. The rootfs only can be write by root
Do the following commands

```
sudo -i
cd /media/$USER/rootfs/lib/modules
cp -fr /home/$USER/sim7600/rootfs/lib/modules/* .
``` 


### Installing Qualcomm QMI interface 

Configure Locales
```
Edit /etc/locale.gen and uncomment the line with en_US.UTF-8
```
Run
```
sudo locale-gen en_US.UTF-8
update-locale en_US.UTF-8
```

Installing qmicli

```
sudo apt-get update -y
sudo apt install libgtk-3-dev libgtop2-dev librsvg2-dev -y
sudo apt-get install gudev-1.0
wget http://www.freedesktop.org/software/libqmi/libqmi-1.24.2.tar.xz
tar -vxf libqmi-1.24.2.tar.xz
cd libqmi-1.24.2/
./configure --prefix=/usr --disable-static
make
sudo make install
```
Verify that you have the qmi_wwan driver
```
lsusb -t
```
Can look like this
```
Port 4: Dev 4, If 5, Class=Vendor Specific Class, Driver=qmi_wwan, 480M
```

Connect via  Serial using an UART-USB
```
sudo screen /dev/cu.usbserial-A506LNW8 115200
```

Test qmicli
```
sudo qmicli  --nas-get-signal-strength  -d /dev/cdc-wdm0
```

It will reply
```
BIM QMUX support available
[/dev/cdc-wdm0] Successfully got signal strength
Current:
        Network 'lte': '-61 dBm'
RSSI:
        Network 'lte': '-61 dBm'
ECIO:
        Network 'lte': '-2.5 dBm'
IO: '-106 dBm'
SINR (8): '9.0 dB'
RSRQ:
        Network 'lte': '-16 dB'
SNR:
        Network 'lte': '3.2 dB'
RSRP:
        Network 'lte': '-90 dBm'
```

Request module manufacturer:

```
sudo qmicli --device=/dev/cdc-wdm0 --dms-get-manufacturer
```

Response
```
[/dev/cdc-wdm0] Device manufacturer retrieved:
        Manufacturer: 'QUALCOMM INCORPORATED'
```

Sim Card Status:
```
sudo qmicli --device=/dev/cdc-wdm0 --uim-get-card-status
```

