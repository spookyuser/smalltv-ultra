# SmallTV-Ultra Tasmota Flash - Backup Information

**Date**: 2025-11-23
**Purpose**: Flash Tasmota firmware for experimentation

---

## Original Firmware Backup

### SmallTV-Ultra V9.0.31 (Latest Official)
- **Location**: `/home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin`
- **Size**: 509,792 bytes (498 KB)
- **MD5**: `b18f1a2abf43f6a24c5a6a4289789b7e`
- **Magic Byte**: `0xE9` ✓
- **Platform**: ESP8266
- **Version**: V9.0.31 (Released: 2025-02-11)
- **Last Fix**: Hour display bug fix

### Original Firmware Features
✅ Weather API integration (WeatherAPI.com, OpenWeatherMap)
✅ GIF playback with optimization
✅ Photo album theme (works offline)
✅ Simple weather theme
✅ Simple time theme (12/24 hour)
✅ Theme loop functionality
✅ Night mode
✅ WiFi AP mode: "GIFTV"
✅ Web interface at http://192.168.4.1
✅ OTA firmware update
✅ File system management

---

## Tasmota Firmware Information

### Downloaded Firmware
- **File**: `tasmota-display.bin`
- **Location**: `/home/user/smalltv-ultra/Tasmota-Experiment/tasmota-display.bin`
- **Size**: 630,784 bytes (616 KB)
- **MD5**: `84a37748d43ca0d75742e6b389428061`
- **Magic Byte**: `0xE9` ✓
- **Source**: http://ota.tasmota.com/tasmota/release/tasmota-display.bin
- **Platform**: ESP8266
- **Build**: Display variant (includes TFT/OLED drivers)

### Supported Display Controllers in Tasmota
✅ **ST7789** (Common in budget TFT displays)
✅ **ILI9341** (Very popular 240x320 TFT)
✅ **ILI9488** (320x480 TFT)
✅ **SSD1306** (OLED)
✅ **SSD1351** (Color OLED)
✅ **SSD1331** (Small color OLED)
✅ **RA8876** (Advanced display controller)
✅ And many more...

### Tasmota Features
✅ WiFi AP mode on first boot: "tasmota-XXXX"
✅ Web interface at http://192.168.4.1
✅ OTA firmware update (can flash back to SmallTV)
✅ Full GPIO configuration via web UI
✅ Display configuration via commands
✅ MQTT support
✅ Home Assistant integration
✅ Rules engine
✅ Timers and automation

---

## How to Recover (Flash Back to Original)

### Method 1: Via Tasmota Web Interface (Easiest)
1. Connect to Tasmota WiFi AP or web interface
2. Go to: **Firmware Upgrade**
3. Choose: `/home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin`
4. Click: **Start Upgrade**
5. Wait for reboot (15 seconds)
6. SmallTV-Ultra original firmware restored! ✓

### Method 2: Via OTA URL (If web accessible)
1. Access Tasmota console
2. Enter command: `OtaUrl http://[your-server]/FW-Smalltv-Ultra-V9.0.31.bin`
3. Enter command: `Upgrade 1`
4. Device downloads and flashes original firmware

### Method 3: Via Original Web Interface (If accessible)
If you can still access the SmallTV web interface:
1. Go to http://192.168.4.1/update
2. Upload original firmware
3. Reboot

---

## Important Notes

⚠️ **KEEP THIS FILE**: Save as reference during experimentation
⚠️ **MD5 Verification**: Always verify MD5 before flashing
⚠️ **WiFi Access**: Tasmota creates AP "tasmota-XXXX" if no WiFi configured
⚠️ **Display**: Will be blank until pins are configured correctly
⚠️ **Recovery**: Tasmota has built-in OTA, so recovery is always possible
⚠️ **Power**: Never unplug during firmware flash

---

## Hardware Information (To Be Discovered)

### Unknown (Need to Find):
- Display controller type: ST7789? ILI9341? Other?
- Display resolution: 240x320? 240x240? Other?
- GPIO pin mappings:
  - SPI MOSI (Data): ?
  - SPI CLK (Clock): ?
  - SPI CS (Chip Select): ?
  - SPI DC (Data/Command): ?
  - RST (Reset): ?
  - BL (Backlight): ?

### How to Discover:
1. Trial and error with Tasmota GPIO configuration
2. Visual inspection (open device, trace PCB)
3. Contact manufacturer
4. Community forums/documentation

---

## Validation Checks (Before Flash)

### Tasmota Binary Validation ✓
```
Magic Byte:    0xE9 ✓ (Valid ESP8266 firmware)
File Type:     DOS executable ✓
Platform:      ESP8266 ✓
Size:          616 KB ✓ (Fits in flash)
Display Build: YES ✓ (Has display drivers)
```

### SmallTV Backup Validation ✓
```
Magic Byte:    0xE9 ✓
MD5 Match:     b18f1a2abf43f6a24c5a6a4289789b7e ✓
File Intact:   YES ✓
```

---

## Next Steps After Flash

1. **Connect to Tasmota WiFi**: "tasmota-XXXX"
2. **Access Web Interface**: http://192.168.4.1
3. **Configure WiFi**: Add your home WiFi credentials
4. **Configure Display**: Try different GPIO pin combinations
5. **Test Display**: Use DisplayText commands
6. **Document Findings**: Record working pin configuration
7. **Decide**: Keep Tasmota or flash back to original

---

**Ready to flash!** 🚀
