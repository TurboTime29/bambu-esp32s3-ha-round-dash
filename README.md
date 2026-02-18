# Bambu Lab ESPHome Dashboard

A wireless, touchscreen dashboard for Bambu Lab 3D printers using ESPHome and Home Assistant. Features real-time printer stats, dynamic progress tracking, AMS filament visualization, and a modern Material Design interface.

## Features

### 🖥️ Four Pages - tap on left or right side of screen to go to next page

1. **Temperatures** - Real-time bed/hotend/chamber temperatures with color-coded cards
2. **Print Progress** - Dynamic arc display with smart color changes:
   - White while printing
   - Green when print completes (bed still hot)
   - Blue when bed has cooled (safe to touch)
3. **AMS Status** - Live filament visualization with actual spool colors
4. **Brightness Control** - Adjust backlight with touch buttons

### ✨ Smart Features

- **Auto Page Switching** - Automatically switches to progress page when print starts
- **Real-Time Clock** - Shows current time (Eastern Time, configurable)
- **Dynamic AMS Colors** - Pulls actual hex color codes from Bambu integration
- **Touch Navigation** - Swipe left/right to navigate pages
- **Material Design Icons** - Clean, modern iconography
- **Instant Updates** - Sensor changes reflect within 1-2 seconds

## Hardware Required

### Essential
- **Waveshare ESP32-S3-Touch-LCD-1.28**
  - 240x240 round GC9A01A display
  - Capacitive touch (CST816S)
  - ~$15-20 on AliExpress/Amazon
  
### Optional
- 3D printed case (3MF file included)
- Charging dock (3MF files included)

## Software Requirements

- **Home Assistant** (running on Raspberry Pi or similar)
- **Bambu Lab Home Assistant Integration** - [GitHub Link](https://github.com/greghesp/ha-bambulab)
- **ESPHome Add-on** for Home Assistant

## Quick Start

### Step 1: Set Up Home Assistant
Install Home Assistant on a Raspberry Pi - [Installation Guide](https://www.home-assistant.io/installation/)

### Step 2: Install Bambu Lab Integration
Settings → Devices & Services → Add Integration → "Bambu Lab"

### Step 3: Install ESPHome
Settings → Apps → Install App → "ESPHome Device Builder" → Install (recommend adding this to your left menu when installing for easy access)

### Step 4: Flash Your Device + Connect to HA
1. Download 'bambu-dashboard-final.yaml'
2. Configuration - 
Replace 'YOUR_PRINTER_ENTITY' with your printer's entity from Home Assistant. 
You can find this by going to Settings > Devices & services > Bambu Lab > click on your printer > Choose one of the sensors shown on the dashboard like "Bed target temperature" and click the Gear icon > Here, the entity ID should contain your printer ID after "sensor." It will look something like "p2s_22a3dw298139112"
3. Click on "Secrets" at the top right of ESPHome Builder and add your wifi information by adding these two lines and then click "Save" - 
wifi_ssid: "YOUR WIFI SSID"
wifi_password: "YOUR WIFI PASSWORD"
4. Go back and click the menu icon (three dots) next to the new device created and click "Install" - I prefer to choose Manual Download as I have had the best luck with that.
5. Once done compiling, choose the "Factory" option and save the .bin file - my browser didn't seem to like the file to I had to click "Keep" each time so remember that
6. Once you have the .bin file, install drivers from CH343SER.zip file and restart PC then go to https://web.esphome.io/ 
7. While holding the "Boot" button on the back of ESP32, plug device into your PC and click on "Connect" - you should now see your device show up, and click "Install" and choose the Factory option and then choose the .bin file downloaded
8. Click the "Reset" button after boot and you should see the device working (no data yet)
9. Go to Home Assistant > Settings > Devices & services - you should now see the new ESPHome device in here - add this to your account
10. Reset device using button and it should restart and stream data!



## Configuration


### Change Timezone

Line 26:

timezone: America/New_York  # Your timezone


### Adjust Default Brightness

Line 484:

initial_value: 40  # 0-100


### Change Arc "Cooled" Temperature

Line 214 (when arc turns blue):

lambda: return x < 46;  # Celsius


## Troubleshooting

| Issue | Solution |
|-------|----------|
| Display blank | Check USB cable is data-capable, verify WiFi credentials |
| Shows "--" | Check Bambu integration working, verify entity IDs match |
| Touch not working | Some units use GPIO13 for reset instead of GPIO5 |
| Wrong colors | Verify Bambu sends color attributes in HA |
| Slow startup | Normal for first boot (30-60s), faster after |


## Technical Specs

**Display:** GC9A01A 240x240 round  
**Touch:** CST816S capacitive  
**Memory:** PSRAM quad mode required  
**Power:** ~0.5W @ 40% brightness  
**Update Lag:** 1-2 seconds  
**Firmware Size:** ~2MB

### Pin Configuration

Display (SPI): CLK=10, MOSI=11, MISO=12, CS=9, DC=8, RST=14
Touch (I2C):   SDA=6, SCL=7, INT=5, RST=13
Backlight:     PWM=2


## Credit and Inspriration

Credit for the original case design goes to SqueakyRobot on Makerworld - https://makerworld.com/en/models/877451

Inspired by WLED printer status strip project - https://makerworld.com/en/models/2172105


## Contributing

PRs welcome! Especially:
- UI themes
- Alternative displays
- Bug fixes

## License

MIT License - Use freely!

## Support

- Issues: GitHub Issues
- Questions: GitHub Discussions
- Reddit: https://www.reddit.com/r/BambuLab/comments/1r4t1y6/

**⭐ Star if this helped you!**

Built with ❤️ using ESPHome + Home Assistant
