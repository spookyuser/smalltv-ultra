# Recovery Procedure - Flash Back to SmallTV Original Firmware

This guide explains how to restore your SmallTV-Ultra to the original firmware after flashing Tasmota.

---

## When to Use This Guide

- ✅ You want to go back to original SmallTV features
- ✅ You want weather/GIF/theme functionality back
- ✅ Display configuration in Tasmota is too difficult
- ✅ You finished experimenting with Tasmota
- ✅ Something went wrong and you want to start fresh
- ✅ You prefer the original interface

---

## Recovery Method 1: Via Tasmota Web Interface (Easiest)

### Prerequisites
- Tasmota is running and accessible
- You can access Tasmota web interface
- Original firmware file available

### Steps

1. **Access Tasmota Web Interface**
   - Connect to device WiFi or same network
   - Open browser to: `http://[tasmota-ip]`
   - Login if password was set

2. **Navigate to Firmware Upgrade**
   - Click: **Firmware Upgrade**
   - You'll see the upgrade page

3. **Upload Original Firmware**
   - Click: **Choose File** (under "Use file upload")
   - Select: `/home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin`
   - Verify: File size shows ~498 KB
   - Click: **Start Upgrade**

4. **Wait for Flash Process**
   ```
   Expected sequence:
   1. Upload progress (0-100%)
   2. Validation (Magic byte check)
   3. Flash write (10-30 seconds)
   4. "Upload Successful" message
   5. Device reboots automatically
   ```

5. **Verify Recovery**
   - Wait 30-60 seconds for reboot
   - Display should show SmallTV boot animation
   - Connect to "GIFTV" WiFi (if needed)
   - Access: http://192.168.4.1
   - Verify SmallTV interface is back

**🎉 RECOVERY COMPLETE! 🎉**

---

## Recovery Method 2: Via Tasmota OTA URL

### When to Use
- Web upload is failing
- You have original firmware hosted on a web server
- You want to use command line

### Steps

1. **Access Tasmota Console**
   - Go to: Main Menu → **Console**

2. **Set OTA URL**
   ```
   OtaUrl http://[your-server-ip]/FW-Smalltv-Ultra-V9.0.31.bin
   ```

3. **Start Upgrade**
   ```
   Upgrade 1
   ```

4. **Wait for Download and Flash**
   - Device downloads firmware
   - Validates
   - Flashes
   - Reboots automatically

5. **Verify Recovery**
   - Display shows SmallTV interface
   - Features restored

---

## Recovery Method 3: Via Original Interface (If Accessible)

### When to Use
- SmallTV interface is still partially accessible
- Tasmota web interface not working properly

### Steps

1. **Access Device**
   - Connect to device WiFi
   - Go to: http://192.168.4.1

2. **Find Update Option**
   - Look for firmware update section

3. **Upload Firmware**
   - Select: `FW-Smalltv-Ultra-V9.0.31.bin`
   - Click: Update
   - Wait for completion

---

## Recovery Method 4: Emergency UART Recovery (Advanced)

### When to Use
- Device won't boot
- No WiFi access
- Web interface not accessible
- Last resort only

### Prerequisites
- USB-to-Serial adapter (FTDI, CH340, CP2102, etc.)
- Access to device internal pins
- esptool.py or similar flash tool
- Some electronics knowledge

### Equipment Needed
- USB-to-Serial adapter (3.3V logic!)
- Jumper wires
- Screwdriver to open case
- Computer with Python/esptool

### Steps

1. **Open SmallTV Case**
   - Carefully remove screws
   - Open case to access PCB

2. **Identify UART Pins on ESP8266**
   - GND (Ground)
   - TX (Transmit)
   - RX (Receive)
   - 3.3V (Power - optional)
   - GPIO0 (for boot mode)

3. **Connect USB-to-Serial Adapter**
   ```
   Adapter    →    ESP8266
   -------         -------
   GND       →     GND
   TX        →     RX
   RX        →     TX
   3.3V      →     3.3V (optional, if not powered)
   ```

4. **Enter Flash Mode**
   - Connect GPIO0 to GND
   - Power on device (or press reset)
   - GPIO0 can be disconnected after boot

5. **Flash Firmware**
   ```bash
   # Install esptool if needed
   pip install esptool

   # Find serial port
   # Linux: /dev/ttyUSB0 or /dev/ttyACM0
   # Mac: /dev/cu.usbserial-*
   # Windows: COM3, COM4, etc.

   # Erase flash (optional but recommended)
   esptool.py --port /dev/ttyUSB0 erase_flash

   # Flash firmware
   esptool.py --port /dev/ttyUSB0 write_flash 0x00000 \
     /home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin
   ```

6. **Disconnect and Reboot**
   - Disconnect GPIO0 from GND
   - Disconnect serial adapter
   - Power cycle device
   - Should boot normally

**⚠️ WARNING**: This method requires hardware access and can damage device if done incorrectly!

---

## Troubleshooting Recovery

### Problem: "Magic Byte" Error During Recovery

**Cause**: Wrong file or corrupted download

**Solution**:
```bash
# Verify original firmware MD5
md5sum /home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin

# Should output:
# b18f1a2abf43f6a24c5a6a4289789b7e

# If different, re-download from repository
```

---

### Problem: Upload Fails at 50% or 75%

**Cause**: Network interruption or browser timeout

