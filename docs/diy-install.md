# DIY Installation Guide

This guide covers flashing the DomoWatch firmware onto a
**Waveshare ESP32-S3-Touch-LCD-1.69** board.

---

## Prerequisites

### Hardware
- Waveshare ESP32-S3-Touch-LCD-1.69 board
- USB-C cable (data-capable, not charge-only)
- PC running Windows

### Software
Download and install the **ESP32 Flash Download Tool** from Espressif:

Search for "ESP32 Flash Download Tool" at:
https://www.espressif.com/en/support/download/other-tools

---

## Step 1 — Download the firmware files

You need **4 files**, all available at the root of this repository:

| File | Address |
|------|---------|
| `bootloader.bin` | `0x0000` |
| `partitions.bin` | `0x8000` |
| `boot_app0.bin` | `0xe000` |
| `firmware.bin` | `0x10000` |

Download them all from:
**https://github.com/francksachet/Domowatch**

Click on each file → **Download raw file** (or use the download button).

---

## Step 2 — Connect the board

Connect the Waveshare board to your PC via USB-C.

The board should appear as a COM port in Windows Device Manager
(e.g. `COM3`, `COM4`...). If no port appears, install the USB driver
provided by Waveshare for your board.

> **If the board is not detected:** hold the **BOOT** button (GPIO0) on the
> board while connecting the USB cable to force it into download mode.

---

## Step 3 — Configure the Flash Download Tool

1. Open the Flash Download Tool
2. Select **ESP32-S3** as chip type
3. Select **USB** as connection mode

### File list (top section)

Check the **4 checkboxes** on the left and fill in each row:

| ✓ | File | Address |
|---|------|---------|
| ☑ | `bootloader.bin` | `0x0000` |
| ☑ | `partitions.bin` | `0x8000` |
| ☑ | `boot_app0.bin` | `0xe000` |
| ☑ | `firmware.bin` | `0x10000` |

Use the **"..."** button on each row to browse to the downloaded files.

### SPI Flash Config

| Setting | Value |
|---------|-------|
| SPI SPEED | **80 MHz** |
| SPI MODE | **QIO** |

### Bottom section

| Setting | Value |
|---------|-------|
| COM | Select your board's port (check Windows Device Manager) |
| BAUD | **921600** |

---

## Step 4 — Flash

1. Make sure the board is connected and the COM port is selected
2. Click **ERASE** first — this is important for a clean first installation
3. Click **START**
4. The green progress bar animates — wait for the **FINISH** message

> **If the flash fails at start:** hold the **BOOT** button on the board while
> clicking START, then release it once the download has begun.

---

## Step 5 — First boot

1. Disconnect and reconnect the USB cable (or press the RESET button)
2. DomoWatch boots and displays the clock screen with a "Not synced" indicator
3. The device is ready for WiFi configuration

→ Continue with the **[User Guide](user-guide.md)** for initial setup.

---

## Troubleshooting

**No COM port appears**
- Try a different USB cable (many USB-C cables are charge-only)
- Hold BOOT (GPIO0) while plugging in to force download mode
- Install or reinstall the Waveshare USB driver

**Flash fails or times out**
- Reduce BAUD to 460800 or 115200
- Try a different USB port (avoid USB hubs)
- Hold the BOOT button while clicking START

**Device reboots in a loop after flashing**
- The flash was incomplete — click ERASE, then START again

**Screen stays black**
- Normal during flashing. Press RESET after the FINISH message appears.
