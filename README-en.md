<div align="center">

## 🌍 **LANGUAGE SELECTION / CHỌN NGÔN NGỮ**

[![Vietnamese](https://img.shields.io/badge/🇻🇳_Vietnamese-Tiếng_Việt-red?style=for-the-badge)](README.md)
[![English](https://img.shields.io/badge/🇺🇸_English-English-blue?style=for-the-badge)](#)
[![Chinese](https://img.shields.io/badge/🇨🇳_Chinese-中文-yellow?style=for-the-badge)](README-zh.md)
[![Russian](https://img.shields.io/badge/🇷🇺_Russian-Русский-green?style=for-the-badge)](README-ru.md)

</div>

---

<div align="center">
  <h1>Complete Guide to Install macOS on Windows. Unlock Full Features and Successfully Login to iCloud</h1>
</div>

<div align="center">
  <img src="https://i.postimg.cc/kXsfgfWG/Screenshot-2025-05-23-at-13-35-13.png" alt="macOS on Windows - Full Setup Preview" width="800"/>
</div>

---

<div align="center">

## 📋 **TABLE OF CONTENTS**

</div>

<div align="center">

| 🔢 | 📂 **SECTION** | 📋 **CONTENT** |
|:---:|:---|:---|
| **1** | **[🔧 Windows System Preparation](#1--windows-system-preparation)** | Disable Core Isolation & Hypervisor |
| **2** | **[🛠️ Install VMware and Unlocker](#2-️-install-vmware-and-unlocker)** | VMware Workstation Pro & Unlocker |
| **3** | **[💻 Create Helper macOS VM](#3--create-helper-macos-vm)** | VM Configuration & VMware Tools |
| **4** | **[💿 Create Main macOS ISO](#4--create-main-macos-iso)** | Terminal commands & ISO creation |
| **5** | **[🎯 Official macOS Installation](#5--official-macos-installation)** | Main Virtual Machine |
| **6** | **[⚙️ Advanced Configuration](#6-️-advanced-configuration)** | SMBIOS, Resolution & Network |
| **7** | **[✅ Verification and Completion](#7--verification-and-completion)** | Verification & Apple ID |

</div>

---

## 1. 🔧 Windows System Preparation

### 1.1 Disable Core Isolation

1. Press **Windows** key, search for `"Core isolation"`
2. In **Memory integrity** section, toggle OFF to disable this feature

### 1.2 Disable Hypervisor

1. Open **Command Prompt (CMD)** as Administrator
2. Enter command:
   ```cmd
   bcdedit /set hypervisorlaunchtype off
   ```
3. Restart computer

---

## 2. 🛠️ Install VMware and Unlocker

### 2.1 Install VMware Workstation Pro

- Download and install latest version of **VMware Workstation Pro**

### 2.2 Install Unlocker for VMware

1. Download Unlocker at: https://github.com/paolo-projects/unlocker.git
2. Extract downloaded folder
3. Run following files as **Administrator**:
   - `win-install.cmd`
   - `win-update-tools.cmd`

---

## 3. 💻 Create Helper macOS VM

### 3.1 Download macOS ISO File

**Example:** Mac OS 15 Sequoia by Metaperso  
📥 [Download at Archive.org](https://archive.org/details/mac-os-15-sequoia-by-metaperso)

### 3.2 Configure Virtual Machine

1. Start **VMware** → **Create a New Virtual Machine**
2. **Recommended Configuration:**

   | Specification | Value |
   |----------|---------|
   | **Processors** | 1 |
   | **Cores per processor** | 8 |
   | **Memory (RAM)** | 8 GB - 16 GB |
   | **Disk size** | 100 GB - 150 GB |

### 3.3 Install VMware Tools

1. Boot macOS → **VM** → **Install VMware Tools** → download `darwin.iso`
2. Shutdown VM → **Edit virtual machine settings** → **CD/DVD (SATA)** → **Use ISO image file** → select `darwin.iso`
3. Restart and install VMware Tools
4. Go to **Settings** → **Privacy & Security** → **Allow**
5. Restart virtual machine

---

## 4. 💿 Create Main macOS ISO

### 4.1 Prepare Terminal

1. Open **Finder** → **Applications** → **Utilities** → **Terminal**
2. Run following commands:

```bash
# Automatically download compatible MacOS installer
softwareupdate --fetch-full-installer

# Check downloaded .app file
ls /Applications | grep "Install macOS"
```

### 4.2 Create ISO File

```bash
# Create empty DMG file
hdiutil create -o /tmp/macos -size 16000m -volname macos -layout SPUD -fs HFS+J

# Mount DMG file
hdiutil attach /tmp/macos.dmg -noverify -mountpoint /Volumes/macos

# Create installer in DMG (example with MacOS Ventura)
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/macos

# Unmount drive
hdiutil detach "/Volumes/Install macOS Ventura"

# Convert DMG to ISO
hdiutil convert /tmp/macos.dmg -format UDTO -o ~/Desktop/macos.iso

# Rename file to .iso
mv ~/Desktop/macos.iso.cdr ~/Desktop/macos.iso
```

### 4.3 Store and Transfer ISO File

1. Compress `macos.iso` to **ZIP**
2. Upload to **Google Drive**, **Dropbox** or other cloud storage
3. Download to Windows machine

---

## 5. 🎯 Official macOS Installation

### 5.1 Delete Helper VM (Optional)

- Right-click on virtual machine → **Manage** → **Delete from Disk**

### 5.2 Create New Virtual Machine

- Repeat VM creation process and install VMware Tools as in **Section 3**

---

## 6. ⚙️ Advanced Configuration

### 6.1 Adjust Resolution

Open **Terminal** and run:

```bash
"/Library/Application Support/VMware Tools/vmware-resolutionSet 1920 1080"
```

**Other resolutions:**
- `2560×1440`
- `2048×1080` 
- `3840×2160`

### 6.2 Configure SMBIOS

1. Shutdown VM and VMware
2. Download and install **Python** (if needed)
3. Download **GenSMBIOS**: https://github.com/corpnewt/GenSMBIOS.git
4. Extract and run:
   ```bash
   python GenSMBIOS.py

   # Select Mac model (recommended):
   # iMac20,1     - Modern iMac
   # MacPro7,1    - Mac Pro  
   # iMacPro1,1   - iMac Pro
   ```
5. Select model (example: `iMac20,1`) and record information:
   - `smbios.product`
   - `smbios.serial`
   - `smbios.boardId`
   - `hw.model`
   - `board-id`
   - `smc.version`

### 6.3 Edit .vmx File

1. Open `.vmx` file with **Notepad**
2. Paste SMBIOS information at end of file
3. Delete `.nvram` file in VM folder

### 6.4 Configure Network (Optional)

1. Go to **VM** → **Settings** → **Network Adapter**
2. Change from **NAT** to **Bridge**

---

## 7. ✅ Verification and Completion

### 7.1 Start Virtual Machine

- Restart macOS virtual machine

### 7.2 Verify Installation

- Login to **Apple ID**
- If successful → Installation complete! 🎉

---

### 💰 Support This Project
If you find this project useful, consider supporting the developer:
- **Local Bank**: `1039506134` | LE PHI ANH | Vietcombank
- **MoMo**: `0971390849` | LE PHI ANH
- **ETH & USDT**: `0x928F8c5443b13f71a4d7094E8bD2E74c86127243`

---

## 👨‍💻 Author & Contact
<div align="center">
  <h3>Le Phi Anh</h3>
</div>
<div align="center">
  <a href="https://github.com/LePhiAnhDev" target="_blank"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://t.me/lephianh386ht" target="_blank"><img src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram"></a>
  <a href="https://lephianh.id.vn/" target="_blank"><img src="https://img.shields.io/badge/Website-FF7139?style=for-the-badge&logo=Firefox-Browser&logoColor=white" alt="Website"></a>
</div>
