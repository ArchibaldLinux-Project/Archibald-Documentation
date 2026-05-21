# Archibald Linux – Branding Guidelines

## Overview

This document provides guidelines for maintaining consistent branding across Archibald Linux. Use these standards when creating new assets, modifying branding, or developing features visible to end users.

---

## Brand Identity

**Name:** Archibald Linux  
**Tagline:** Clean. Minimal. Yours.  
**Target Audience:** Users who want a streamlined Arch Linux experience without bloatware  

### Brand Values

- **Clean:** Minimal, purposeful design. No unnecessary UI clutter.
- **Transparent:** Clear communication, open development process.
- **User-Centric:** Respects user choice and customization.
- **Lightweight:** Fast, efficient, low resource overhead.

---

## Visual Identity

For complete color specifications and design rules, see [branding/design-system.md](../branding/design-system.md).

### Color Palette Quick Reference

| Color | Hex Code | RGB | Usage |
|-------|----------|-----|-------|
| **Archibald Blue** | `#0066CC` | 0, 102, 204 | Primary actions, highlights |
| **Archibald Dark** | `#1A1A2E` | 26, 26, 46 | Backgrounds, text on light |
| **Archibald Light** | `#F0F2F5` | 240, 242, 245 | Light backgrounds, text on dark |
| **Accent Green** | `#00CC66` | 0, 204, 102 | Success, positive feedback |
| **Accent Orange** | `#FF9900` | 255, 153, 0 | Warnings, secondary CTAs |
| **Accent Red** | `#CC3333` | 204, 51, 51 | Errors, destructive actions |

### Logo Usage

**File Location:** `branding/archibald-logo.png` (and variants)

**Usage Rules:**

- ✅ Use official logo files only
- ✅ Maintain aspect ratio (never squish or stretch)
- ✅ Use on solid backgrounds with sufficient contrast
- ❌ Don't rotate, skew, or apply effects
- ❌ Don't use low-resolution versions (minimum 64×64px)
- ❌ Don't alter colors (use provided color variants instead)

**Placement Examples:**

- **Installer splash screen:** Center top, above logo
- **GRUB bootloader:** Top-left corner
- **KDE Plasma panel:** Taskbar/application menu
- **Welcome screen:** Center, large size
- **Documentation:** Header/footer, small-medium size

---

## Asset Organization

All branding assets are stored in [branding/](../branding/):

```
branding/
├── archibald-logo.png          # Primary full-color logo
├── design-system.md             # Complete design system
│
├── wallpapers/
│   ├── desktop-dark-4k.png      # Desktop background (3840×2160)
│   ├── desktop-dark-1080p.png   # Desktop background (1920×1080)
│   ├── lock-screen-4k.png       # Lock screen (3840×2160)
│   └── lock-screen-1080p.png    # Lock screen (1920×1080)
│
├── icons/
│   ├── archibald-logo-mark.png  # Logo symbol only (512×512)
│   ├── archibald-mono-black.png # Monochrome black version (512×512)
│   ├── archibald-mono-white.png # Monochrome white version (512×512)
│   └── README.md                # Icon guidelines
│
└── grub-theme/
    ├── theme.txt                # GRUB2 theme configuration
    ├── background.png           # GRUB background image
    └── README.md                # GRUB customization guide
```

---

## Component Branding

### Calamares Installer

**Location:** `calamares/branding/`

**Visual Elements:**

