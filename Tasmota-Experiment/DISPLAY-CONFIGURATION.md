# Tasmota Display Configuration Guide

After flashing Tasmota, the display will be blank. This guide helps you configure it.

---

## Understanding the Problem

**Why is the display blank?**

Tasmota doesn't know:
1. What type of display controller you have (ST7789? ILI9341? Other?)
2. Which GPIO pins connect to the display
3. Display resolution (240x320? 240x240?)
4. SPI configuration

We need to **discover** and **configure** these settings.

---

## Display Configuration Process

### **Method 1: Via Web Interface (Recommended)**

#### Step 1: Access Tasmota Configuration

1. Go to Tasmota web interface: `http://[tasmota-ip]`
2. Click: **Configuration** → **Configure Module**
3. You'll see GPIO pin configuration page

#### Step 2: Set Module to Generic

1. Under "Module type" select: **Generic (18)**
2. Click **Save**
3. Device will reboot

#### Step 3: Configure SPI Pins (Trial and Error)

**Common ESP8266 pins for SPI displays:**

Try this configuration first:

| GPIO Pin | Function | Tasmota Setting |
|----------|----------|-----------------|
| GPIO12 | MOSI (Data) | SPI MOSI |
| GPIO14 | CLK (Clock) | SPI CLK |
| GPIO15 | CS (Chip Select) | SPI CS |
| GPIO2 | DC (Data/Command) | SPI DC |
| GPIO0 | RST (Reset) | Display Rst |
| GPIO4 | BL (Backlight) | Backlight |

**How to configure:**

1. Configuration → Configure Module
2. For each GPIO pin, select from dropdown:
   - GPIO12 → "SPI MOSI"
   - GPIO14 → "SPI CLK"
   - GPIO15 → "SPI CS"
   - GPIO2 → "SPI DC"
   - GPIO0 → "Display Rst"
   - GPIO4 → "Backlight"
3. Click **Save**
4. Device reboots

#### Step 4: Select Display Controller

After reboot, configure the display type:

1. Go to **Console** (Main Menu → Console)
2. Try each display type with these commands:

**For ST7789 (common budget TFT):**
```
DisplayModel 18
DisplayWidth 240
DisplayHeight 240
```

**For ILI9341 (very popular):**
```
DisplayModel 4
DisplayWidth 240
DisplayHeight 320
```

**For ILI9342:**
```
DisplayModel 5
DisplayWidth 320
DisplayHeight 240
```

#### Step 5: Test Display

```
Backlight 1
DisplayDimmer 100
DisplayText Hello World!
```

**If display works:** 🎉 SUCCESS!
**If display blank:** Try different pin configuration or display model

---

### **Method 2: Via Console Commands**

Instead of web UI, you can configure everything via console:

```
# Set module to generic
Module 0

# Configure GPIO pins
GPIO12 SPI MOSI
GPIO14 SPI CLK
GPIO15 SPI CS
GPIO2 SPI DC
GPIO0 Display Rst
GPIO4 Backlight

# Restart
Restart 1

# After restart, configure display
DisplayModel 18
DisplayWidth 240
DisplayHeight 240
Backlight 1
DisplayDimmer 100
DisplayText Test
```

---

## Display Models Reference

| Model # | Display Controller | Typical Resolution |
|---------|-------------------|-------------------|
| 4 | ILI9341 | 240x320 |
| 5 | ILI9342 | 320x240 |
| 6 | ILI9488 | 320x480 |
| 7 | SSD1306 (OLED) | 128x64 |
| 18 | ST7789 | 240x240 or 240x320 |
| 19 | SSD1351 (Color OLED) | 128x128 |

**Full list**: https://tasmota.github.io/docs/Displays/

---

## Common GPIO Pin Combinations to Try

If the first configuration doesn't work, try these combinations:

### **Configuration A (Default SPI)**
- GPIO12 → SPI MOSI
- GPIO14 → SPI CLK
- GPIO15 → SPI CS
- GPIO2 → SPI DC
- GPIO0 → Display Rst
- GPIO4 → Backlight

### **Configuration B (Alternative)**
- GPIO13 → SPI MOSI
- GPIO14 → SPI CLK
- GPIO15 → SPI CS
- GPIO5 → SPI DC
- GPIO16 → Display Rst
- GPIO4 → Backlight

### **Configuration C (HSPI)**
- GPIO13 → SPI MOSI (HSPI)
- GPIO14 → SPI CLK (HSPI)
- GPIO15 → SPI CS (HSPI)
- GPIO2 → SPI DC
- GPIO0 → Display Rst
- GPIO4 → Backlight

---

