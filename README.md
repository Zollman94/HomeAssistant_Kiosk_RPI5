# **HomeAssistant Kiosk on Raspberry Pi 5**  
> **Guide to set up a kiosk using Raspberry Pi 5 and iiyama PLT1700 touch screen monitor**

## **🛠️ Requirements**
- **Raspberry Pi 5**  
- **Power supply** (5V, 3A)  
- **Touchscreen monitor**: iiyama PLT1700  
- **Cables**:  
  - USB A to USB B (for touch)  
  - HDMI/DVI/VGA to MicroHDMI (for video)  

---

## **📥 Install Raspberry Pi OS**
1. Download and install **Raspberry Pi Imager** from the [official website](https://www.raspberrypi.com/software/).  
2. Use the imager to install:  
   **Raspberry Pi OS Full (64-bit)** with **desktop environment**.  
3. Boot up the Raspberry Pi and connect the iiyama PLT1700 monitor.  
4. Open a terminal and run the following commands to update the system:  
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   sudo apt install git
   sudo apt install p7zip-full
   ```

---

## **🖥️ Verify Touchscreen Connection**
To ensure the Raspberry Pi recognizes the iiyama touchscreen, run:  
```bash
dmesg | grep -i touch
```
If the touchscreen is detected, you will see relevant logs in the output.  

---

## **📦 Install Touchscreen Drivers**
### **Option 1: Download from official website**  
Download the latest drivers from the official EETI website:  
🔗 [Download Linux Drivers](https://www.eeti.com/drivers_Linux.html)  

> **Important:** Ensure you download the driver for the **ARM architecture**.  

### **Option 2: Use the pre-downloaded version from this repository**  
If the official website is down, you can use the driver included in this repository.  

```bash
git clone https://github.com/Zollman94/HomeAssistant_Kiosk_RPI5.git
cd HomeAssistant_Kiosk_RPI5
7z x eGTouch_v2.5.13219.L-ma.7z
cd eGTouch_v2.5.13219.L-ma
```

---

## **🚀 Install the Driver**
1. Make the installation script executable:  
   ```bash
   sudo chmod +x setup.sh
   ```
2. Run the installer script as **sudo** and follow the on-screen instructions:  
   ```bash
   sudo ./setup.sh
   ```

> **Note:** This script will guide you through the driver installation process.  

3. **Reboot the system** to apply changes:  
   ```bash
   sudo reboot
   ```

---

## **🖐️ Test Touchscreen**
After reboot, you can test if the touchscreen is working properly.  

Run the following command to manually start the touchscreen service:  
```bash
cd /usr/bin
sudo ./eGTouchD
```

> If the touchscreen responds, then the driver is working correctly.  

---

## **⚙️ Enable Touchscreen on Boot**
To ensure that the touchscreen service starts automatically on boot, you need to create a **systemd service**.  

1. Create a new systemd service file:  
   ```bash
   sudo nano /etc/systemd/system/egettouchd.service
   ```

2. Paste the following configuration into the file:  
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

> **Explanation of Key Settings:**  
- `Type=forking`: This is used if the process "forks" (detaches from the terminal) to run in the background.  
- `RemainAfterExit=yes`: Ensures the service is considered "active" even if the process exits.  

---

## **🔄 Apply the Service Configuration**
Run the following commands to reload the systemd daemon, restart the service, and check its status:  
```bash
sudo systemctl daemon-reload
sudo systemctl restart egettouchd.service
sudo systemctl status egettouchd.service
```

You should see **"Active: active (running)"** in the service status.  

---

## **🖥️ Autostart chromium in fullscreen mode**
To set up launching chomium after system boot in full screen mode follow these steps:
Check if `~/.config/autostart/` already exists
```bash
ls ~/.config/autostart/
```
If not make one
```bash
mkdir -p ~/.config/autostart/
```
Make service file for script
```bash
nano ~/.config/autostart/start_chromium.desktop
```
Edit your `USERNAME` and paste this config
```ini
[Desktop Entry]
Type=Application
Name=Start Chromium
Exec=/home/<USERNAME>/start_chromium.sh
X-GNOME-Autostart-enabled=true
```
Make Script:
```bash
nano ~/start_chromium.sh
```
Paste this:
```bash
#!/bin/bash
/usr/bin/chromium --start-fullscreen
```
Done. Now set your default page to home assistant URL and you are all set up!

## **🎉 Finished!**
Now your Raspberry Pi 5 with iiyama PLT1700 touchscreen will automatically start the touchscreen service after each reboot with home assistant dashboard opened in chromium full screen.
If you encounter any issues, check the service logs with:  
```bash
journalctl -u egettouchd.service -e
```
