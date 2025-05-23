<div align="center">

## 🌍 **LANGUAGE SELECTION / CHỌN NGÔN NGỮ**

[![Vietnamese](https://img.shields.io/badge/🇻🇳_Vietnamese-Tiếng_Việt-red?style=for-the-badge)](README.md)
[![English](https://img.shields.io/badge/🇺🇸_English-English-blue?style=for-the-badge)](README-en.md)
[![Chinese](https://img.shields.io/badge/🇨🇳_Chinese-中文-yellow?style=for-the-badge)](README-zh.md)
[![Russian](https://img.shields.io/badge/🇷🇺_Russian-Русский-green?style=for-the-badge)](#)

</div>

---

<div align="center">
  <h1>Полное руководство по установке macOS на Windows. Разблокировка всех функций и успешный вход в iCloud</h1>
</div>

<div align="center">
  <img src="https://i.postimg.cc/kXsfgfWG/Screenshot-2025-05-23-at-13-35-13.png" alt="macOS on Windows - Full Setup Preview" width="800"/>
</div>

---

<div align="center">

## 📋 **СОДЕРЖАНИЕ**

</div>

<div align="center">

| 🔢 | 📂 **РАЗДЕЛ** | 📋 **СОДЕРЖАНИЕ** |
|:---:|:---|:---|
| **1** | **[🔧 Подготовка системы Windows](#1--подготовка-системы-windows)** | Отключение Core Isolation и Hypervisor |
| **2** | **[🛠️ Установка VMware и Unlocker](#2-️-установка-vmware-и-unlocker)** | VMware Workstation Pro и Unlocker |
| **3** | **[💻 Создание вспомогательной macOS VM](#3--создание-вспомогательной-macos-vm)** | Конфигурация ВМ и VMware Tools |
| **4** | **[💿 Создание основного macOS ISO](#4--создание-основного-macos-iso)** | Команды терминала и создание ISO |
| **5** | **[🎯 Официальная установка macOS](#5--официальная-установка-macos)** | Основная виртуальная машина |
| **6** | **[⚙️ Расширенная конфигурация](#6-️-расширенная-конфигурация)** | SMBIOS, разрешение и сеть |
| **7** | **[✅ Проверка и завершение](#7--проверка-и-завершение)** | Проверка и Apple ID |

</div>

---

## 1. 🔧 Подготовка системы Windows

### 1.1 Отключение Core Isolation

1. Нажмите клавишу **Windows**, найдите `"Core isolation"`
2. В разделе **Memory integrity** выключите переключатель для отключения этой функции

### 1.2 Отключение Hypervisor

1. Откройте **Командную строку (CMD)** от имени администратора
2. Введите команду:
   ```cmd
   bcdedit /set hypervisorlaunchtype off
   ```
3. Перезагрузите компьютер

---

## 2. 🛠️ Установка VMware и Unlocker

### 2.1 Установка VMware Workstation Pro

- Скачайте и установите последнюю версию **VMware Workstation Pro**

### 2.2 Установка Unlocker для VMware

1. Скачайте Unlocker по адресу: https://github.com/paolo-projects/unlocker.git
2. Распакуйте скачанную папку
3. Запустите следующие файлы от имени **администратора**:
   - `win-install.cmd`
   - `win-update-tools.cmd`

---

## 3. 💻 Создание вспомогательной macOS VM

### 3.1 Скачать файл macOS ISO

**Пример:** Mac OS 15 Sequoia by Metaperso  
📥 [Скачать на Archive.org](https://archive.org/details/mac-os-15-sequoia-by-metaperso)

### 3.2 Настройка виртуальной машины

1. Запустите **VMware** → **Создать новую виртуальную машину**
2. **Рекомендуемая конфигурация:**

   | Характеристика | Значение |
   |----------|---------|
   | **Процессоры** | 1 |
   | **Ядер на процессор** | 8 |
   | **Память (RAM)** | 8 ГБ - 16 ГБ |
   | **Размер диска** | 100 ГБ - 150 ГБ |

### 3.3 Установка VMware Tools

1. Загрузите macOS → **VM** → **Установить VMware Tools** → скачайте `darwin.iso`
2. Выключите ВМ → **Изменить настройки виртуальной машины** → **CD/DVD (SATA)** → **Использовать файл образа ISO** → выберите `darwin.iso`
3. Перезапустите и установите VMware Tools
4. Перейдите в **Настройки** → **Конфиденциальность и безопасность** → **Разрешить**
5. Перезапустите виртуальную машину

---

## 4. 💿 Создание основного macOS ISO

### 4.1 Подготовка терминала

1. Откройте **Finder** → **Приложения** → **Утилиты** → **Терминал**
2. Выполните следующие команды:

```bash
# Автоматически скачать совместимый установщик MacOS
softwareupdate --fetch-full-installer

# Проверить скачанный .app файл
ls /Applications | grep "Install macOS"
```

### 4.2 Создание ISO файла

```bash
# Создать пустой DMG файл
hdiutil create -o /tmp/macos -size 16000m -volname macos -layout SPUD -fs HFS+J

# Смонтировать DMG файл
hdiutil attach /tmp/macos.dmg -noverify -mountpoint /Volumes/macos

# Создать установщик в DMG (пример с MacOS Ventura)
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/macos

# Размонтировать диск
hdiutil detach "/Volumes/Install macOS Ventura"

# Конвертировать DMG в ISO
hdiutil convert /tmp/macos.dmg -format UDTO -o ~/Desktop/macos.iso

# Переименовать файл в .iso
mv ~/Desktop/macos.iso.cdr ~/Desktop/macos.iso
```

### 4.3 Сохранение и передача ISO файла

1. Сжать `macos.iso` в **ZIP**
2. Загрузить на **Google Drive**, **Dropbox** или другое облачное хранилище
3. Скачать на Windows машину

---

## 5. 🎯 Официальная установка macOS

### 5.1 Удаление вспомогательной ВМ (по желанию)

- Щелкните правой кнопкой мыши на виртуальной машине → **Управление** → **Удалить с диска**

### 5.2 Создание новой виртуальной машины

- Повторите процесс создания ВМ и установки VMware Tools как в **Разделе 3**

---

## 6. ⚙️ Расширенная конфигурация

### 6.1 Настройка разрешения

Откройте **Терминал** и выполните:

```bash
"/Library/Application Support/VMware Tools/vmware-resolutionSet 1920 1080"
```

**Другие разрешения:**
- `2560×1440`
- `2048×1080` 
- `3840×2160`

### 6.2 Настройка SMBIOS

1. Выключите ВМ и VMware
2. Скачайте и установите **Python** (если нужно)
3. Скачайте **GenSMBIOS**: https://github.com/corpnewt/GenSMBIOS.git
4. Распакуйте и запустите:
   ```bash
   python GenSMBIOS.py

   # Выберите модель Mac (рекомендуется):
   # iMac20,1     - Современный iMac
   # MacPro7,1    - Mac Pro  
   # iMacPro1,1   - iMac Pro
   ```
5. Выберите модель (например: `iMac20,1`) и запишите информацию:
   - `smbios.product`
   - `smbios.serial`
   - `smbios.boardId`
   - `hw.model`
   - `board-id`
   - `smc.version`

### 6.3 Редактирование .vmx файла

1. Откройте `.vmx` файл в **Блокноте**
2. Вставьте информацию SMBIOS в конец файла
3. Удалите `.nvram` файл в папке ВМ

### 6.4 Настройка сети (по желанию)

1. Перейдите в **VM** → **Настройки** → **Сетевой адаптер**
2. Измените с **NAT** на **Мост**

---

## 7. ✅ Проверка и завершение

### 7.1 Запуск виртуальной машины

- Перезапустите виртуальную машину macOS

### 7.2 Проверка установки

- Войдите в **Apple ID**
- Если успешно → Установка завершена! 🎉

---

### 💰 Поддержать этот проект
Если вы считаете этот проект полезным, рассмотрите возможность поддержки разработчика:
- **Местный банк**: `1039506134` | LE PHI ANH | Vietcombank
- **MoMo**: `0971390849` | LE PHI ANH
- **ETH & USDT**: `0x928F8c5443b13f71a4d7094E8bD2E74c86127243`

---

## 👨‍💻 Автор и контакты
<div align="center">
  <h3>Le Phi Anh</h3>
</div>
<div align="center">
  <a href="https://github.com/LePhiAnhDev" target="_blank"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://t.me/lephianh386ht" target="_blank"><img src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram"></a>
  <a href="https://lephianh.id.vn/" target="_blank"><img src="https://img.shields.io/badge/Website-FF7139?style=for-the-badge&logo=Firefox-Browser&logoColor=white" alt="Website"></a>
</div>