- **Sidebar:** Dark background (Archibald Dark) with blue accent bar
- **Logo:** Archibald logo displayed at top
- **Buttons:** Primary actions use Archibald Blue (#0066CC)
- **Progress bar:** Blue with rounded ends
- **Text:** Light text on dark backgrounds

**Customization Points:**

- `branding/branding.desc` — Main configuration
- `branding/qml/` — Custom QML UI layouts (if needed)
- Images: Place 1920×1440 or 1024×768 images in `branding/`

---

### GRUB Bootloader

**Location:** `branding/grub-theme/`

**Visual Elements:**

- **Background:** Dark gradient (Archibald Dark to black)
- **Text:** Archibald Light (off-white)
- **Selection highlight:** Archibald Blue
- **Logo:** Display Archibald mark in top-left or center

**Configuration:**

Edit `branding/grub-theme/theme.txt`:

```
# Example GRUB theme configuration
title_color="#F0F2F5"           # Archibald Light
message_color="#F0F2F5"
highlight_color="#0066CC"       # Archibald Blue
```

---

### ly Greeter (Login Screen)

**Location:** `ly-greeter/config.ini`

**Visual Elements:**

- **Background:** Dark (Archibald Dark or subtle blue gradient)
- **Text input:** Subtle border color on focus (Archibald Blue)
- **Username/password labels:** Archibald Light
- **Optional:** Logo or minimal ASCII art (keep subtle)

**Configuration:**

```ini
[Appearance]
bg = 0x1A1A2E           # Archibald Dark
fg = 0xF0F2F5           # Archibald Light
hl = 0x0066CC           # Archibald Blue (highlight)
message = "Welcome to Archibald Linux"
```

---

### KDE Plasma

**Location:** `kde-plasma/`

**Visual Elements:**

- **Color Scheme:** Custom Archibald colors
- **Wallpaper:** Archibald Linux wallpaper from `branding/wallpapers/`
- **Panel/Taskbar:** Minimal, matching color scheme
- **Icons:** Standard Breeze icons with Archibald color accents (if customized)

**Setup:**

1. Create custom KDE color scheme in `~/.local/share/color-schemes/Archibald.colors`
2. Apply via: System Settings → Appearance → Global Themes
3. Set wallpaper via: System Settings → Startup and Shutdown → Desktop

---

## Creating New Branding Assets

### Wallpapers

**Requirements:**

- **Dimensions:**
  - Desktop: 4K (3840×2160), 1080p (1920×1080)
  - Lock screen: Same as desktop
  - Mobile: 1080×1920 (optional)
  
- **File Format:** PNG (no compression loss), or JPEG (compressed)

- **Style Guidelines:**
  - Use Archibald blue as primary color (can be subtle)
  - Modern, minimal aesthetic
  - No text/watermarks (optional Archibald logo, very subtle)
  - High contrast between background and any overlaid text

### Icons

**Requirements:**

- **Dimensions:** 512×512px minimum (will be scaled down)
- **File Format:** PNG with transparency, or SVG
- **Color Variants:**
  - Full color (blue primary)
  - Monochrome black (for light backgrounds)
  - Monochrome white (for dark backgrounds)

- **Style:**
  - Minimal line-based design (2px strokes)
  - Clear at small sizes (64×64, 32×32)
  - Consistent with Breeze icon set (KDE standard)

### Logos

**Logo Mark (Symbol Only)**

- Simple, recognizable shape
- Works at 64×64px minimum
- Scalable without loss of clarity

**Full Logo (Mark + Text)**

- Horizontal layout recommended
- Minimum 200×64px
- Clear spacing between mark and text

---

## Testing Branding

### Branding Checklist

- [ ] Logo displays correctly across all resolutions
- [ ] Colors are consistent across wallpapers, installer, bootloader, and desktop
- [ ] Text contrast meets accessibility standards (WCAG AA minimum)
- [ ] Installer displays logo and custom branding
- [ ] Bootloader shows custom theme and logo
- [ ] KDE Plasma desktop uses correct wallpaper and colors
- [ ] ly greeter displays custom styling and optional ASCII art
- [ ] No pixelation or distortion of images
- [ ] All branding visible in both light and dark modes (if applicable)

### Quality Standards

**Resolution:** All images at intended dimensions, no upscaling artifacts  
**Color Accuracy:** Test on multiple monitors/displays  
**Accessibility:** Sufficient contrast for readability  
**Consistency:** Brand elements recognizable across all components  

---

## Future Enhancements

- **Icon Theme:** Full system icon theme with Archibald identity
- **Boot Animation:** Optional splash animation (performance trade-off)
- **Documentation Branding:** Consistent headers, footers, styling
- **Community Variants:** Secondary color schemes (e.g., "Archibald Dark", "Archibald Light")
- **Localization:** Multi-language support for installer and UI text

---

## Support

For questions about branding:

1. Review [branding/design-system.md](../branding/design-system.md) for complete specifications
2. Check existing assets for style reference
3. Propose changes via pull request with examples and rationale

---

**Maintain the visual identity. Keep it clean, minimal, and distinctive. 🎨**
