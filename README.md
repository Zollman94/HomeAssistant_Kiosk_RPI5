# HomeAssistant_Kiosk_RPI5
Guide how to set up kiosk using Raspberry PI5 and iiyama plt1700 touch screen monitor.

#Things you need:
Raspberry PI 5
Power brick 5v 3A
Touch Screen monitor iiyama plt1700
USB A to USB B cable
HDMI/DVI/VGA to MicroHDMI

Using official Raspberry Pi imager install latest `Raspberry PI OS Full (64-bit)` version with **desktop environment**.
Boot up RPI and plug monitor in. Open terminal and do update and upgrade

```console
sudo apt-get update
sudo apt-get upgrade
sudo apt install git
sudo apt install p7zip-full
```

Make sure RPI sees monitor
```console
dmesg | grep -i touch
```


Download drivers from official website : https://www.eeti.com/drivers_Linux.html
Must be for ARM processor!
If not available i saved latest release in this repo.

```console
git clone 
```
