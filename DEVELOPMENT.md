# Archibald Linux – Development Guide

## Overview

Archibald Linux welcomes contributions from the community! This guide covers development setup, contribution workflow, and project standards.

---

## Getting Started

### Fork & Clone Repository

```bash
# Clone the official repository (replace with your fork URL)
git clone https://github.com/YourUsername/ArchibaldLinux.git
cd ArchibaldLinux

# Add upstream remote
git remote add upstream https://github.com/ArchibaldLinux/ArchibaldLinux.git

# Create a feature branch
git checkout -b feature/your-feature-name
```

### Project Structure Review

See [README.md](../README.md) for complete project structure overview.

Key directories:
- `build/archlive/` — ISO build configuration
- `branding/` — Visual assets and guidelines
- `calamares/` — Installer configuration
- `scripts/` — Build automation
- `docs/` — User and developer documentation
- `kde-plasma/`, `ly-greeter/` — Environment customization

---

## Development Workflow

### 1. Create Feature Branch

```bash
# Always branch from main/master
git checkout main
git pull upstream main

# Create descriptive branch name
git checkout -b feature/calamares-theme-update
# or
git checkout -b bugfix/grub-boot-timeout
```

### 2. Make Changes

**For ISO/Build Changes:**
- Modify `build/archlive/packages.x86_64` (add/remove packages)
- Update `build/archlive/profiledef.sh` (if changing kernel, bootloader, etc.)
- Test with `make build`

**For Branding Changes:**
- Update assets in `branding/`
- Update `branding/design-system.md` if changing design guidelines
- Test installation and verify branding displays correctly

**For Documentation:**
- Update relevant `.md` files in `docs/`
- Ensure clarity and accuracy
- Check for markdown formatting issues

**For Calamares Customization:**
- Modify `calamares/settings.conf` or `calamares/branding/`
- Test installer behavior

### 3. Test Your Changes

**For Build System:**
```bash
# Build and test ISO
make clean
make build

# Test in VM
qemu-system-x86_64 -m 2G -cdrom build/out/Archibald-Linux-*.iso
```

**For Branding:**
- Boot ISO in VM
- Verify all branding elements display correctly
- Check across different resolutions if applicable

**For Documentation:**
- Verify markdown rendering
- Check links and file references
- Test code examples if applicable

### 4. Commit with Clear Messages

```bash
git add .

# Commit with descriptive message
git commit -m "Add custom GRUB theme with Archibald logo

- Update branding/grub-theme/theme.txt with primary colors
- Add high-res background image (1920x1440)
- Test on both UEFI and BIOS systems
- Verify logo alignment and sizing"
```

**Commit Message Guidelines:**
- First line: Short summary (50 chars max)
- Blank line
- Detailed explanation (wrap at 72 chars)
- Reference issues: "Fixes #123" or "Related to #456"

### 5. Push and Create Pull Request

```bash
# Push branch to your fork
git push origin feature/your-feature-name

# Visit GitHub and create Pull Request
# - Link any related issues
# - Describe your changes and why they're needed
# - Include testing steps
```

### 6. Code Review & Merge

- Maintainers review pull request
- Address feedback and update commits
- Upon approval, changes are merged to main

---

## Areas for Contribution

### High Priority

- **Branding Assets:** Wallpapers, icons, GRUB theme refinements
- **Documentation:** User guides, troubleshooting, API/architecture docs
- **Build Automation:** CI/CD pipeline setup (GitHub Actions, GitLab CI)
- **Package Optimization:** Test and optimize package list for specific use cases
- **Testing:** Test ISO on various hardware, report issues

### Medium Priority

- **Calamares Customization:** Improve installer UX, add custom modules
- **KDE Plasma Defaults:** Refine default settings and color schemes
- **Performance Tuning:** Optimize boot time, memory usage
- **Multilingual Support:** Translate documentation and UI

### Future Enhancements

- **Alternative Spins:** ARM builds, minimal server version
- **Custom Packages:** Archibald-branded utilities and tools
- **Community Mirror Network:** Distribute ISO via mirrors
- **Release Infrastructure:** Automated releases, changelog generation

---

## Build System Modifications

### Adding a Package