**Solution**:
1. Try different browser (Chrome, Firefox, Edge)
2. Move closer to device (if using WiFi)
3. Disable browser extensions
4. Try Method 2 (OTA URL) instead
5. Power cycle device and try again

---

### Problem: Device Boots Tasmota After "Successful" Flash

**Cause**: Flash didn't actually complete or wrong partition

**Solution**:
1. Try again - sometimes needs 2 attempts
2. Use Method 2 (OTA URL)
3. Erase flash first (console command):
   ```
   Reset 5  # Factory reset
   ```
   Then flash original firmware again

---

### Problem: Device Won't Boot After Recovery

**Cause**: Power loss during flash or corrupted flash

**Solution**:
1. Power cycle (unplug 10 seconds, replug)
2. Wait 2 minutes for boot
3. Try recovery process again
4. If still fails: UART recovery (Method 4)

---

### Problem: Display Shows Garbage After Recovery

**Cause**: Incomplete flash or config remnants

**Solution**:
1. Flash original firmware again
2. Power cycle device
3. Factory reset via web interface (if accessible)
4. Check MD5 of firmware file

---

### Problem: Can't Access Tasmota to Start Recovery

**Cause**: Lost IP address or WiFi configuration

**Solution**:

**Option A: Find Tasmota IP**
```bash
# Scan network
nmap -sn 192.168.1.0/24 | grep -i tasmota

# Or use phone app:
# - Fing (iOS/Android)
# - Network Analyzer (Android)
```

**Option B: Reset Tasmota WiFi**
1. Power on device
2. Press and hold reset button (if accessible) for 10 seconds
3. Tasmota creates AP: `tasmota-XXXXXX`
4. Connect and access http://192.168.4.1

**Option C: UART Recovery**
Use Method 4 (Emergency UART)

---

## Verification After Recovery

After successful recovery, verify:

### ✅ Checklist

- [ ] Display shows boot animation/logo
- [ ] Display shows time or weather
- [ ] WiFi AP "GIFTV" appears (if not configured)
- [ ] Can access http://192.168.4.1
- [ ] SmallTV web interface loads
- [ ] Can upload images/GIFs
- [ ] Weather works (if configured)
- [ ] Themes work
- [ ] All original features functional

---

## After Recovery: What's Different?

### What's Back ✅
- Weather display
- GIF playback
- Photo album mode
- Theme system
- SmallTV web interface
- Original WiFi AP name (GIFTV)

### What's Gone ❌
- Tasmota features (MQTT, rules, etc.)
- GPIO configuration
- Tasmota templates
- Home Assistant integration

### Fresh Start
- Device is back to original state
- Previous Tasmota config erased
- Can experiment again if desired

---

## Preventing Issues

### Before Flashing Anything:

1. **Verify MD5**
   ```bash
   md5sum [firmware-file]
   ```

2. **Check File Size**
   - Original: ~498 KB (509,792 bytes)
   - Tasmota: ~616 KB (630,784 bytes)

3. **Ensure Power**
   - Use reliable power source
   - Don't flash on battery
   - Avoid power interruptions

4. **Backup First**
   - Document original settings
   - Save firmware file
   - Note WiFi credentials

---

## Emergency Contact Info

### If All Else Fails:

1. **GeekMagic Support**
   - Check official website for support
   - Contact manufacturer
   - RMA if under warranty

2. **Community Help**
   - Tasmota Discord/Forums
   - ESP8266 communities
   - Reddit: r/esp8266, r/tasmota

3. **Professional Repair**
   - Local electronics repair shop
   - ESP8266 specialists
   - Last resort: replacement

---

## Quick Recovery Command Reference

### Tasmota Console Commands

```bash
# Check firmware version
Status 2

# Factory reset Tasmota
Reset 5

# Restart device
Restart 1

# OTA upgrade
OtaUrl http://[server]/firmware.bin
Upgrade 1

# Check WiFi
Status 5
```

### UART Flash Commands

```bash
# Check connection
esptool.py --port /dev/ttyUSB0 flash_id

# Erase flash
esptool.py --port /dev/ttyUSB0 erase_flash

# Flash firmware
esptool.py --port /dev/ttyUSB0 write_flash 0x00000 firmware.bin

# Read flash (backup)
esptool.py --port /dev/ttyUSB0 read_flash 0 0x400000 backup.bin
```

---

## Files You Need

All necessary files are in repository:

```
/home/user/smalltv-ultra/
├── Ultra-V9.0.31/
│   ├── FW-Smalltv-Ultra-V9.0.31.bin ← Original firmware
│   ├── md5.txt                       ← Checksum verification
│   └── update_history.txt            ← Version history
└── Tasmota-Experiment/
    ├── BACKUP-INFO.md                ← Backup documentation
    ├── FLASH-INSTRUCTIONS.md         ← Flash guide
    ├── DISPLAY-CONFIGURATION.md      ← Display setup
    └── RECOVERY-PROCEDURE.md         ← This file
```

---

## Summary

**Easiest Recovery Path:**
1. Access Tasmota web interface
2. Firmware Upgrade
3. Upload `FW-Smalltv-Ultra-V9.0.31.bin`
4. Wait for reboot
5. Done! ✓

**Time Required:** 2-5 minutes
**Difficulty:** Easy
**Risk:** Very low (can't brick via OTA)

---

**Don't panic!** Recovery is straightforward with Tasmota's built-in OTA capability. 🛡️

The device can always be restored to original firmware! 💪
