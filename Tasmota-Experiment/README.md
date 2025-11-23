# SmallTV-Ultra Tasmota Experiment

**Date**: 2025-11-23
**Status**: Ready to flash
**Risk Level**: 🟡 Low (Safe recovery available)

---

## What's This?

This folder contains everything needed to safely flash Tasmota firmware to your SmallTV-Ultra device and recover back to original firmware.

---

## Contents

| File | Purpose |
|------|---------|
| **tasmota-display.bin** | Tasmota firmware with display drivers (616 KB) |
| **FLASH-INSTRUCTIONS.md** | Step-by-step flashing guide ⭐ START HERE |
| **BACKUP-INFO.md** | Original firmware info & backup details |
| **DISPLAY-CONFIGURATION.md** | How to configure the display in Tasmota |
| **RECOVERY-PROCEDURE.md** | How to flash back to original firmware |
| **README.md** | This file |

---

## Quick Start

### 🚀 Ready to Flash?

1. **Read**: `FLASH-INSTRUCTIONS.md` (complete guide)
2. **Power on** SmallTV-Ultra
3. **Access** web interface: http://192.168.4.1
4. **Upload** `tasmota-display.bin`
5. **Wait** for reboot
6. **Connect** to WiFi AP: `tasmota-XXXXXX`
7. **Configure** via web interface

**Time needed**: 10-15 minutes
**Difficulty**: Easy

### ⚡ Quick Flash Commands

For SmallTV web interface:
- Go to: http://192.168.4.1
- Firmware Upgrade → Choose File
- Select: `tasmota-display.bin`
- Click: Update
- Wait 30 seconds

---

## What Happens After Flash?

### ✅ What Works
- WiFi (creates AP: `tasmota-XXXXXX`)
- Web interface
- OTA updates (can flash back!)
- GPIO configuration
- MQTT, timers, rules
- Home automation integration

### ❌ What Doesn't Work (Until Configured)
- Display (blank/black screen)
- Weather features
- GIF playback
- Theme system

### 🔧 To Make Display Work
See: `DISPLAY-CONFIGURATION.md`
- Configure GPIO pins
- Select display controller
- Test with DisplayText command

---

## Safety & Recovery

### Is It Safe?
✅ **YES** - Tasmota has built-in OTA recovery
✅ Can always flash back to original
✅ Won't brick device (bootloader protected)
✅ Original firmware backed up

### How to Recover?
See: `RECOVERY-PROCEDURE.md`

**Quick recovery:**
1. Access Tasmota web interface
2. Firmware Upgrade
3. Upload: `/home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin`
4. Wait for reboot
5. Original firmware restored! ✓

**Time**: 2-5 minutes

---

## File Verification

### Tasmota Firmware
```
File: tasmota-display.bin
Size: 630,784 bytes (616 KB)
MD5:  84a37748d43ca0d75742e6b389428061
Magic Byte: 0xE9 ✓
Platform: ESP8266 ✓
```

### Original Firmware (Backup)
```
File: ../Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin
Size: 509,792 bytes (498 KB)
MD5:  b18f1a2abf43f6a24c5a6a4289789b7e
Magic Byte: 0xE9 ✓
Platform: ESP8266 ✓
```

---

## Why Tasmota?

### Advantages
- ✅ Open source
- ✅ Active community
- ✅ Extensive documentation
- ✅ Display driver support
- ✅ Home automation ready
- ✅ MQTT built-in
- ✅ Web-based configuration
- ✅ OTA updates
- ✅ Highly customizable

### Disadvantages
- ❌ No weather features (unless custom coded)
- ❌ No GIF playback
- ❌ Display needs configuration
- ❌ Different from original interface
- ❌ Lose SmallTV themes

---

## Use Cases

### 1. Learning & Experimentation
- Understand ESP8266 hardware
- Discover GPIO pin mappings
- Learn display configuration
- Practice firmware flashing

### 2. IoT Integration
- MQTT sensor display
- Home Assistant dashboard
- Custom automation display
- Smart home controller

### 3. Custom Firmware Development
- Use as base for custom firmware
- Learn Tasmota codebase
- Develop custom features
- Share templates

