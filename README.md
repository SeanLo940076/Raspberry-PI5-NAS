# Raspberry-PI5-NAS

[中文版本](./README_zh.md)

This project documents how to build a **dual-SSD NAS** based on **Raspberry Pi 5**.

Special thanks to the installation guide from [OpenMediaVault Plugin Developers](https://github.com/OpenMediaVault-Plugin-Developers/installScript).

For the complete installation procedure, please follow the steps provided by OpenMediaVault Plugin Developers.  
This README only summarizes the **hardware setup** and **important notes during installation**.

If you encounter any issues or have suggestions (usage / design / code improvements), feel free to open an [Issue](../../issues)!

---

## Hardware

### 1) Components

- [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/)
- [Waveshare PCIe TO 2-CH M.2 HAT+ (B)](https://www.waveshare.com/wiki/PCIe_TO_2-CH_M.2_HAT%2B_(B))
- [PNY CS3040 2TB x 2](https://www.pny.com/cs3040-m2-nvme-ssd)
- [Case P579](https://wiki.geekworm.com/P579)
- microSD (used as the OS boot drive)

### 2) Operating System

- Use **Raspberry Pi OS Lite (64-bit) - Bookworm**. Avoid the **Full** (desktop) image.
- In Raspberry Pi Imager, **do not** pre-configure Wi‑Fi. You can configure Wi‑Fi later in the OMV Web GUI (if needed; refer to the full installation guide).
- Set the correct **timezone**.
- Do **not** use `admin` as your Linux username (`admin` is the default OMV Web GUI account).
- Enable **SSH**.
- It is recommended to use **Ethernet (wired)** for setup and management.

---

## Parts & Build Photos

### 1) Components & Appearance

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/Component.jpg" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/WithoutCase.jpg" width="350" />
</div>

### 2) Size Reference & Power Consumption

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/CaseSizeRef.jpg" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/Power.jpg" width="350" />
</div>

---

## Installation / Usage Example

### 1) Update System and Enable PCIe Gen 3

1. Enable **PCIe Gen 3** using `sudo raspi-config` (common path: Advanced Options → PCIe Speed → Gen 3):
   ```bash
   sudo raspi-config
   ```

2. Update system packages:
   ```bash
   sudo apt-get update
   sudo apt-get upgrade -y
   ```

> Note: If you encounter PCIe link instability or SSD disconnects, try switching back to Gen 2 to confirm stability.

**Reference screenshots (PCIe Speed):**

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_1.png" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_2.png" width="350" />
</div>
<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_3.png" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_4.png" width="350" />
</div>

### 2) Install OpenMediaVault

```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/preinstall | sudo bash
```

### 3) Wait and Reboot

Installation may take some time depending on your network and microSD speed (it took about **1 hour** in my case).  
Reboot after it finishes:

```bash
sudo reboot
```

### 4) First Login and Initial Setup

- After reboot, use the **same IP address** as your SSH client. Open a browser and go to:  
  `http://<your_pi_ip>/`
- Default OMV Web GUI credentials:
  - Username: `admin`
  - Password: `openmediavault`

### 5) Install RAID Plugin (mdadm)

In OMV Web GUI:

- **System** → **Plugins** → install `openmediavault-md`

If the installation fails, in most cases you can fix it with:

```bash
sudo dpkg --configure -a
sudo apt -f install
sudo apt update
```

### 6) Create a Shared Folder and Enable SMB

In OMV Web GUI:

- **Storage** → **Shared Folders** → **Add**
- **Services** → **SMB/CIFS** → **Settings**: check **Enable**
- **Services** → **SMB/CIFS** → **Shares**: click **Add**

---

## License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE).  
Thanks for reading — hope you enjoy using it, and contributions are welcome!
