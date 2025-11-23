# Tasmota Flash Instructions for SmallTV-Ultra

**⚠️ READ COMPLETELY BEFORE STARTING ⚠️**

---

## Pre-Flight Checklist ✅

Before you begin, ensure:

- [ ] SmallTV-Ultra is powered on and working
- [ ] You can access the SmallTV web interface at http://192.168.4.1
- [ ] Original firmware backup documented (see BACKUP-INFO.md)
- [ ] Tasmota binary downloaded and verified
- [ ] You understand you'll lose weather/GIF features temporarily
- [ ] You're prepared for trial-and-error display configuration

---

## Step-by-Step Flash Procedure

### **Step 1: Access SmallTV Web Interface**

1. Power on your SmallTV-Ultra
2. Connect to WiFi:
   - Option A: Connect to your home WiFi (if already configured)
   - Option B: Connect to "GIFTV" access point (if in AP mode)
3. Open browser and go to: **http://192.168.4.1**
4. You should see the SmallTV web interface

---

### **Step 2: Navigate to Firmware Update Page**

1. Look for **"Firmware Upgrade"** or **"Update"** option
2. Click on it
3. You should see a page with:
   - "Choose firmware file"
   - File upload button
   - "Update" button

---

### **Step 3: Upload Tasmota Firmware**

1. Click **"Choose file"** or **"Browse"**
2. Navigate to: `/home/user/smalltv-ultra/Tasmota-Experiment/`
3. Select: **`tasmota-display.bin`**
4. **VERIFY**: File shows as `tasmota-display.bin` (616 KB)
5. Click **"Update"** or **"Start Upgrade"**

---

### **Step 4: Wait for Flash Process**

```
Expected sequence:
1. Upload progress (0-100%)
2. Validation checks
   ✓ Magic byte: 0xE9
   ✓ Platform: ESP8266
   ✓ Size: OK
3. Flash write (may take 10-30 seconds)
4. Success message: "Update Success! Rebooting..."
5. Auto-redirect after 15 seconds
```

**⚠️ DO NOT:**
- Unplug the device
- Close the browser
- Press any buttons
- Interrupt the process

