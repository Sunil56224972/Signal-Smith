<h1 align="center">Signal-Smith by Sunil</h1>
<div align="center">
  <img width="1198" height="689" alt="image" src="https://github.com/user-attachments/assets/e256659e-9615-4333-8ece-9c7cbe481b3d" />
  <h3 align="center">Intended for authorized testing, research, and educational use only!</h3>
</div>

---
A portable sub-GHz RF signal Multi-Tool built on the ESP32-C3 SuperMini with a CC1101 radio module, created to push the boundaries. Signal-Smith scans, detects, logs, and replays wireless signals across common ISM bands - all from a compact handheld device with a 0,96" 128×64 I2C OLED display and 5-button navigation.

#### STILL IN DEVELOPMENT!

---

## Functions
Signal-Smith features a structured handheld-style menu system designed for fast navigation using the 5‑button interface.  
Expand each menu below to explore available functions.

### Main Menu

<details>
  <summary>📥 CAPTURE</summary>

  **Capture**
  - Receive RF signals
  - Replay captured transmissions instantly
  - Save signals for later use
  - Analyze detected protocols and parameters

  **Capture RAW** (being reworked!)
  - Records raw pulse timings
  - Used for unknown or unsupported protocols
  - Allows advanced replay experimentation

  **Frequency Analyzer**
  - Finds unknown signal frequencies (e.g. keyfobs, remotes)
  - Automatic active-frequency detection
  - Optimized for fast discovery across ISM bands

  **Frequency Spectrum**
  - Visualizes RF activity around you
  - Live RSSI energy monitoring
  - Displays surrounding airwave usage in real time

  **Config** *(Work in Progress)*
  - Configure capture presets
  - Adjustable detection parameters
  - Workflow optimization profiles

</details>

---

<details>
  <summary>📡 TRANSMIT</summary>

  **Saved**
  - Central storage for all saved signals
  - Organized categories:
    - Captured signals
    - RAW recordings
    - Randomly generated signals
  - Clean and structured signal management

  **Bruteforce**
  - Sequential signal transmission
  - Selectable:
    - Frequency
    - Bit length
    - Protocol
  - Includes protocols implemented in the Flipper Zero ecosystem

  **Random**
  - Generates completely random signal parameters:
    - Protocol
    - Key
    - Bit length
    - TE timing
    - Modulation
    - Lo‑first / Hi‑first ordering
    - Repeat count

  **Config** *(Work in Progress)*
  - Transmission presets
  - Saved transmission profiles
  - Automation preparation

</details>

---

<details>
  <summary>🛡️ JAMMER DETECTOR</summary>

  **Coming Soon**
  - Detect abnormal RF noise levels
  - Identify possible jamming activity
  - Monitor interference across monitored frequencies

</details>

---

<details>
  <summary>📶 JAMMER</summary>

  **Coming Soon**
  - Experimental RF interference testing tools
  - Intended strictly for controlled environments and authorized research

</details>

---

<details>
  <summary>⚙️ SETTINGS</summary>

  **Coming Soon**
  - Device configuration
  - Radio parameters
  - Power management options

</details>

---

### 🚧 More Features Incoming
Signal-Smith is actively evolving - new tools, protocols, and experimental RF capabilities are continuously being developed.

---

## Hardware

| Component | Details |
|---|---|
| MCU | ESP32-C3 SuperMini |
| Radio | CC1101 sub-GHz transceiver module |
| Display | 0,96" 128×64 OLED (I2C, SSD1306) |
| Navigation | 5 push buttons |
| Power | USB-C or LiPo via ESP32-C3 onboard regulator |

### Pin Wiring (NOT FINAL! Hardware will be added, therefore pins may be changed in the future!)

| Signal | ESP32-C3 Pin | ESP32 |
|---|---|---|
| CC1101 SCK | GPIO 4 | GPIO 18 |
| CC1101 MISO | GPIO 5 | GPIO 19 |
| CC1101 MOSI | GPIO 6 | GPIO 23 |
| CC1101 CSN | GPIO 7 | GPIO 5 |
| CC1101 GDO0 | GPIO 2 | GPIO 2 |
| 0,96" OLED SDA | GPIO 8 | GPIO 21 |
| 0,96" OLED SCL | GPIO 9 | GPIO 22 |
| Button UP | GPIO 1 | GPIO 27 |
| Button DOWN | GPIO 3 | GPIO 26 |
| Button LEFT | GPIO 10 | GPIO 33 |
| Button RIGHT | GPIO 20 | GPIO 32 |
| Button SELECT | GPIO 21 | GPIO 25 |

