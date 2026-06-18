# Archibald Linux – Build Instructions

## Prerequisites

### System Requirements

- **OS:** Arch Linux (or compatible distribution like Manjaro, EndeavourOS)
- **Tools:** mkarchiso (from Arch Linux repositories)
- **Privileges:** Root/sudo access (mkarchiso requires it for filesystem mounting)
- **Disk Space:** ~50GB free (40GB for work directory, 10GB for output ISO)
- **RAM:** 2GB minimum
- **Network:** Required for downloading packages during build

### Installation

```bash
# Install mkarchiso and dependencies (on Arch/compatible Linux)
sudo pacman -S archiso squashfs-tools

# Clone or navigate to Archibald Linux project directory
cd /home/bowser/ArchibaldLinux
```

---

## Quick Build

### Basic Build

```bash
make build
```

This will:
1. Validate the build profile and dependencies
2. Run mkarchiso to build the ISO
3. Generate SHA256 and MD5 checksums
4. Output the ISO to `build/out/`

The resulting ISO will be named: `Archibald-Linux-SNAPSHOT-YYYYMMDD-x86_64.iso`

### With Version Update

```bash
make bump-version build
```

This bumps the snapshot version to the current date before building.

### Full Build Workflow (with GPG signing)

```bash
make full-build
```

Requires GPG keys configured for signing. See [Signing ISOs](#signing-isos) below.

---

## Detailed Build Steps

### 1. Validate Build Environment

```bash
# Check that mkarchiso is installed
which mkarchiso

# Check Arch Linux mirrorlist is present
ls /etc/pacman.d/mirrorlist
```

### 2. Review Build Profile

Edit the following if needed:

- **`build/archlive/profiledef.sh`** — ISO metadata (name, version, kernel)
- **`build/archlive/packages.x86_64`** — Package list (add/remove as needed)
- **`build/archlive/pacman.conf`** — Pacman configuration (mirror selection, repositories)
- **`branding/`** - Every branding files, feel free to use them how you want to.

### 3. Run Build

```bash
bash scripts/build.sh
```

Or use the Makefile:

```bash
make build
```

The build process:
- Creates a temporary work directory (`build/work/`)
- Downloads and stages packages
- Compresses the filesystem
- Generates boot configuration
- Produces the final ISO

**This can take 10-30 minutes** depending on network speed and system performance.

![NOTE]

Be carefull while compiling a distro on your main machine, it can result to unwanted changes in you missunderstand where you are on your machine (directory)



### 4. Verify Output

```bash
ls -lh build/out/
cat build/out/checksums.sha256
```

---

## Testing the ISO

### In Virtual Machine (VirtualBox/QEMU)

#### VirtualBox is not tested yet!
#### QEMU is tested on an x64 machine with and without KVM with 4G of ram.

#### VirtualBox

```bash
# Open VirtualBox and create new VM
# Set boot order: CD/DVD → Hard disk
# Mount the ISO file
# Start VM
```

#### QEMU (command-line)

```bash
# UEFI boot test
qemu-system-x86_64 \
  -m 2G \
  -enable-kvm \
  -bios /usr/share/OVMF/OVMF_CODE.fd \
  -cdrom build/out/Archibald-Linux-SNAPSHOT-*.iso

# BIOS boot test
qemu-system-x86_64 \
  -m 2G \
  -enable-kvm \
  -cdrom build/out/Archibald-Linux-SNAPSHOT-*.iso
```

### What to Test

- [ ] ISO boots successfully (UEFI mode)
- [ ] ISO boots successfully (BIOS mode)
- [ ] Calamares installer launches with Archibald branding
- [ ] Desktop wallpaper and theme appear correct
- [ ] GRUB/systemd-boot splash shows Archibald branding
- [ ] Included packages are present (verify with `pacman -Q` in live environment)
- [ ] No extra/bloatware packages included

---

## Troubleshooting

### mkarchiso not found

```bash
sudo pacman -S archiso
```

### Permission denied during build

```bash
# mkarchiso requires root. Verify sudo access:
sudo -l

# If needed, configure passwordless sudo for specific commands (NOT recommended for security)
# Instead, just enter your password when prompted
```

### Build fails with "Failed to find pacman cache"

- Ensure your Arch Linux system is up to date: `sudo pacman -Syu`
- Check `/etc/pacman.d/mirrorlist` is configured correctly
- Try updating the mirrorlist:
  ```bash
  sudo pacman -S archlinux-keyring
  sudo pacman-key --init
  ```

### Out of disk space

```bash
# Clean up previous builds
make clean

# Verify available space
df -h
```

---

## Customizing the Build

### Adding/Removing Packages

Edit `build/archlive/packages.x86_64`:

```bash
# Add package
echo "firefox" >> build/archlive/packages.x86_64
```

OR

```bash
# Remove package (edit manually)
vim build/archlive/packages.x86_64
```
You can use nano (pacman -S nano)
Then rebuild:

```bash
make clean
make build
```

### Changing the Kernel (not recommended)

Edit `build/archlive/profiledef.sh`:

```bash
# Default (latest stable)
kernel="linux"

# Long-term support
kernel="linux-lts"

# Hardened kernel
kernel="linux-hardened"
```

### Custom Branding

- Replace logo: `cp your-logo.png branding/archibald-logo.png`
- Modify colors in [branding/design-system.md](../branding/design-system.md)
- Customize Calamares theme: `calamares/branding/`

---

## Signing ISOs

### Generate GPG Key (if needed)

```bash
gpg --full-generate-key
# Follow prompts
gpg --list-secret-keys
```

### Sign ISO

```bash
# Using default key
make sign ISO=build/out/Archibald-Linux-SNAPSHOT-*.iso

# Or manually
bash scripts/sign-iso.sh build/out/Archibald-Linux-SNAPSHOT-*.iso
```

Produces `Archibald-Linux-SNAPSHOT-*.iso.asc` (detached signature)

### Verify Signature

```bash
gpg --verify build/out/Archibald-Linux-SNAPSHOT-*.iso.asc \
    build/out/Archibald-Linux-SNAPSHOT-*.iso
```

---

## Distribution

The Archibald Linux ISO is ready for distribution:

```
build/out/
├── Archibald-Linux-SNAPSHOT-20260521-x86_64.iso  (ISO image)
├── checksums.sha256                              (SHA256 checksums)
├── checksums.md5                                 (MD5 checksums)
└── Archibald-Linux-SNAPSHOT-20260521-x86_64.iso.asc  (GPG signature, if signed)
```

**For public release:**

1. Verify the ISO works in VirtualBox/QEMU
2. Sign with your GPG key
3. Calculate checksums (already done by build script)
4. Upload to your distribution platform (GitHub releases, website, mirror, etc.)
5. Publish checksums and signatures alongside the ISO
6. Document the release in `CHANGELOG.md`

---

## Advanced: CI/CD Integration

Future enhancements can automate builds via GitHub Actions, GitLab CI, or similar services. See [DEVELOPMENT.md](DEVELOPMENT.md) for roadmap.

---

## Support

For issues, see [DEVELOPMENT.md](DEVELOPMENT.md) for contributing guidelines or contact the Archibald Linux project.
