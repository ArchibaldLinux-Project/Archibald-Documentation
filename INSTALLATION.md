# Archibald Linux – Installation Guide

## Overview

Archibald Linux uses the Calamares installer to guide you through the installation process. This guide covers the basic installation steps and initial setup.

---

## System Requirements

### Minimum

- **Processor:** Intel/AMD 64-bit processor from 2006 or later
- **RAM:** 2 GB
- **Storage:** 20 GB for base system (SSD recommended)
- **Boot:** UEFI or BIOS (both supported)

### Recommended

- **Processor:** Modern multi-core CPU
- **RAM:** 8 GB or more
- **Storage:** 50+ GB SSD for comfortable usage
- **Network:** Ethernet or Wi-Fi for internet access during installation

---

## Getting Started

### Step 1: Download ISO

Download the latest Archibald Linux ISO from the official website/mirror.

### Step 2: Create Bootable Media

#### On Linux

```bash
# Insert USB drive, then find its device name
lsblk

# Write ISO to USB (replace sdX with your device, e.g., sdb)
sudo dd if=Archibald-Linux-SNAPSHOT-*.iso of=/dev/sdX bs=4M status=progress && sync

# Eject USB safely
sudo eject /dev/sdX
```

#### On Windows

Use [Rufus](https://rufus.ie/) or [balena Etcher](https://www.balena.io/etcher/):
- Select the ISO file
- Select your USB drive
- Click "Write"

#### On macOS

```bash
# Find USB device
diskutil list

# Unmount it
diskutil unmountDisk /dev/diskX

# Write ISO
sudo dd if=Archibald-Linux-SNAPSHOT-*.iso of=/dev/rdiskX bs=4m && sync

# Eject
diskutil ejectDisk /dev/diskX
```

### Step 3: Boot from Media

1. Insert the USB drive into your computer
2. Restart and enter boot menu:
   - **Dell:** F2 or F10
   - **HP:** F10
   - **Lenovo:** F1 or F2
   - **ASUS:** F2 or Delete
   - **Generic:** F12 or Esc (or see your motherboard manual)
  
If nothing above worked use :

### Windows :

```batch
shutdown /r /fw /t 0
```

### Linux

```batch
systemctl reboot --firmware-setup
```

(If you are not using systemD, you know how to access the bios xD)

## Boot from media (after bios)

3. Select the USB drive to boot from
4. Wait for the live environment to load.

---

## Installation Process

### Live Environment

Once booted, you'll see:
- KDE Plasma desktop with a custom calamares like installer
- Network connection configured via NetworkManager (if any network is available)
- Root terminal available for advanced users

### Using Installer

**The installer will guide you through these steps:**

#### 1. Welcome & Language

- Select your language (default: English)
- Choose timezone/region

#### 2. Keyboard Layout

- Select your keyboard layout
- Test in the preview area

#### 3. Partitioning

Three options:

**Option A: Automatic Partitioning (Recommended for Beginners)**
- Calamares will automatically partition the disk
- Suitable for new installations
- Click "Next"

**Option B: Manual Partitioning (Advanced)**
- Create custom partitions
- Useful for dual-boot or specific layouts
- Requires understanding of filesystems

**Option C: Existing Partitions**
- Use existing partitions
- Useful for multi-boot setups

**Recommended Partition Layout (for 50GB SSD):**

```
/dev/sdaX1    512 MB   EFI System (FAT32)       → /boot/efi
/dev/sdaX2    4 GB     Linux Swap               → (swap)
/dev/sdaX3    45.5 GB  Linux Filesystem (Ext4) → /
```

#### 4. User Account

- **Username:** Lowercase, no spaces (e.g., `john`)
- **Computer name:** Your machine hostname (e.g., `archibald-laptop`)
- **Password:** Strong password (12+ chars recommended)
- Optionally auto-login (reduces security)

#### 5. Summary

- Review all settings
- Confirm and begin installation
- Wait 5-15 minutes depending on disk speed

#### 6. Completion

- Click "Done"
- Remove USB drive and restart

---

## First Boot

### Welcome

After reboot, you'll see:
1. **Boot screen:** systemd-boot with Archibald branding
2. **GRUB menu** (if dual-boot configured)
3. **ly greeter:** Login screen with Archibald theme
4. **KDE Plasma:** Desktop environment

### Login

1. Enter your username
2. Enter your password
3. Select session: **Plasmawayland** or **Plasma (X11)**
   - Recommended: Wayland (modern, better performance on most systems)

### Note

You can still use the normal way to install arch inside of KDE.
---

## Post-Installation Setup

### Update System

```bash
sudo pacman -Syu
```

### Install Additional Software

Arch Linux uses pacman for package management:

```bash
# Search for packages
pacman -Ss firefox

# Install package
sudo pacman -S firefox

# Remove package
sudo pacman -R firefox

# Update all packages
sudo pacman -Syu
```

### Enable Network Services

```bash
# Ensure NetworkManager is running because it should be
sudo systemctl enable --now NetworkManager
```

### Configure Display

If you need to change display settings:

```bash
# Open KDE System Settings
kdesystemsettings5

# Navigate to: Startup and Shutdown → Display Configuration
```

### Install Drivers

For better hardware support:

```bash
# NVIDIA (if applicable)
sudo pacman -S nvidia

# AMD
sudo pacman -S xf86-video-amdgpu

# Intel
sudo pacman -S xf86-video-intel

# Reboot required after driver installation
```

---

## Customization

### Change Wallpaper

KDE Plasma → Right-click desktop → "Configure Desktop" → Wallpaper

### Install Themes

```bash
# KDE Plasma themes
sudo pacman -S plasma-themes

# Or install from KDE Store:
# System Settings → Appearance → Global Themes → "Get New..."
```

### Add Applications

Common utilities:

```bash
# Text editor
sudo pacman -S code gedit

# Media player
sudo pacman -S vlc

# Image viewer
sudo pacman -S feh

# Web browser
sudo pacman -S firefox chromium
```

---

## Troubleshooting

### No Internet Connection

```bash
# Restart NetworkManager
sudo systemctl restart NetworkManager

# Or manually connect:
nmtui
```

### GRUB/Boot Issues

If the system doesn't boot after installation:

1. Boot from USB again
2. Chroot into installed system:
   ```bash
   sudo mount /dev/sdaX3 /mnt
   sudo arch-chroot /mnt
   ```
3. Reinstall bootloader:
   ```bash
   sudo bootctl install
   ```

### Stuck at Login Screen

- Verify caps lock is off
- Check keyboard layout (use arrow keys to change if needed)
- Try Ctrl+Alt+F1 for TTY login

---

## Next Steps

- Read [CUSTOMIZATION.md](CUSTOMIZATION.md) for deeper system configuration
- Check [docs/DEVELOPMENT.md](DEVELOPMENT.md) for contributing to Archibald Linux
- Visit [Arch Linux Wiki](https://wiki.archlinux.org/) for comprehensive documentation

---

## Support

For issues:
1. Check [Arch Linux Wiki](https://wiki.archlinux.org/)
2. Search [Arch Linux Forums](https://bbs.archlinux.org/)
3. Report bugs to the Archibald Linux project

Happy... idk! 🐧