---

## Supported Frequency Bands

| Band | Typical Devices |
|---|---|
| 300 MHz | Generic ISM remotes |
| 303 / 308 MHz | Genie Intellicode, US alarm fobs |
| 310 / 315 / 318 MHz | Toyota, Honda, GM fobs, Chamberlain, LiftMaster |
| 330 / 345 MHz | Honeywell, 2GIG alarm sensors |
| 390 MHz | Older OOK remotes |
| 418 MHz | UK/EU ISM : alarm PIRs, door sensors |
| 430 MHz | EU/JP garage remotes |
| 433 MHz | EU standard : remotes, sensors, LoRa |
| 868 MHz | Z-Wave EU, LoRaWAN, wireless M-Bus, Sigfox |
| 915 MHz | Z-Wave US, LoRaWAN US915, smart meters |

---

## Installation

The source code is not made publicly available due to ungrateful individuals that previously stole my projects to profit from the sale of my devices without any attribution, ignoring the included project license. Install Signal-Smith by flashing the pre-compiled binary. Hopefully we will warp into a future where such consequences are not required...

### Option 1 - esptool (Command Line)

Make sure Python and esptool are installed:

```bash
pip install esptool
```

Then flash with:

```bash
python -m esptool --chip esp32c3 --port COM16 --baud 115200 write_flash ^
  0x0 Signal-Smith.bootloader.bin ^
  0x8000 Signal-Smith.partitions.bin ^
  0x10000 Signal-Smith.bin
```

> Replace `COM16` with your actual serial port!

To erase the chip before flashing (recommended for first install):

```bash
esptool.py --chip esp32c3 --port COM16 erase_flash
```

### Option 2 - ESP Flash Download Tool (Windows GUI)

1. Download the [ESP Flash Download Tool](https://www.espressif.com/en/support/download/other-tools) from Espressif
2. Select chip: **ESP32-C3**
3. Add `freqfoxrf.bin` at offset `0x0`
4. Select your COM port, set baud to `460800`
5. Click **START**

### Option 3 - Web Flasher

A browser-based flasher (no drivers required) may be available via [ESP Web Tools](https://esphome.github.io/esp-web-tools/) - check the releases page for a hosted installer link.

---

## Notes

- The CC1101 uses OOK energy detection for RSSI-based scanning across all bands, this allows reliable detection of both OOK and FSK devices without needing to match modulation
- 868 MHz detection requires the device to be reasonably close to the signal source due to the CC1101's reduced sensitivity above 700 MHz
- Saved signals persist across reboots via EEPROM (up to 20 signals, 2048 bytes total)
- Raw capture stores pulse timings for signals that fall outside known protocol formats
- in active development, features will be added!

---

## UI & Display

The Signal-Smith display graphics were designed using **[Lopaka](https://lopaka.app)** - a free, browser-based UI editor for embedded displays. If you're building anything that makes pixels go "Beep, Boop! 0 1", Lopaka lets you visually design your layouts and exports ready-to-use code directly. No more guessing pixel coordinates by hand.

Seriously worth bookmarking if you do any embedded UI work: **https://lopaka.app**
Special thanks to Lopaka.app for sponsoring and supporting by this!

---

## Disclaimer

Signal-Smith is intended for authorized testing, research, and educational use only. Transmitting on licensed frequencies without authorization may be illegal in your jurisdiction. I, the author (Sunil), DO NOT take responsibility for misuse.

---

## Contact & Connect

<p align="left">
  <a href="https://github.com/Sunil56224972"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" /></a>
  <a href="https://linkedin.com/in/YOUR_LINKEDIN"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
  <a href="https://instagram.com/YOUR_INSTAGRAM"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white" alt="Instagram" /></a>
  <a href="mailto:YOUR_EMAIL@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail" /></a>
  <a href="https://discord.com/users/YOUR_DISCORD_ID"><img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord" /></a>
</p>

---

## License

Hardware design and firmware are proprietary. Binary releases are provided for personal, non-commercial use only. Redistribution of the binary without permission is not permitted.
