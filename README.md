# Hackintosh Configuration for HP Spectre x360 aw0xxx

A simple EFI setup that works to some extent. Contributions and guidance are appreciated.

## Table of Contents

- [Hardware](#-hardware)
- [Software](#-software)
- [Installation Instructions](#-installation-instructions)
- [TODO / Dev Roadmap](#-todo--dev-roadmap)
- [Acknowledgements](#-acknowledgements)
- [Gallery](#-gallery)

## 💻 Hardware

| Component        | Model                              | Status | Notes |
|------------------|-------------------------------------|--------|-------|
| Laptop           | HP Spectre x360 13 aw0xxx          | ✔      | macOS 13.6.1 Ventura |
| CPU              | Intel Core i7-1065G7                | ✔      |       |
| iGPU             | Intel Iris Plus Graphics            | ✔      |       |
| eGPU             | XFX RX 5700XT                       | ✔      | Via Thunderbolt enclosure; detected and used, but frequency/power can't be adjusted – always near idle |
| RAM              | Micron LPDDR4X 3733MHz 8GB × 2      | ✔      |       |
| SSD              | Gloway NVMe 2TB                     | ✔      |       |
| Display          | Samsung AMOLED 4K                   | ✔      | Cursor glitches; brightness can't be adjusted via DDC (BetterDisplay helps) |
| Audio            | Realtek ALC285                      | ✔      | Speakers and headphones appear together in outputs; only far-end speakers emit sound |
| Microphone       | Intel Smart Sound Technology        | ❌     |       |
| Webcam           | HP TrueVision HD Camera             | ✔      |       |
| IR Camera        | HP IR Camera                        | ❌     |       |
| Fingerprint      | Synaptics UWP WBDI                  | ❌     |       |
| Wi-Fi & BT       | Intel AX201                         | ✔      | Some Bluetooth mice may be unstable; others are fine |
| Keyboard         | —                                   | ✔      | Mute LED doesn't light; Fn key is hardware-level (not macOS soft-Fn) |
| Trackpad         | Synaptics 3297                      | ❌     | Still under investigation |
| Touchscreen      | ELAN EzTouchFilter 2514&Col1        | ✔      | Supports APIC interrupts, multitouch, gestures; usable instead of trackpad |
| Pen              | ELAN EzTouchFilter 2514&Col6        | ❌     | MPP 2.0 pens work on Windows/Linux but not macOS |
| USB-A 5Gb/s      | —                                   | ✔      |       |
| USB-C 10Gb/s     | —                                   | ✔      |       |
| Thunderbolt 3    | Controller 8A17                     | ❌     | Some devices detected individually; no TB bus or hotplug; cold boot only; TB networking not functional |
| SD Card Reader   | Realtek SDXC UHS-I                  | ✔      |       |
| Sensor Status    | —                                   | ✔      | Sensors work; thermal info readable |

## 💿 Software

| Feature                        | Status | Notes |
|-------------------------------|--------|-------|
| CPU turbo/freq scaling        | ✔      |       |
| GPU hardware acceleration     | ✔      | No hardware video decode – CPU software decoding only |
| iCloud, iMessage, FaceTime    | ✔      | FaceTime crashes on outgoing calls |
| Continuity / Handoff          | ❌     | AirportItlwm limitation |
| Find My                       | ❌     | Can't share location or play sound from "Find My" app |
| Battery level indicator       | ✔      |       |
| Virtual Touch Bar             | ✔      | Works with SMBIOS MacBookPro16,2 |
| Sleep                         | ❌     | Long delay waking after screen off |
| Hibernation                   | ❌     |       |
| Boot macOS from internal EFI  | ❌     | Works for other OSes; macOS only works via USB EFI |
| Screen auto-rotation          | ❌     | Can bind to hotkeys manually |

## 🧭 Installation Instructions

You'll need:

- A USB drive (≥16GB, USB 3.0 or faster recommended)
- [OpenCore Auxiliary Tool (OCAT)](https://github.com/ic005k/OC-Gen-X/releases)
- macOS installer image & this repo
- Time & patience ◝(ᵔᵕᵔ)◜

### Preparation

1. Follow the [OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/) to create the installer and mount its EFI partition.
2. Adjust BIOS settings according to the guide.
3. Open `EFI-RegularBoot/*/OC/config.plist` using OCAT, choose HP options, and save.

### Installation

1. Replace the USB's `EFI` with `1. AW0XXX -  EFI for Ventura 13.3 - Installation` and rename it to `EFI`.
2. Shutdown, plug in the USB, boot from it, and install macOS.
3. After a few reboots, booting from the internal disk will fail — let the system shut down.
4. Replace the USB’s `EFI` again with `3. AW0XXX - EFI BOOTABLE 4 Ventura 13.3` and rename it to `EFI`.
5. Boot again from the USB, choose internal disk, finish macOS setup. **Don’t log in to Apple ID yet.**

### Post-Install Cleanup

1. Replace the USB’s `EFI` with `EFI-RegularBoot` and rename to `EFI`.
2. Open `EFI/OC/config.plist` using OCAT and generate your own Serial Number, UUID, MLB, and ROM. Test at Apple’s site.  
⚠️ Do **not** reuse the ones from this repo – they’re randomly generated and unverified.
3. Reboot and enjoy macOS. Now log into Apple ID.

## 🧾 TODO / Dev Roadmap

- [ ] Fix trackpad
- [ ] Fix pen support
- [ ] Detect convertible mode (disable KB/touchpad when flipped)
- [ ] Fix cursor artifacts
- [ ] Improve eGPU behavior
- [ ] Fix remaining quirks