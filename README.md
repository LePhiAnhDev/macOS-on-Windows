<div align="center">

## 🌍 **LANGUAGE SELECTION / CHỌN NGÔN NGỮ**

[![Vietnamese](https://img.shields.io/badge/🇻🇳_Vietnamese-Tiếng_Việt-red?style=for-the-badge)](#)
[![English](https://img.shields.io/badge/🇺🇸_English-English-blue?style=for-the-badge)](README-en.md)
[![Chinese](https://img.shields.io/badge/🇨🇳_Chinese-中文-yellow?style=for-the-badge)](README-zh.md)
[![Russian](https://img.shields.io/badge/🇷🇺_Russian-Русский-green?style=for-the-badge)](README-ru.md)

</div>

---

<div align="center">
  <h1>Hướng dẫn cài đặt macOS trên Windows. Mở khoá đầy đủ tính năng và Đăng nhập iCloud thành công</h1>
</div>

<div align="center">
  <img src="https://i.postimg.cc/kXsfgfWG/Screenshot-2025-05-23-at-13-35-13.png" alt="macOS on Windows - Full Setup Preview" width="800"/>
</div>

---

<div align="center">

## 📋 **MỤC LỤC**

</div>

<div align="center">

| 🔢 | 📂 **PHẦN** | 📋 **NỘI DUNG** |
|:---:|:---|:---|
| **1** | **[🔧 Chuẩn bị hệ thống Windows](#1--chuẩn-bị-hệ-thống-windows)** | Vô hiệu hóa Core Isolation & Hypervisor |
| **2** | **[🛠️ Cài đặt VMware và Unlocker](#2-️-cài-đặt-vmware-và-unlocker)** | VMware Workstation Pro & Unlocker |
| **3** | **[💻 Tạo máy ảo macOS phụ](#3--tạo-máy-ảo-macos-phụ)** | Cấu hình máy ảo & VMware Tools |
| **4** | **[💿 Tạo file ISO macOS chính](#4--tạo-file-iso-macos-chính)** | Terminal commands & ISO creation |
| **5** | **[🎯 Cài đặt macOS chính thức](#5--cài-đặt-macos-chính-thức)** | Máy ảo chính thức |
| **6** | **[⚙️ Cấu hình nâng cao](#6-️-cấu-hình-nâng-cao)** | SMBIOS, Resolution & Network |
| **7** | **[✅ Kiểm tra và hoàn thiện](#7--kiểm-tra-và-hoàn-thiện)** | Xác minh & Apple ID |

</div>

---

## 1. 🔧 Chuẩn bị hệ thống Windows

### 1.1 Vô hiệu hóa Core Isolation

1. Nhấn phím **Windows**, tìm kiếm `"Core isolation"`
2. Trong phần **Memory integrity**, tắt công tắc để vô hiệu hóa tính năng này

### 1.2 Vô hiệu hóa Hypervisor

1. Mở **Command Prompt (CMD)** với quyền Administrator
2. Nhập lệnh:
   ```cmd
   bcdedit /set hypervisorlaunchtype off
   ```
3. Khởi động lại máy tính

---

## 2. 🛠️ Cài đặt VMware và Unlocker

### 2.1 Cài đặt VMware Workstation Pro

- Tải và cài đặt phiên bản mới nhất của **VMware Workstation Pro**

### 2.2 Cài đặt Unlocker cho VMware

1. Tải Unlocker tại: https://github.com/paolo-projects/unlocker.git
2. Giải nén thư mục vừa tải về
3. Chạy các file sau với quyền **Administrator**:
   - `win-install.cmd`
   - `win-update-tools.cmd`

---

## 3. 💻 Tạo máy ảo macOS phụ

### 3.1 Tải file ISO macOS

**Ví dụ:** Mac OS 15 Sequoia by Metaperso  
📥 [Tải tại Archive.org](https://archive.org/details/mac-os-15-sequoia-by-metaperso)

### 3.2 Cấu hình máy ảo

1. Khởi động **VMware** → **Create a New Virtual Machine**
2. **Cấu hình khuyến nghị:**

   | Thông số | Giá trị |
   |----------|---------|
   | **Processors** | 1 |
   | **Cores per processor** | 8 |
   | **Memory (RAM)** | 8 GB - 16 GB |
   | **Disk size** | 100 GB - 150 GB |

### 3.3 Cài đặt VMware Tools

1. Khởi động macOS → **VM** → **Install VMware Tools** → tải `darwin.iso`
2. Tắt máy ảo → **Edit virtual machine settings** → **CD/DVD (SATA)** → **Use ISO image file** → chọn `darwin.iso`
3. Khởi động lại và cài đặt VMware Tools
4. Vào **Settings** → **Privacy & Security** → **Allow**
5. Khởi động lại máy ảo

---

## 4. 💿 Tạo file ISO macOS chính

### 4.1 Chuẩn bị Terminal

1. Mở **Finder** → **Applications** → **Utilities** → **Terminal**
2. Chạy các lệnh sau:

```bash
# Tự động tải bộ cài đặt MacOS tương thích
softwareupdate --fetch-full-installer

# Kiểm tra file .app đã tải về
ls /Applications | grep "Install macOS"
```

### 4.2 Tạo file ISO

```bash
# Tạo file DMG trống
hdiutil create -o /tmp/macos -size 16000m -volname macos -layout SPUD -fs HFS+J

# Gắn kết file DMG
hdiutil attach /tmp/macos.dmg -noverify -mountpoint /Volumes/macos

# Tạo bộ cài đặt vào DMG (ví dụ với MacOS Ventura)
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/macos

# Tháo ổ đĩa
hdiutil detach "/Volumes/Install macOS Ventura"

# Chuyển DMG thành ISO
hdiutil convert /tmp/macos.dmg -format UDTO -o ~/Desktop/macos.iso

# Đổi tên file thành .iso
mv ~/Desktop/macos.iso.cdr ~/Desktop/macos.iso
```

### 4.3 Lưu trữ và chuyển file ISO

1. Nén `macos.iso` thành **ZIP**
2. Upload lên **Google Drive**, **Dropbox** hoặc dịch vụ lưu trữ khác
3. Tải về máy Windows

---

## 5. 🎯 Cài đặt macOS chính thức

### 5.1 Xóa máy ảo phụ (Tuỳ chọn)

- Nhấp chuột phải vào máy ảo → **Manage** → **Delete from Disk**

### 5.2 Tạo máy ảo mới

- Lặp lại quá trình tạo máy ảo và cài đặt VMware Tools như **Phần 3**

---

## 6. ⚙️ Cấu hình nâng cao

### 6.1 Chỉnh độ phân giải

Mở **Terminal** và chạy:

```bash
"/Library/Application Support/VMware Tools/vmware-resolutionSet 1920 1080"
```

**Các độ phân giải khác:**
- `2560×1440`
- `2048×1080` 
- `3840×2160`

### 6.2 Cấu hình SMBIOS

1. Tắt máy ảo và VMware
2. Tải và cài **Python** (nếu cần)
3. Tải **GenSMBIOS**: https://github.com/corpnewt/GenSMBIOS.git
4. Giải nén và chạy:
   ```bash
   python GenSMBIOS.py

   # Select Mac model (recommended):
   # iMac20,1     - Modern iMac
   # MacPro7,1    - Mac Pro  
   # iMacPro1,1   - iMac Pro
   ```
5. Chọn model (ví dụ: `iMac20,1`) và ghi nhận thông tin:
   - `smbios.product`
   - `smbios.serial`
   - `smbios.boardId`
   - `hw.model`
   - `board-id`
   - `smc.version`

### 6.3 Chỉnh sửa file .vmx

1. Mở file `.vmx` bằng **Notepad**
2. Dán thông tin SMBIOS vào cuối file
3. Xóa file `.nvram` trong thư mục máy ảo

### 6.4 Cấu hình mạng (Tuỳ chọn)

1. Vào **VM** → **Settings** → **Network Adapter**
2. Chuyển từ **NAT** sang **Bridge**

---

## 7. ✅ Kiểm tra và hoàn thiện

### 7.1 Khởi động máy ảo

- Khởi động lại máy ảo macOS

### 7.2 Xác minh cài đặt

- Đăng nhập **Apple ID**
- Nếu thành công → Hoàn tất quá trình cài đặt! 🎉

---

### 📡 Theo dõi tôi tại đây

💻 GitHub: [github.com/LePhiAnhDev](https://github.com/LePhiAnhDev)
🌐 Website: [lephianhdev.github.io/Portfolio-Page](https://lephianhdev.github.io/Portfolio-Page)
📬 Telegram: [t.me/lephianh386ht](https://t.me/lephianh386ht)
👥 Channel tech: [t.me/LPAnhDev](https://t.me/LPAnhDev)
🎥 TikTok: [tiktok.com/@lephianhdev](https://tiktok.com/@lephianhdev)

---

### 💰 Support This Project
If you find this project useful, consider supporting the developer:
- **Local Bank**: 1039506134 | LE PHI ANH | Vietcombank
- **MoMo**: 0971390849 | LE PHI ANH
- **ETH & USDT**: 0x928F8c5443b13f71a4d7094E8bD2E74c86127243

---

## 👨‍💻 Author & Contact
<div align="center">
  <h3>Le Phi Anh</h3>
</div>
<div align="center">
  <a href="https://github.com/LePhiAnhDev" target="_blank"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://t.me/lephianh386ht" target="_blank"><img src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram"></a>
  <a href="https://lephianh.id.vn/" target="_blank"><img src="https://img.shields.io/badge/Website-FF7139?style=for-the-badge&logo=Firefox-Browser&logoColor=white" alt="Website"></a>
</div> thì bổ sung thêm phần sau ở cuối ngay trước ### 💰 Support This Project nhé 💻 GitHub: github.com/LePhiAnhDev
🌐 Website: lephianhdev.github.io/Portfolio-Page
📬 Telegram: t.me/lephianh386ht
👥 Channel tech: t.me/LPAnhDev
🎥 TikTok: tiktok.com/@lephianhdev