1. Edit `build/archlive/packages.x86_64`
2. Add package name (one per line)
3. Test build: `make clean && make build`
4. Verify package is installed: Boot ISO and run `pacman -Q package-name`

### Changing Bootloader

Edit `build/archlive/profiledef.sh`:

```bash
# Default (systemd-boot + GRUB fallback)
bootmodes=('uefi-x64.systemd-boot.esp' 'uefi-x64.grub.esp' 'bios.syslinux.mbr' 'bios.syslinux.eltorito')

# GRUB only
bootmodes=('uefi-x64.grub.esp' 'bios.syslinux.mbr')

# systemd-boot only
bootmodes=('uefi-x64.systemd-boot.esp')
```

### Customizing Pacman Configuration

Edit `build/archlive/pacman.conf` to:
- Add/remove repositories
- Change mirror selections
- Adjust download options

---

## Branding Contributions

### Creating New Wallpapers

**Requirements:**
- Resolution: 4K (3840×2160) + 1080p (1920×1080)
- Format: PNG (preferred) or JPEG
- Color: Use Archibald blue palette (see `branding/design-system.md`)
- File size: <10 MB for 4K, <3 MB for 1080p

**Submission:**
1. Place in `branding/wallpapers/`
2. Use descriptive filename: `desktop-archibald-variant-name-4k.png`
3. Update `branding/README.md` with description
4. Create pull request

### Updating Design System

**Before changing branding:**

1. Review `branding/design-system.md`
2. Propose changes via GitHub issue
3. Gather community feedback
4. Create pull request with:
   - Rationale for changes
   - Before/after examples
   - Impact on all branding touchpoints (installer, bootloader, desktop, etc.)

---

## Documentation Contributions

### Writing Documentation

**Guidelines:**
- Use clear, simple language
- Target audience: Ubuntu users transitioning to Arch
- Include examples and code snippets
- Link to related Arch Linux Wiki pages
- Use consistent formatting (see existing docs for style)

**Structure:**
- H1 title (`# Title`)
- Overview section
- Prerequisites/requirements (if applicable)
- Step-by-step instructions
- Troubleshooting section
- Link to related docs

**Example:**
```markdown
# Title

## Overview
Brief description of what this covers.

## Prerequisites
- Item 1
- Item 2

## Steps

### Step 1: Description
Explanation and example code.

### Step 2: Description
More details.

## Troubleshooting

### Problem
Solution.

## See Also
- [Related Doc](relative-link.md)
- [External Link](https://example.com)
```

---

## Continuous Integration (Future)

Plans for automated testing:

- **GitHub Actions:** Automatically build ISO on commits/PRs
- **Testing:** Verify ISO boots in QEMU, checksums match
- **Linting:** Check markdown, bash scripts for style issues
- **Changelog:** Auto-generate from commit messages

Interested in setting this up? Open an issue or contact maintainers!

---

## Code Style & Standards

### Shell Scripts

```bash
#!/usr/bin/env bash
set -euo pipefail

# Use meaningful function names
print_header() {
    echo "======================================="
    echo "$1"
    echo "======================================="
}

# Use consistent indentation (4 spaces)
if [[ -f "$file" ]]; then
    do_something
fi

# Quote variables
echo "Value: ${variable}"
```

### Markdown

- Use ATX-style headings (`#`, `##`, etc.)
- Consistent bullet list style (`-` or `*`)
- Code blocks with language specifier (` ```bash `)
- Line wrap at 80 characters (recommended)

### Git Commits

- One logical change per commit
- Atomic commits (buildable/testable state after each commit)
- Clear, descriptive messages
- Reference issues when applicable

---

## Communication

- **Issues:** Bug reports, feature requests, discussion
- **Pull Requests:** Code review, feedback, approval
- **Email:** Reach out to maintainers (if applicable)
- **Matrix/IRC:** Community chat (if set up)

---

## Code of Conduct

All contributors agree to:
- Be respectful and inclusive
- Welcome diverse perspectives
- Focus on the code, not personal criticism
- Report inappropriate behavior to maintainers

---

## Questions?

1. Check this guide and existing documentation
2. Search GitHub issues for similar questions
3. Open a new issue for clarification
4. Contact project maintainers

---

**Thank you for contributing to Archibald Linux! 🎉**

Together, we build a better, cleaner Linux distribution.
