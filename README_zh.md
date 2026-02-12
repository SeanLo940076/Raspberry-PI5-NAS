# Raspberry-PI5-NAS

[English Version](README.md)

此專案提供基於 **Raspberry Pi 5** 的雙 SSD NAS 製作紀錄。

感謝 [OpenMediaVault Plugin Developers](https://github.com/OpenMediaVault-Plugin-Developers/installScript) 的安裝教學。

詳細安裝流程建議直接參考 OpenMediaVault Plugin Developers 提供的步驟；本文僅整理 **硬體配置** 與 **安裝時需要注意的項目**。

如果在使用、設計或程式上遇到任何問題，或有任何改進建議，歡迎提出 [Issue](../../issues) 與我討論！

---

## 硬體配置

### 1) 硬體

- [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/)
- [Waveshare PCIe TO 2-CH M.2 HAT+ (B)](https://www.waveshare.com/wiki/PCIe_TO_2-CH_M.2_HAT%2B_(B))
- [PNY CS3040 2TB x 2](https://www.pny.com/cs3040-m2-nvme-ssd)
- [Case P579](https://wiki.geekworm.com/P579)
- microSD（作為系統開機碟）

### 2) 作業系統

- 請使用 **Raspberry Pi OS Lite (64-bit) - Bookworm**，避免使用 **Full**（桌面版）。
- 請勿勾選 Raspberry Pi Imager 的 **設定無線網路**：Wi‑Fi 可在稍後於 OMV Web GUI 中設定（若需要，請參考完整安裝教學）。
- 記得設定 **時區**。
- Linux 使用者名稱請勿使用 **admin**（OMV Web GUI 預設帳號為 `admin`）。
- 啟用 **SSH**。
- 建議後續使用 **乙太網路（有線）** 進行設定與管理。

---

## 零件與成品結果

### 1) 零件與外觀

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/Component.jpg" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/WithoutCase.jpg" width="350" />
</div>


### 2) 大小參照與功耗

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/CaseSizeRef.jpg" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/Power.jpg" width="350" />
</div>

---

## 安裝 / 使用範例

### 1) 更新系統並設定 PCIe 為 Gen 2
1. 透過指令直接設定（`sudo nano /boot/firmware/config.txt`）加入下面修正
   ```bash
   dtparam=pciex1_gen=2
   ```

2. 更新系統套件：
   ```bash
   sudo apt-get update
   sudo apt-get upgrade -y
   ```

> 註：Gen 2 通常較穩定；若你要追求較高頻寬，可再視情況改為 Gen 3 進行測試。

**設定示意圖（PCIe Speed）：**

<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_1.png" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_2.png" width="350" />
</div>
<div align="center">
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_3.png" width="350" />
  <img src="https://github.com/SeanLo940076/Raspberry-PI5-NAS/blob/main/Demo/PCIESpeed_4.png" width="350" />
</div>


### 2) 安裝 OpenMediaVault

```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/preinstall | sudo bash
```

### 3) 等待與重啟

安裝時間可能需要一段時間（視網路與 microSD 速度而定）。完成後請重新開機 (我大概等待 1 小時)：

```bash
sudo reboot
```

### 4) 初次登入與設置

- 重啟後，可以使用與 SSH 用戶端相同的 **IP 位址**，在瀏覽器輸入該 IP 進入 OMV：  
  `http://<你的樹莓派IP>/`
- OMV Web GUI 預設帳密：
  - 使用者名稱：`admin`
  - 密碼：`openmediavault`

### 5) 安裝 RAID 外掛（mdadm）

在 OMV Web GUI：

- **系統** → **外掛（Plugins）** → 安裝 `openmediavault-md`

若安裝過程出現錯誤，多數情況可透過修復與重新安裝解決：

```bash
sudo dpkg --configure -a
sudo apt -f install
sudo apt update
```

### 6) 建立共享資料夾並啟用 SMB

在 OMV Web GUI：

- **儲存裝置** → **共享檔案夾** → **新增**
- **服務** → **SMB/CIFS** → **設定**：勾選 **啟用**
- **服務** → **SMB/CIFS** → **共享**：按 **新增**

---

## License

本專案採用 MIT License 授權。詳細內容請參考 [LICENSE](LICENSE)。  
感謝你的閱讀，祝你使用愉快，期待你的回饋與貢獻！
