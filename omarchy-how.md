d# Omarchy Quick Notes

## 1. Google Antigravity

### File Locations
- **Binaries & App:** `~/.local/share/antigravity/`
- **Terminal Command:** `~/.local/bin/antigravity`
- **App Launcher Menu:** `~/.local/share/applications/antigravity.desktop`
- **Icon:** `~/.local/share/icons/hicolor/512x512/apps/antigravity.png`
- **Config & Logs:** `~/.config/Antigravity/`
- **Installer Sources:** `~/Downloads/Antigravity*`

### Complete Uninstall / Cleanup
Run in terminal:
```bash
rm -rf ~/.local/share/antigravity ~/.local/bin/antigravity
rm -f ~/.local/share/applications/antigravity.desktop
rm -f ~/.local/share/icons/hicolor/512x512/apps/antigravity.png
rm -rf ~/.config/Antigravity
rm -rf ~/Downloads/Antigravity ~/Downloads/Antigravity.tar.gz
update-desktop-database ~/.local/share/applications
```

---

## 2. UEFI Dual-Boot Order

- **`0004`**: Omarchy Linux (Limine bootloader)
- **`0000`**: Windows Boot Manager

### Check Current Boot Order
```bash
efibootmgr
```

### Set Omarchy Linux as Default
```bash
sudo efibootmgr -o 0004,0000,0002,0001
```

### Revert to Windows as Default
```bash
sudo efibootmgr -o 0000,0004,0002,0001
```

### BIOS BBS Priorities (Auto-Boot Fix)
If Windows overrides boot order or `efibootmgr` resets:
1. Restart and spam **Del** to enter BIOS.
2. Go to the **Boot** tab.
3. Open **UEFI Hard Disk Drive BBS Priorities** (or *NVMe / Hard Drive BBS Priorities*).
4. Set **Limine (Linux)** to **Boot Option #1** (move Windows Boot Manager to #2).
