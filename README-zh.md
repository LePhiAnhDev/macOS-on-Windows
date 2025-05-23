<div align="center">

## 🌍 **LANGUAGE SELECTION / CHỌN NGÔN NGỮ**

[![Vietnamese](https://img.shields.io/badge/🇻🇳_Vietnamese-Tiếng_Việt-red?style=for-the-badge)](README.md)
[![English](https://img.shields.io/badge/🇺🇸_English-English-blue?style=for-the-badge)](README-en.md)
[![Chinese](https://img.shields.io/badge/🇨🇳_Chinese-中文-yellow?style=for-the-badge)](#)
[![Russian](https://img.shields.io/badge/🇷🇺_Russian-Русский-green?style=for-the-badge)](README-ru.md)

</div>

---

<div align="center">
  <h1>在Windows上安装macOS完整指南。解锁全部功能并成功登录iCloud</h1>
</div>

<div align="center">
  <img src="https://i.postimg.cc/kXsfgfWG/Screenshot-2025-05-23-at-13-35-13.png" alt="macOS on Windows - Full Setup Preview" width="800"/>
</div>

---

<div align="center">

## 📋 **目录**

</div>

<div align="center">

| 🔢 | 📂 **章节** | 📋 **内容** |
|:---:|:---|:---|
| **1** | **[🔧 Windows系统准备](#1--windows系统准备)** | 禁用核心隔离和Hypervisor |
| **2** | **[🛠️ 安装VMware和Unlocker](#2-️-安装vmware和unlocker)** | VMware Workstation Pro 和 Unlocker |
| **3** | **[💻 创建辅助macOS虚拟机](#3--创建辅助macos虚拟机)** | 虚拟机配置和VMware Tools |
| **4** | **[💿 创建主要macOS ISO](#4--创建主要macos-iso)** | 终端命令和ISO创建 |
| **5** | **[🎯 正式macOS安装](#5--正式macos安装)** | 主虚拟机 |
| **6** | **[⚙️ 高级配置](#6-️-高级配置)** | SMBIOS、分辨率和网络 |
| **7** | **[✅ 验证和完成](#7--验证和完成)** | 验证和Apple ID |

</div>

---

## 1. 🔧 Windows系统准备

### 1.1 禁用核心隔离

1. 按**Windows**键，搜索`"Core isolation"`
2. 在**内存完整性**部分，关闭开关以禁用此功能

### 1.2 禁用Hypervisor

1. 以管理员身份打开**命令提示符(CMD)**
2. 输入命令：
   ```cmd
   bcdedit /set hypervisorlaunchtype off
   ```
3. 重启计算机

---

## 2. 🛠️ 安装VMware和Unlocker

### 2.1 安装VMware Workstation Pro

- 下载并安装最新版本的**VMware Workstation Pro**

### 2.2 为VMware安装Unlocker

1. 在以下地址下载Unlocker：https://github.com/paolo-projects/unlocker.git
2. 解压下载的文件夹
3. 以**管理员**身份运行以下文件：
   - `win-install.cmd`
   - `win-update-tools.cmd`

---

## 3. 💻 创建辅助macOS虚拟机

### 3.1 下载macOS ISO文件

**示例：** Mac OS 15 Sequoia by Metaperso  
📥 [在Archive.org下载](https://archive.org/details/mac-os-15-sequoia-by-metaperso)

### 3.2 配置虚拟机

1. 启动**VMware** → **创建新虚拟机**
2. **推荐配置：**

   | 规格 | 值 |
   |----------|---------|
   | **处理器** | 1 |
   | **每个处理器的核心数** | 8 |
   | **内存(RAM)** | 8 GB - 16 GB |
   | **磁盘大小** | 100 GB - 150 GB |

### 3.3 安装VMware Tools

1. 启动macOS → **VM** → **安装VMware Tools** → 下载`darwin.iso`
2. 关闭虚拟机 → **编辑虚拟机设置** → **CD/DVD (SATA)** → **使用ISO映像文件** → 选择`darwin.iso`
3. 重启并安装VMware Tools
4. 转到**设置** → **隐私与安全** → **允许**
5. 重启虚拟机

---

## 4. 💿 创建主要macOS ISO

### 4.1 准备终端

1. 打开**访达** → **应用程序** → **实用工具** → **终端**
2. 运行以下命令：

```bash
# 自动下载兼容的MacOS安装程序
softwareupdate --fetch-full-installer

# 检查已下载的.app文件
ls /Applications | grep "Install macOS"
```

### 4.2 创建ISO文件

```bash
# 创建空的DMG文件
hdiutil create -o /tmp/macos -size 16000m -volname macos -layout SPUD -fs HFS+J

# 挂载DMG文件
hdiutil attach /tmp/macos.dmg -noverify -mountpoint /Volumes/macos

# 在DMG中创建安装程序（以MacOS Ventura为例）
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/macos

# 卸载驱动器
hdiutil detach "/Volumes/Install macOS Ventura"

# 将DMG转换为ISO
hdiutil convert /tmp/macos.dmg -format UDTO -o ~/Desktop/macos.iso

# 重命名文件为.iso
mv ~/Desktop/macos.iso.cdr ~/Desktop/macos.iso
```

### 4.3 存储和传输ISO文件

1. 将`macos.iso`压缩为**ZIP**
2. 上传到**Google Drive**、**Dropbox**或其他云存储
3. 下载到Windows机器

---

## 5. 🎯 正式macOS安装

### 5.1 删除辅助虚拟机（可选）

- 右键点击虚拟机 → **管理** → **从磁盘删除**

### 5.2 创建新虚拟机

- 重复虚拟机创建过程并安装VMware Tools，如**第3节**所述

---

## 6. ⚙️ 高级配置

### 6.1 调整分辨率

打开**终端**并运行：

```bash
"/Library/Application Support/VMware Tools/vmware-resolutionSet 1920 1080"
```

**其他分辨率：**
- `2560×1440`
- `2048×1080` 
- `3840×2160`

### 6.2 配置SMBIOS

1. 关闭虚拟机和VMware
2. 下载并安装**Python**（如需要）
3. 下载**GenSMBIOS**：https://github.com/corpnewt/GenSMBIOS.git
4. 解压并运行：
   ```bash
   python GenSMBIOS.py

   # 选择Mac型号（推荐）：
   # iMac20,1     - 现代iMac
   # MacPro7,1    - Mac Pro  
   # iMacPro1,1   - iMac Pro
   ```
5. 选择型号（例如：`iMac20,1`）并记录信息：
   - `smbios.product`
   - `smbios.serial`
   - `smbios.boardId`
   - `hw.model`
   - `board-id`
   - `smc.version`

### 6.3 编辑.vmx文件

1. 用**记事本**打开`.vmx`文件
2. 将SMBIOS信息粘贴到文件末尾
3. 删除虚拟机文件夹中的`.nvram`文件

### 6.4 配置网络（可选）

1. 转到**VM** → **设置** → **网络适配器**
2. 从**NAT**更改为**桥接**

---

## 7. ✅ 验证和完成

### 7.1 启动虚拟机

- 重启macOS虚拟机

### 7.2 验证安装

- 登录**Apple ID**
- 如果成功 → 安装完成！🎉

---

### 💰 支持此项目
如果您觉得此项目有用，请考虑支持开发者：
- **本地银行**: `1039506134` | LE PHI ANH | Vietcombank
- **MoMo**: `0971390849` | LE PHI ANH
- **ETH & USDT**: `0x928F8c5443b13f71a4d7094E8bD2E74c86127243`

---

## 👨‍💻 作者及联系方式
<div align="center">
  <h3>Le Phi Anh</h3>
</div>
<div align="center">
  <a href="https://github.com/LePhiAnhDev" target="_blank"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://t.me/lephianh386ht" target="_blank"><img src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram"></a>
  <a href="https://lephianh.id.vn/" target="_blank"><img src="https://img.shields.io/badge/Website-FF7139?style=for-the-badge&logo=Firefox-Browser&logoColor=white" alt="Website"></a>
</div>
