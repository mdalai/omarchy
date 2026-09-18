# Omarchy Quick Notes

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

---

## 3. Monitor Management (Hyprland)

Omarchy uses Hyprland with Lua configuration (`hyprland.lua`), which means legacy `hyprctl keyword` syntax is rejected. Monitor commands must be dispatched via `hyprctl eval` using Omarchy's `hl.monitor` Lua API.

### Check Connected Monitors
```bash
hyprctl monitors all
```
Key outputs to look for:
- Connector / port name (e.g. `HDMI-A-1`, `DP-1`)
- Description (e.g. `desc:LG Electronics LG QHD 112NTAB1B976`)
- Resolution & refresh rate (e.g. `1920x1080@60`)
- Position offset (e.g. `1920x0`)

### Temporarily Disable a Monitor
To disable a display output dynamically without changing configuration files (e.g. `HDMI-A-1`):
```bash
hyprctl eval 'hl.monitor({ output = "HDMI-A-1", disabled = true })'
```

### Re-Enable the Monitor
- **Option A (Recommended - reload saved configuration):**
  ```bash
  hyprctl reload
  ```
- **Option B (Dynamically re-apply monitor spec):**
  ```bash
  hyprctl eval 'hl.monitor({ output = "desc:LG Electronics LG QHD 112NTAB1B976", mode = "1920x1080@60", position = "1920x0", scale = 1 })'
  ```

### Turn Screen Off / On (DPMS Power Savings)
To blank or put a display to sleep without altering desktop layout / workspaces:
```bash
# Turn off
hyprctl dispatch dpms off HDMI-A-1

# Turn on
hyprctl dispatch dpms on HDMI-A-1
```

### Persistent Configuration
Permanent monitor settings are defined in:
- `~/.config/hypr/monitors.lua`

