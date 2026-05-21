# Archibald Linux – Release Procedure

This document outlines the process for creating and distributing Archibald Linux releases.

---

## Release Types

### Snapshots (Current Development)

- **Frequency:** As-needed during active development
- **Naming:** `SNAPSHOT-YYYYMMDD` (e.g., `SNAPSHOT-20260521`)
- **Retention:** 30 days (old snapshots cleaned periodically)
- **Stability:** Development, may contain bugs
- **Use Case:** Early testing, community feedback, rapid iteration

### Stable Releases (Future)

- **Frequency:** Quarterly or as-planned
- **Naming:** `v1.0.0`, `v1.1.0`, etc. (semantic versioning)
- **Retention:** Long-term (2+ years)
- **Stability:** Tested, production-ready
- **Support:** Bug fixes, security updates for defined period

---

## Pre-Release Checklist

Before building a snapshot or stable release:

- [ ] All planned features completed and merged
- [ ] Documentation updated (BUILD.md, INSTALLATION.md, etc.)
- [ ] CHANGELOG.md updated with new features/fixes
- [ ] Package list reviewed and tested
- [ ] Branding assets finalized and tested
- [ ] Git repository clean (all changes committed)
- [ ] No known critical bugs

---

## Release Process

### Step 1: Prepare Release

```bash
# Update version in build profile
scripts/version-bump.sh

# Update CHANGELOG.md
nano CHANGELOG.md

# Commit changes
git add CHANGELOG.md build/archlive/profiledef.sh
git commit -m "Release: Prepare snapshot for SNAPSHOT-YYYYMMDD"
```

### Step 2: Build ISO

```bash
# Clean previous builds
make clean

# Build new ISO
make build

# Verify output
ls -lh build/out/Archibald-Linux-SNAPSHOT-*.iso
cat build/out/checksums.sha256
```

### Step 3: Test Release ISO

```bash
# Boot in QEMU (UEFI)
qemu-system-x86_64 -m 2G -enable-kvm \
  -bios /usr/share/OVMF/OVMF_CODE.fd \
  -cdrom build/out/Archibald-Linux-SNAPSHOT-*.iso

# Test installation workflow
# Test branding (wallpaper, colors, installer theme)
# Verify no extra packages are included
# Confirm post-install environment works
```

### Step 4: Sign Release

```bash
# Sign ISO with your GPG key
make sign ISO=build/out/Archibald-Linux-SNAPSHOT-*.iso

# Verify signature
gpg --verify build/out/Archibald-Linux-SNAPSHOT-*.iso.asc
```

### Step 5: Tag Release

```bash
# Create git tag
RELEASE_NAME="snapshot-20260521"
git tag -a "$RELEASE_NAME" -m "Release: Archibald Linux $RELEASE_NAME

Snapshot release with complete build system and documentation.

Changes: See CHANGELOG.md

Build artifacts:
- ISO: Archibald-Linux-SNAPSHOT-20260521-x86_64.iso
- Checksums: SHA256, MD5
- Signature: GPG detached signature"

# Push tag to repository
git push upstream "$RELEASE_NAME"
```

### Step 6: Upload Release

**Option A: GitHub Releases**

```bash
# Install github-cli (gh)
sudo pacman -S github-cli

# Create release
gh release create "$RELEASE_NAME" \
    build/out/Archibald-Linux-SNAPSHOT-*.iso \
    build/out/checksums.sha256 \
    build/out/checksums.md5 \
    build/out/Archibald-Linux-SNAPSHOT-*.iso.asc \
    -t "Archibald Linux $RELEASE_NAME" \
    -n "$(cat CHANGELOG.md | head -30)"
```

**Option B: Manual Upload (to website/mirror)**

```bash
# Create release directory
mkdir -p /path/to/releases/$RELEASE_NAME
cp build/out/Archibald-Linux-SNAPSHOT-*.iso* /path/to/releases/$RELEASE_NAME/
cp build/out/checksums.* /path/to/releases/$RELEASE_NAME/

# Upload via SCP, rsync, or web interface
rsync -avz /path/to/releases/$RELEASE_NAME/ user@mirror.example.com:/releases/
```

### Step 7: Announce Release

- [ ] Update website with download link
- [ ] Post on community forums/Reddit (if applicable)
- [ ] Announce via email list (if applicable)
- [ ] Update GitHub releases page
- [ ] Social media (Twitter, Mastodon, etc.)

---

## Distribution Artifacts

A complete release includes:

```
releases/SNAPSHOT-20260521/
├── Archibald-Linux-SNAPSHOT-20260521-x86_64.iso
├── Archibald-Linux-SNAPSHOT-20260521-x86_64.iso.asc
├── checksums.sha256
├── checksums.md5
├── RELEASE_NOTES.md
└── README.md
```

### Release Notes Template

```markdown
# Archibald Linux SNAPSHOT-YYYYMMDD

**Release Date:** YYYY-MM-DD

## Overview

Brief summary of what's new and notable.

## What's New

- Feature 1
- Feature 2
- Bugfix 1

## Download

[Download ISO](url-to-iso)

### Verification

Verify ISO integrity before use:

\`\`\`bash
sha256sum -c checksums.sha256

# Verify signature
gpg --verify Archibald-Linux-SNAPSHOT-*.iso.asc
\`\`\`

### System Requirements

- 2GB RAM (minimum)
- 20GB disk space
- x86_64 processor
- UEFI or BIOS boot

## Known Issues

- Issue 1: Workaround X
- Issue 2: Workaround Y

## What's Next

- Planned feature 1
- Planned feature 2

## Support

- [Installation Guide](../docs/INSTALLATION.md)
- [Build Instructions](../docs/BUILD.md)
- [GitHub Issues](https://github.com/ArchibaldLinux/ArchibaldLinux/issues)
```

---

## Version Maintenance

### Snapshot Cleanup

```bash
# List releases older than 30 days
find /path/to/releases -maxdepth 1 -type d -mtime +30

# Remove old snapshots (archive first if desired)
find /path/to/releases -maxdepth 1 -type d -mtime +30 -exec rm -rf {} \;
```

### Stable Release Support

For stable releases (v1.0+), define support window:

| Version | Release Date | Support End | Status |
|---------|--------------|-------------|--------|
| v1.0    | TBD          | TBD         | N/A    |
| v1.1    | TBD          | TBD         | N/A    |

- **Active:** Receive bug fixes and security updates
- **LTS (Long-term Support):** Extended support window (2-3 years)
- **EOL (End of Life):** No further updates

---

## Troubleshooting Releases

### ISO Doesn't Boot

```bash
# Verify integrity
sha256sum build/out/Archibald-Linux-SNAPSHOT-*.iso

# Re-build from scratch
make clean
make build
```

### Signature Verification Fails

```bash
# Ensure GPG key is in keyring
gpg --list-keys

# Import key if needed
gpg --import /path/to/key.asc

# Try verification again
gpg --verify Archibald-Linux-SNAPSHOT-*.iso.asc
```

### Checksum Mismatch

```bash
# Regenerate checksums
cd build/out/
sha256sum Archibald-Linux-SNAPSHOT-*.iso > checksums.sha256
md5sum Archibald-Linux-SNAPSHOT-*.iso > checksums.md5
```

---

## Future Improvements

- [ ] Automated CI/CD release pipeline (GitHub Actions)
- [ ] Mirror network for distributed downloads
- [ ] Torrent distribution
- [ ] Docker container for reproducible builds
- [ ] Signed Git commits for release tags
- [ ] Auto-generated release notes from commits
- [ ] Beta/RC release process

---

## References

- [Arch Linux Release Standards](https://wiki.archlinux.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
