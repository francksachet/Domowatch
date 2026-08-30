# DIY Installation Guide

This guide covers flashing the DomoWatch firmware onto a
**Waveshare ESP32-S3-Touch-LCD-1.69** board.

---

## Prerequisites

### Hardware
- Waveshare ESP32-S3-Touch-LCD-1.69 board
- USB-C cable (data-capable, not charge-only)
- PC running Windows, macOS, or Linux

### Software
Download and install the **ESP32 Flash Download Tool** (Windows) or
use **esptool.py** (cross-platform, requires Python).

**Option A — Flash Download Tool (Windows, easiest)**
Download from: https://www.espressif.com/en/support/download/other-tools

**Option B — esptool.py (all platforms)**
```
pip install esptool
```

---

## Step 1 — Download the firmware

Download the latest `firmware.bin` from the
[releases section](https://github.com/francksachet/Domowatch/releases)
or directly from the repository:

[📥 Download firmware.bin](../firmware.bin)

---

## Step 2 — Connect the board

1. Connect the Waveshare board to your PC via USB-C
2. The board should appear as a COM port (Windows) or `/dev/ttyACM0` (Linux/macOS)
3. If the port is not recognized, install the [CP210x USB driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)

> **Note:** If the board does not appear after connecting, hold the BOOT button
> (GPIO0) while connecting the USB cable to force it into download mode.

---

## Step 3 — Flash the firmware

### Option A — Flash Download Tool (Windows)

1. Open the Flash Download Tool
2. Select **ESP32-S3** as chip type
3. Select **USB** as connection mode
4. In the file list, add `firmware.bin` at address **`0x0`**
5. Set the following parameters:
   - SPI Speed: **80 MHz**
   - SPI Mode: **DIO**
   - Flash Size: **16MB**
6. Click **ERASE** first (important for first-time installation)
7. Click **START** to flash

### Option B — esptool.py (all platforms)

Open a terminal and run:

```bash
esptool.py --chip esp32s3 --port COM3 --baud 921600 \
  --before default_reset --after hard_reset \
  write_flash --flash_mode dio --flash_freq 80m --flash_size 16MB \
  0x0 firmware.bin
```

Replace `COM3` with your actual port (`/dev/ttyACM0` on Linux, `/dev/cu.usbmodem*` on macOS).

> **First installation:** add `--erase-all` before `write_flash` to ensure a clean slate.

---

## Step 4 — First boot

1. Disconnect and reconnect the USB cable (or press the RESET button)
2. DomoWatch will boot and display the clock screen with "Not synced" indicator
3. The device is ready for WiFi configuration

→ Continue with the **[User Guide](user-guide.md)** for initial setup.

---

## Troubleshooting

**Board not detected**
- Try a different USB cable (many USB-C cables are charge-only)
- Hold BOOT (GPIO0) while plugging in to force download mode
- Install or reinstall the USB driver

**Flash fails or times out**
- Reduce baud rate to 460800 or 115200
- Try a different USB port (avoid USB hubs)

**Device reboots in a loop after flashing**
- The flash was incomplete — repeat the procedure with `--erase-all`

**Screen stays black**
- Normal during flashing. Press RESET after successful flash.
