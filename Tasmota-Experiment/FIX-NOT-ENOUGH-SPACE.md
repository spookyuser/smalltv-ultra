# Fix "Not Enough Space" Error - Two-Step Upgrade Method

**Problem**: `Update error: ERROR[4]: Not Enough Space`

**Cause**: The tasmota-display.bin (616 KB) is too large to fit in the OTA partition space on your SmallTV-Ultra.

**Solution**: Use the **two-step upgrade method** with a smaller firmware first.

---

## Why This Happens

The ESP8266 has a partition layout like this:

```
┌─────────────────────┐
│  Bootloader         │
├─────────────────────┤
│  App1 (Current FW)  │ ← Your SmallTV firmware (498 KB)
├─────────────────────┤
│  App2 (OTA Space)   │ ← Too small for 616 KB!
├─────────────────────┤
│  File System        │
└─────────────────────┘
```

The OTA partition (App2) is sized for the original firmware (~500 KB). When you try to upload a larger firmware (616 KB), it doesn't fit.

---

## Two-Step Upgrade Solution

### Step 1: Flash Tasmota-Minimal (368 KB)
### Step 2: Upgrade to Tasmota-Display (616 KB) via Tasmota OTA

This works because:
1. **Minimal fits**: 368 KB < OTA partition size ✓
2. **Tasmota repartitions**: After first boot, Tasmota can reorganize flash space
3. **Then upgrade**: Now there's enough room for the full 616 KB version

---

## Step-by-Step Instructions

### **STEP 1: Flash Tasmota-Minimal**

1. **Access SmallTV Web Interface**
   - Go to: http://192.168.4.1

2. **Navigate to Firmware Upgrade**
   - Find the firmware update page

3. **Upload Minimal Version**
   - Choose file: **`tasmota-minimal.bin`** (368 KB, NOT tasmota-display.bin!)
   - Click: **Update** / **Start Upgrade**

4. **Wait for Flash**
   ```
   Expected:
   - Upload progress (0-100%)
   - Validation passes (368 KB fits!)
   - Flash write successful
   - "Update Success! Rebooting..."
   ```

5. **Device Reboots**
   - Wait 30-60 seconds
   - Display will be BLANK (expected)
   - WiFi AP appears: **"tasmota-XXXXXX"**

---

### **STEP 2: Upgrade to Tasmota-Display**

1. **Connect to Tasmota**
   - Connect to WiFi: **"tasmota-XXXXXX"**
   - Browser opens to: http://192.168.4.1
   - Configure your WiFi credentials
   - Device connects to your network
   - Note the new IP address

2. **Access Tasmota Web Interface**
   - Go to: http://[tasmota-ip]

3. **Navigate to Firmware Upgrade**
   - Click: **Firmware Upgrade**
   - You'll see OTA options

4. **Upload Full Display Version**
   - Under "Use file upload" section
   - Choose file: **`tasmota-display.bin`** (616 KB)
   - Click: **Start Upgrade**

5. **Wait for Upgrade**
   ```
   Expected:
   - Upload progress
   - "Upload Successful"
   - Automatic reboot
   - Tasmota-Display running!
   ```

6. **Verify**
   - Device reboots (30-60 seconds)
   - Connect to same WiFi/IP
   - Access web interface
   - Check: Main Menu → Information
   - Should show "tasmota-display" or similar

**🎉 SUCCESS! You now have Tasmota-Display with display driver support!**

---

## Quick Reference

### File Comparison

| Firmware | Size | Use |
|----------|------|-----|
| **FW-Smalltv-Ultra-V9.0.31.bin** | 498 KB | Original (recovery) |
| **tasmota-minimal.bin** | 368 KB | Step 1 (fits in OTA) ✓ |
| **tasmota-display.bin** | 616 KB | Step 2 (via Tasmota OTA) ✓ |

### Verification

```bash
# Check file sizes
ls -lh Tasmota-Experiment/*.bin

# Verify magic bytes (all should be 0xE9)
head -c 1 tasmota-minimal.bin | od -A n -t x1
head -c 1 tasmota-display.bin | od -A n -t x1

# Verify MD5
md5sum tasmota-minimal.bin
md5sum tasmota-display.bin
```