### 4. Hardware Reverse Engineering
- Discover pin mappings
- Identify display controller
- Document hardware specs
- Create schematics

---

## Supported Display Controllers

Tasmota-display.bin includes drivers for:

- **ST7789** (common budget TFT - 240x240, 240x320)
- **ILI9341** (very popular - 240x320)
- **ILI9342** (320x240)
- **ILI9488** (320x480)
- **SSD1306** (OLED - 128x64)
- **SSD1351** (color OLED - 128x128)
- **SSD1331** (small color OLED)
- Many more...

---

## Next Steps After Flash

### Option A: Configure Display
1. Read: `DISPLAY-CONFIGURATION.md`
2. Configure GPIO pins
3. Select display controller
4. Test display output
5. Document working config
6. Share with community

### Option B: Use as IoT Device
1. Configure WiFi
2. Set up MQTT
3. Integrate with Home Assistant
4. Create automation rules
5. Control via web/app

### Option C: Flash Back
1. Read: `RECOVERY-PROCEDURE.md`
2. Access Tasmota web interface
3. Upload original firmware
4. Reboot
5. Back to SmallTV features

---

## Resources

### Tasmota Documentation
- **Main Docs**: https://tasmota.github.io/docs/
- **Display Guide**: https://tasmota.github.io/docs/Displays/
- **Commands**: https://tasmota.github.io/docs/Commands/
- **Templates**: https://templates.blakadder.com/

### Tasmota Community
- **GitHub**: https://github.com/arendst/Tasmota
- **Discussions**: https://github.com/arendst/Tasmota/discussions
- **Discord**: https://discord.gg/Ks2Kzd4

### ESP8266 Resources
- **Datasheet**: Search "ESP8266 datasheet"
- **Forums**: https://www.esp8266.com/
- **Reddit**: r/esp8266

### SmallTV Resources
- **Repository**: /home/user/smalltv-ultra/
- **Manual**: ../Latest GeekMagic SmallTV-Ultra User Manual V9.0.30.pdf
- **Original Firmware**: ../Ultra-V9.0.31/

---

## Troubleshooting

| Issue | Solution | Reference |
|-------|----------|-----------|
| Flash fails | Verify MD5, try again | FLASH-INSTRUCTIONS.md |
| Can't find WiFi | Wait 2 min, power cycle | FLASH-INSTRUCTIONS.md |
| Display blank | Normal! Configure pins | DISPLAY-CONFIGURATION.md |
| Want original back | Flash recovery firmware | RECOVERY-PROCEDURE.md |
| Device won't boot | Power cycle, check power | RECOVERY-PROCEDURE.md |

---

## Experiment Log

Use this space to document your findings:

### GPIO Pin Configuration (Fill in when discovered)
```
MOSI (Data):  GPIO ?? → Display SDI/SDA
CLK (Clock):  GPIO ?? → Display SCK/CLK
CS (Chip Select): GPIO ?? → Display CS
DC (Data/Command): GPIO ?? → Display DC/RS
RST (Reset):  GPIO ?? → Display RST
BL (Backlight): GPIO ?? → Display BL/LED
```

### Display Controller (Fill in when discovered)
```
Type: ??? (ST7789, ILI9341, etc.)
Resolution: ??? x ???
Tasmota Model Number: ???
```

### Working Configuration
```
DisplayModel ???
DisplayWidth ???
DisplayHeight ???
GPIO Configuration: (paste from Tasmota)
```

---

## Changelog

### 2025-11-23 - Initial Setup
- Downloaded tasmota-display.bin (616 KB)
- MD5 verified: 84a37748d43ca0d75742e6b389428061
- Created documentation
- Prepared for flash
- Ready to experiment

---

## Notes

- Keep this folder for reference during experimentation
- Document your findings in Experiment Log above
- Share successful configurations with community
- Always verify MD5 before flashing
- Don't panic - recovery is easy!

---

**Have fun experimenting!** 🎉

Remember: You can ALWAYS go back to original firmware. The device is safe! 🛡️

---

**Questions?** Check the documentation files or Tasmota community resources.

**Found the working display config?** Please share it with the community! 🙏
