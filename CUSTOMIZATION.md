# Archibald Linux – Customization Guide

## Overview

Archibald Linux is designed for customization. This guide covers common post-installation tweaks, configuration, and personalization.

---

## Desktop Environment (KDE Plasma)

### Change Wallpaper

**Method 1: Right-click Desktop**
1. Right-click on desktop
2. Select "Configure Desktop..."
3. Click "Wallpaper" tab
4. Choose image or browse for your own

**Method 2: System Settings**
1. Open "System Settings" (or `kdesystemsettings5`)
2. Navigate to: **Appearance** → **Wallpaper**
3. Select or browse for wallpaper

### Change Color Scheme

**System Settings → Appearance → Global Themes**

- Select "Colors" tab
- Choose from installed schemes or download new ones
- Click "Get New..." for additional schemes from KDE Store

**Popular KDE Color Schemes:**
- Breeze (default, light and dark variants)
- Papirus (modern, clean)
- Ant Dracula (dark, high contrast)

### Customize Panel (Taskbar)

**Edit Panel:**
1. Right-click on panel
2. Select "Edit Panel"
3. Resize, reposition, or add widgets
4. Add application menu, clock, system tray, etc.

**Add Widgets:**
1. Right-click on panel
2. Select "Add Widgets..."
3. Drag widgets to panel

### Install Themes & Styles

**Via KDE Store (GUI):**
1. System Settings → Appearance → Global Themes
2. Click "Get New..." (bottom of window)
3. Browse and download themes
4. Apply theme

**Via Command Line:**
```bash
# Install popular themes
sudo pacman -S materia-gtk oxygen-kde5 layan-kde

# Or use Pamac (graphical pacman frontend)
sudo pacman -S pamac-aur
pamac search kde-theme
```

---

## System Configuration

### Network

**Wi-Fi:**
```bash
# Open network manager
nmtui

# Or via GUI (KDE):
# Click network icon → Connect to network
```

**Static IP:**
```bash
# Edit NetworkManager config
sudo nano /etc/NetworkManager/conf.d/static.conf

# Add:
[main]
# ... existing config ...
```

### Display & Monitor

**Multiple Monitors:**
1. System Settings → **Startup and Shutdown** → **Display Configuration**
2. Arrange monitors by dragging
3. Set primary display
4. Apply

**Resolution & Refresh Rate:**
1. System Settings → **Display Configuration**
2. Select monitor
3. Choose resolution and refresh rate
4. Apply

### Keyboard & Input

**Keyboard Layout:**
1. System Settings → **Input Devices** → **Keyboard**
2. Add/remove keyboard layouts
3. Switch layouts via panel widget or Alt+Shift (if configured)

**Mouse Settings:**
1. System Settings → **Input Devices** → **Mouse**
2. Adjust sensitivity, acceleration, double-click speed

---

## Software Management

### Install Applications

**Via Pacman (command line):**
```bash
# Search for packages
pacman -Ss firefox

# Install
sudo pacman -S firefox

# Install multiple
sudo pacman -S firefox chromium thunderbird

# Update all packages
sudo pacman -Syu
```

**Via Pamac (GUI, includes AUR):**
```bash
sudo pacman -S pamac-aur
pamac
```

Then search and install graphically.

### Popular Applications

**Media:**
```bash
sudo pacman -S vlc mpv ffmpeg
```

**Productivity:**
```bash
sudo pacman -S libreoffice thunderbird
```

**Development:**
```bash
sudo pacman -S code vim git
```

**Graphics:**
```bash
sudo pacman -S gimp inkscape krita
```

**Utilities:**
```bash
sudo pacman -S ranger fzf bat exa
```

### Enable AUR (Arch User Repository)

AUR provides community-maintained packages:

```bash
# Install git and base-devel (needed for AUR)
sudo pacman -S git base-devel

# Install yay (AUR helper)
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Use yay to install AUR packages
yay -Ss package-name
yay -S aur-package-name
```

---

## System Optimization

### Improve Boot Time

```bash
# Check boot time
systemd-analyze

# Check slowest services
systemd-analyze blame

# Disable unnecessary services
sudo systemctl disable service-name
```

### Clean Up Disk Space

```bash
# Remove orphaned packages
sudo pacman -Rns $(pacman -Qdtq)

# Clean pacman cache (keep last 3 versions)
sudo pacman -Sc

# Clean all old pacman cache (frees more space)
sudo pacman -Scc  # Warning: will require re-downloading packages

# Remove temporary files
rm -rf ~/.cache/tmp/*
```

### System Monitoring