---

## Alternative: Web OTA Method (Easier!)

Instead of uploading files, use Tasmota's web OTA feature:

### After Step 1 (Minimal Installed):

1. **Access Tasmota Console**
   - Main Menu → **Console**

2. **Set OTA URL to Display Build**
   ```
   OtaUrl http://ota.tasmota.com/tasmota/release/tasmota-display.bin
   ```

3. **Start Upgrade**
   ```
   Upgrade 1
   ```

4. **Wait**
   - Tasmota downloads from internet
   - Flashes automatically
   - Reboots with display version

This skips the manual file upload!

---

## Troubleshooting

### Problem: Minimal Also Says "Not Enough Space"

**Very unlikely** (minimal is only 368 KB), but if it happens:

**Solution 1: Try Tasmota-Lite**
```bash
# Even smaller version (~430 KB when decompressed)
curl -L -o tasmota-lite.bin.gz \
  "http://ota.tasmota.com/tasmota/release/tasmota-lite.bin.gz"
gunzip tasmota-lite.bin.gz
# Flash this instead
```

**Solution 2: UART Flash (Advanced)**
If OTA completely fails, use USB-to-Serial adapter:
```bash
esptool.py --port /dev/ttyUSB0 write_flash 0x00000 tasmota-minimal.bin
```

---

### Problem: Step 1 Works, Step 2 Fails

**Cause**: Network issue or OTA server down

**Solution**:
1. Try again (sometimes transient)
2. Upload file manually instead of OTA URL
3. Check internet connection
4. Try different Tasmota mirror

---

### Problem: Want Original Firmware Back

**From Tasmota-Minimal or Tasmota-Display:**

1. Access Tasmota web interface
2. Firmware Upgrade
3. Upload: `FW-Smalltv-Ultra-V9.0.31.bin`
4. Device reboots with original firmware

This should work from either Tasmota version!

---

## Why Two Steps?

**Can't Tasmota Minimal just download Display version on first boot?**

No, because:
1. Minimal needs to boot first to create WiFi AP
2. You need to configure WiFi
3. Then it has internet access to download
4. OTA updates need running firmware to manage the process

**Can't we compress the Display version more?**

The .gz compression is already applied during download. The 616 KB is the decompressed binary that must fit in flash.

**Could we make a smaller custom Tasmota with only display drivers?**

Yes! But that requires compiling from source. The pre-built tasmota-display.bin is the smallest official build with display support.

---

## Files Provided

Both files are ready in `/home/user/smalltv-ultra/Tasmota-Experiment/`:

✅ **tasmota-minimal.bin** (368 KB)
- MD5: `e7bff11d6592fafb6f65edb2da39d0ee` (may vary by version)
- For Step 1: Initial flash from SmallTV

✅ **tasmota-display.bin** (616 KB)
- MD5: `84a37748d43ca0d75742e6b389428061`
- For Step 2: Upgrade from Tasmota-Minimal

---

## Complete Process Summary

```
SmallTV-Ultra (498 KB)
    ↓ Flash via SmallTV web interface
Tasmota-Minimal (368 KB) ← Fits in OTA partition! ✓
    ↓ Upgrade via Tasmota OTA
Tasmota-Display (616 KB) ← Now has enough space! ✓
    ↓ Configure display
Working SmallTV with Tasmota! 🎉
```

---

## Recovery Path

At any point, you can return to original:

```
Any Tasmota Version
    ↓ Flash via Tasmota web interface
SmallTV-Ultra Original (498 KB) ← Fits! ✓
    ↓
Back to normal! ✓
```

---

**Ready to try the two-step method?** Start with Step 1 using `tasmota-minimal.bin`! 🚀

---

## Additional Resources

- **Tasmota OTA Server**: http://ota.tasmota.com/tasmota/release/
- **Upgrade Documentation**: https://tasmota.github.io/docs/Upgrading/
- **Partition Info**: https://tasmota.github.io/docs/Partitions/

Good luck! This method should work. 💪