## Systematic Discovery Process

If nothing works, use this systematic approach:

### **Phase 1: Find the Display Controller Type**

1. Visual inspection:
   - Open the SmallTV case (carefully)
   - Look at display PCB
   - Find the controller IC (small chip on back of display)
   - Note the part number (e.g., "ST7789V", "ILI9341")

2. Based on controller, use correct DisplayModel:
   - ST7789 → Model 18
   - ILI9341 → Model 4
   - ILI9488 → Model 6

### **Phase 2: Find GPIO Pin Mappings**

**Method A: Trace PCB**
1. Open device
2. Follow traces from ESP8266 to display
3. Note which GPIO pin connects to which display pin

**Method B: Trial and Error**
1. Try all common configurations (A, B, C above)
2. For each configuration:
   - Set GPIO pins
   - Restart
   - Set DisplayModel
   - Test with DisplayText
3. Document what works

**Method C: Community Research**
1. Search for SmallTV-Ultra schematic
2. Check manufacturer documentation
3. Ask in GeekMagic forums/Discord

---

## Testing Commands

Once configured, test with these commands:

```
# Basic test
DisplayText Hello Tasmota!

# Clear display
DisplayClear

# Draw text at position
DisplayText [x50y50s2] Big Text

# Set brightness
DisplayDimmer 50

# Turn backlight on/off
Backlight 0  (off)
Backlight 1  (on)

# Display system info
DisplayText [x10y10] %topic%
DisplayText [x10y30] IP: %ip%
DisplayText [x10y50] RSSI: %rssi%

# Refresh
DisplayRefresh
```

---

## Advanced: Using DisplayDescriptor

For complex displays, you might need custom descriptor:

```
DisplayDescriptor <descriptor_string>
```

Example for ST7789:
```
DisplayDescriptor :H,ST7789,240,240,16,SPI,1,*,*,*,*,*,*,*,5
```

Format: `:H,<driver>,<width>,<height>,<colors>,<interface>,<bits>,...`

---

## Display Modes

Tasmota supports different display modes:

```
DisplayMode 0  # Turn off display
DisplayMode 1  # Local sensor data
DisplayMode 2  # MQTT data
DisplayMode 3  # Time/Date
DisplayMode 4  # Local sensor data (rotated)
DisplayMode 5  # MQTT data (rotated)
```

---

## If You Can't Get Display Working

**Don't worry!** You have options:

### **Option 1: Use Tasmota Without Display**
- Still has WiFi
- Still has web interface
- Use for MQTT/IoT
- Control via web/app

### **Option 2: Flash Back to Original**
1. Firmware Upgrade
2. Upload: `FW-Smalltv-Ultra-V9.0.31.bin`
3. Reboot
4. Display works again!

### **Option 3: Get Help**
- Tasmota Discord: https://discord.gg/Ks2Kzd4
- Tasmota Forums: https://github.com/arendst/Tasmota/discussions
- SmallTV Community (if exists)

### **Option 4: Document and Share**
Once you find the working configuration:
- Document the pins
- Share with community
- Help others with same device
- Create custom template

---

## Creating a Template (After Success)

Once you find the working configuration, create a template:

1. Go to: Configuration → Configure Other
2. Click: **Template**
3. Fill in:
   - Name: "SmallTV-Ultra"
   - Base: Generic (18)
   - GPIO Configuration: (your working pins)
4. Click: **Activate**
5. Export template and share!

Example template format:
```
{"NAME":"SmallTV-Ultra","GPIO":[0,0,224,0,225,226,0,0,227,228,229,0,0,0],"FLAG":0,"BASE":18}
```

---

## Useful Resources

- **Tasmota Display Docs**: https://tasmota.github.io/docs/Displays/
- **GPIO Functions**: https://tasmota.github.io/docs/Components/
- **Display Commands**: https://tasmota.github.io/docs/Commands/#displays
- **ST7789 Example**: https://tasmota.github.io/docs/ST7789/
- **ILI9341 Example**: https://tasmota.github.io/docs/ILI9341/

---

## Quick Troubleshooting

| Problem | Solution |
|---------|----------|
| Display blank | Check power (Backlight command), verify pins, try different DisplayModel |
| Garbage on screen | Wrong display type or wrong resolution |
| Display upside down | Use DisplayRotate command (0, 1, 2, 3) |
| Colors inverted | Use DisplayInvert 1 |
| Display dim | Increase DisplayDimmer (0-100) or Backlight 1 |
| No response | GPIO pins wrong, try different configuration |

---

**Good luck with display configuration!** 🎨

Remember: Even if display doesn't work, Tasmota is still fully functional via web interface!
