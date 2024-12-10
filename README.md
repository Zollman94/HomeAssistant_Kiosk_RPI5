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
Extract and make shell script executable then run it as sudo. Follow instructions of installer.

```console
git clone https://github.com/Zollman94/HomeAssistant_Kiosk_RPI5.git
cd HomeAssistant_Kiosk_RPI5
7z x eGTouch_v2.5.13219.L-ma.7z
cd eGTouch_v2.5.13219.L-ma
sudo chmod +x setup.sh
sudo ./setup.sh
```

Now its good time to do reboot. After that you should be able to use touch screen by launching this service

```console
cd /usr/bin
sudo ./eGTouchD
```

If everything works now you can automate service to launch after system start.

```console
sudo nano /etc/systemd/system/egettouchd.service
```

```ini
[Unit]
Description=Start eGTouchD on boot
After=multi-user.target

[Service]
Type=forking
ExecStart=/usr/bin/eGTouchD
User=root
Group=root
RemainAfterExit=yes
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
Than reload 

```console
sudo systemctl daemon-reload
sudo systemctl restart egettouchd.service
sudo systemctl status egettouchd.service
```
