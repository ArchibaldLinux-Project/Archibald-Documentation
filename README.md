# Archibald Linux

A clean, minimal Arch Linux-based distribution with KDE Plasma, systemd, and the Calamares installer. Archibald is designed for users who want a lightweight, branded Arch experience without bloatware.

## Features

- **Base:** Arch Linux with systemd
- **Desktop:** KDE Plasma (essential packages only, no bloat)
- **Installer:** Calamares with Archibald branding
- **Login:** ly greeter with minimal Archibald theming
- **Bootloader:** systemd-boot (modern, integrated with systemd)
- **Branding:** Consistent visual identity across all components

## Quick Start

### Building the ISO

```bash
cd /home/bowser/ArchibaldLinux
make build
```

The resulting ISO will be in `build/out/`.

### System Requirements for Building

- Arch Linux (or compatible environment)
- `mkarchiso` installed
- Root/sudo access (mkarchiso requires it)
- ~30GB free disk space
- ~2GB RAM minimum

See [docs/BUILD.md](docs/BUILD.md) for detailed build instructions and troubleshooting.

## Project Structure

```
ArchibaldLinux/
├── build/                  # ISO build system
│   ├── archlive/          # mkarchiso profile (profiledef.sh, pacman.conf, packages)
│   └── out/               # Output ISOs (generated)
├── branding/              # Visual assets
│   ├── archibald-logo.png # Main logo
│   ├── wallpapers/        # Desktop backgrounds
│   ├── icons/             # Icon theme assets
│   ├── grub-theme/        # GRUB2 bootloader theme
│   └── design-system.md   # Brand guidelines
├── calamares/             # Installer configuration
│   ├── settings.conf      # Module configuration
│   ├── branding/          # Installer theming (QML, images)
│   └── modules/           # Custom Calamares modules
├── kde-plasma/            # KDE Plasma defaults
├── ly-greeter/            # ly login greeter config
├── pacman/                # Custom package definitions
├── scripts/               # Build automation
│   ├── build.sh           # Main build wrapper
│   ├── version-bump.sh    # Version management
│   ├── sign-iso.sh        # GPG signing
│   └── Makefile           # Build orchestration
└── docs/                  # Documentation
    ├── BUILD.md
    ├── INSTALLATION.md
    ├── BRANDING.md
    ├── CUSTOMIZATION.md
    └── DEVELOPMENT.md
```

## Documentation

- [BUILD.md](BUILD.md) — How to build the ISO from source
- [INSTALLATION.md](INSTALLATION.md) — Installation guide for end users
- [BRANDING.md](BRANDING.md) — Brand guidelines and asset usage
- [CUSTOMIZATION.md](CUSTOMIZATION.md) — Post-install customization
- [DEVELOPMENT.md](DEVELOPMENT.md) — Contributing guidelines

## Release Strategy

Archibald uses **snapshot-based versioning**: each build is tagged with a timestamp (e.g., `SNAPSHOT-20260521`). Future releases can adopt semantic versioning if desired.

## License

[Specify your license here, e.g., GPL-3.0, MIT]

## Contributing

See [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md) for contribution guidelines.

---

**Current Status:** Initial project setup (Phase 1-2)