```bash
# Interactive system monitor
htop

# Or in GUI: Task Manager (included with KDE)

# Disk usage
ncdu ~/.
```

---

## Terminal Customization

### Shell Configuration

**Change Default Shell:**
```bash
# List available shells
cat /etc/shells

# Change to zsh
chsh -s /bin/zsh

# Or fish shell
sudo pacman -S fish
chsh -s /usr/bin/fish
```

**Customize Bash/Zsh:**

Edit `~/.bashrc` or `~/.zshrc`:

```bash
# Add aliases
alias ll='ls -lah'
alias update='sudo pacman -Syu'

# Custom prompt (example for bash)
PS1='\u@\h:\w$ '

# Export environment variables
export EDITOR=nano
```

**Install Oh-My-Zsh (if using zsh):**
```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Terminal Emulator

**Configure Konsole (default KDE terminal):**

1. Open Konsole
2. Menu → **Settings** → **Configure Konsole**
3. Customize:
   - **Appearance:** Font, color scheme, transparency
   - **Scrollback:** History lines
   - **Keyboard:** Shortcuts

---

## File Management

### Configure Dolphin (File Manager)

**Preferences:**
1. Open Dolphin (Files)
2. Menu → **Settings** → **Configure Dolphin**
3. Customize:
   - **General:** Default view, sorting
   - **Startup:** Default location
   - **View Modes:** Icon/list/compact view options

**Add Custom Places:**
1. In Dolphin sidebar, right-click
2. Select "Add Entry"
3. Add frequently used directories

### Set File Associations

**Open Dolphin → Edit File Type Associations:**
1. Right-click a file
2. Select "Properties"
3. "Open With" tab
4. Choose default application

---

## Performance Tuning

### GPU Acceleration

**For Intel:**
```bash
sudo pacman -S lib32-intel-gop-firmware
```

**For AMD:**
```bash
sudo pacman -S xf86-video-amdgpu mesa lib32-mesa
```

**For NVIDIA:**
```bash
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils
# Reboot required
sudo reboot
```

### Swap Configuration

```bash
# Check current swap
free -h

# Adjust swappiness (lower = prefer RAM)
# Default: 60 (out of 100)
# Add to /etc/sysctl.d/99-swappiness.conf:
echo 'vm.swappiness=10' | sudo tee /etc/sysctl.d/99-swappiness.conf

# Apply
sudo sysctl -p
```

---

## Backup & Recovery

### Create System Backup

```bash
# Full system backup to external drive
sudo rsync -av --delete / /mnt/backup/archibald-backup

# Backup home directory only
tar -czf ~/backup-$(date +%Y%m%d).tar.gz ~/.config ~/.local/share
```

### Restore from Backup

```bash
# Restore full system
sudo rsync -av /mnt/backup/archibald-backup/ /

# Restore specific directory
tar -xzf backup-*.tar.gz -C ~/
```

---

## Troubleshooting Customization

### Plasma Crashed?

```bash
# Restart Plasma without rebooting
killall plasmashell && kstart5 plasmashell
```

### Theme Not Applied?

```bash
# Clear Plasma cache
rm -rf ~/.cache/kde*
rm -rf ~/.config/kde*

# Restart Plasma
killall plasmashell && kstart5 plasmashell
```

### Package Conflicts?

```bash
# Check for broken dependencies
sudo pacman -Dk

# Force reinstall if corrupted
sudo pacman -S --force package-name
```

---

## Advanced Customization

### Custom Keyboard Shortcuts

**System Settings → Shortcuts and Gestures → Custom Shortcuts**

1. Create new group
2. Right-click → "New Shortcut..."
3. Define action (application or command)
4. Assign keyboard shortcut

### Autostart Applications

**Add startup scripts:**
```bash
# Location: ~/.config/autostart/
mkdir -p ~/.config/autostart

# Create .desktop file:
nano ~/.config/autostart/myapp.desktop

# Content:
[Desktop Entry]
Type=Application
Name=My App
Exec=/path/to/app
```

### Custom Service Units

```bash
# Create systemd user service
nano ~/.config/systemd/user/myservice.service

[Unit]
Description=My Custom Service

[Service]
Type=simple
ExecStart=/path/to/command

[Install]
WantedBy=default.target

# Enable and start
systemctl --user enable myservice
systemctl --user start myservice
```

---

## Next Steps

- Explore [Arch Linux Wiki](https://wiki.archlinux.org/) for deeper customization
- Install additional software to suit your workflow
- Share your customizations with the community!

**Customize responsibly. Keep your system clean and documented. 🚀**