**IF YOU SEE ERRORS:**
- "Magic Byte error" → Wrong file, use tasmota-display.bin for ESP8266
- "Out of space" → File too large (shouldn't happen with 616KB file)
- "MD5 verification failed" → Re-download tasmota-display.bin
- Other errors → See TROUBLESHOOTING section below

---

### **Step 5: First Boot with Tasmota**

After reboot (about 30-60 seconds):

**What you'll see on SmallTV:**
- 📺 Display: **BLANK/BLACK** (expected - not configured yet)
- 💡 Power LED: **ON** (device is running)
- 📡 WiFi: Broadcasting new AP

**What to do:**

1. **Disconnect** from "GIFTV" (if you were connected to it)
2. **Look for new WiFi network**: `tasmota-XXXXXX`
   - XXXXXX = Last 6 digits of MAC address
   - Example: `tasmota-3B2F1A`
3. **Connect** to this network
   - Password: Usually blank (no password)
   - If password required, try: `tasmota` or `12345678`

---

### **Step 6: Initial Tasmota Configuration**

1. After connecting to `tasmota-XXXXXX`:
   - Browser should **auto-open** to configuration page
   - If not, manually go to: **http://192.168.4.1**

2. You'll see **Tasmota WiFi Configuration** page:
   ```
   AP1 SSID: [Your WiFi Name]
   AP1 Password: [Your WiFi Password]
   ```

3. **Configure your WiFi:**
   - Enter your home WiFi name (SSID)
   - Enter your home WiFi password
   - Click **"Save"**

4. Device will **reboot** and connect to your WiFi

5. **Find Tasmota's new IP:**
   - Check your router's DHCP list
   - Look for device named: `tasmota-XXXXXX`
   - Or use IP scanner app
   - Note the IP address (e.g., 192.168.1.123)

---

### **Step 7: Access Tasmota Main Interface**

1. Go to: **http://[tasmota-ip]** (use the IP from Step 6)
2. You should see **Tasmota Main Menu**:
   - Configuration
   - Firmware Upgrade
   - Console
   - Information
   - etc.

**🎉 TASMOTA IS NOW RUNNING! 🎉**

---

## What to Do Next

### **Option A: Configure Display (Advanced)**

See: `DISPLAY-CONFIGURATION.md` (will be created next)

This involves:
- Configuring GPIO pins via web interface
- Selecting display controller type
- Testing display output

### **Option B: Use as IoT Device (No Display)**

Use Tasmota features without the display:
- MQTT publishing
- GPIO control
- Timers and rules
- Home Assistant integration

### **Option C: Flash Back to Original**

If you want to return to SmallTV firmware:
1. Go to: **Firmware Upgrade**
2. Upload: `/home/user/smalltv-ultra/Ultra-V9.0.31/FW-Smalltv-Ultra-V9.0.31.bin`
3. Click: **Start Upgrade**
4. Wait for reboot
5. SmallTV restored! ✓

---

## Troubleshooting

### Problem: "Magic Byte" Error During Flash

**Cause**: Wrong firmware file or corrupted download

**Solution**:
1. Verify you're using `tasmota-display.bin` (not tasmota32 or other variant)
2. Check file size: Should be ~616 KB
3. Re-download if necessary
4. Verify MD5: `84a37748d43ca0d75742e6b389428061`

---

### Problem: Can't Find "tasmota-XXXXXX" WiFi After Flash

**Cause**: Device still trying to connect to old WiFi, or AP not started

**Solution**:
1. Wait 2-3 minutes (Tasmota tries saved WiFi first)
2. Power cycle the device (unplug and replug)
3. Look again for `tasmota-XXXXXX` network
4. Try scanning from different device (phone, laptop)

---

### Problem: Display Stays Black After Flash

**Cause**: This is **EXPECTED** - display needs GPIO configuration

**Solution**:
1. This is normal! Display won't work until configured
2. Access Tasmota via WiFi (it still works!)
3. Follow display configuration guide
4. OR flash back to original firmware if you need display immediately

---

### Problem: Upload Fails / Connection Lost

**Cause**: Network interruption during upload

**Solution**:
1. Try again (device is still running original firmware)
2. Move closer to device (if using WiFi)
3. Try different browser
4. Check device is still responding at http://192.168.4.1

---

### Problem: Device Won't Boot After Flash

**Cause**: Rare - possibly corrupted flash or power loss during write

**Solution**:
1. Power cycle device (unplug for 10 seconds, replug)
2. Wait 2 minutes for boot
3. Look for WiFi AP
4. If still no boot: Device needs UART recovery (advanced)

---

## Safety Notes 🛡️

✅ **Safe**: Tasmota has OTA, you can always flash back
✅ **Safe**: Display being blank is normal, not a problem
✅ **Safe**: You can't "brick" it via OTA (bootloader protected)

⚠️ **Caution**: Don't unplug during flash write
⚠️ **Caution**: Verify file before flashing
⚠️ **Note**: Weather/GIF features won't work in Tasmota

---

## Quick Reference

### Original Firmware
- **File**: `FW-Smalltv-Ultra-V9.0.31.bin`
- **MD5**: `b18f1a2abf43f6a24c5a6a4289789b7e`
- **WiFi AP**: GIFTV
- **Web Interface**: http://192.168.4.1

### Tasmota Firmware
- **File**: `tasmota-display.bin`
- **MD5**: `84a37748d43ca0d75742e6b389428061`
- **WiFi AP**: tasmota-XXXXXX
- **Web Interface**: http://192.168.4.1 (first boot) or http://[device-ip]

---

## Need Help?

- **Recovery**: See `BACKUP-INFO.md`
- **Display Config**: See `DISPLAY-CONFIGURATION.md` (coming next)
- **Tasmota Docs**: https://tasmota.github.io/docs/
- **Tasmota Support**: https://github.com/arendst/Tasmota/discussions

---

**Ready? Let's flash! 🚀**

Remember: You can ALWAYS flash back to the original firmware using the same process.
