---
weight: 310
title: "Prerequisites"
description: "Prerequisites"
icon: menu_book
lead: ""
draft: false
---

# Prerequisites
Read this before proceeding. I am not responsible if you fuck up your laptop


## ⚠️ Disclaimer 

 > **Warning**:  **By continuing, you acknowledge that you have read and understood the contents of [LICENSE.md](LICENSE.md) and the [disclaimer](#%EF%B8%8F-disclaimer-%EF%B8%8F), and consent to their terms.**

The instructions outlined in this [GitHub repo](https://github.com/meghan06/ChromebookOSX) have the potential to cause permanent harm to your laptop, and you should be aware of this potential outcome before proceeding. I cannot be held accountable for any damage caused from following or disregarding these instructions. I make no assurances concerning the dependability or efficacy of the materials referenced in this repository.


### Requirements

Before you start, you'll need to have the following items to complete the process:

- **An understanding that this process has the potential to damage and/or brick your device, potentially causing it to become inoperable.**
- An external storage device (can range from a SD card to a USB Disk / Drive) for creating the installer USB.  
- The latest OpenCore version (**at least 0.8.8**) for eMMC boot drive support.   
- An internet connection.

### Current Status


| **Feature**        | **Status**           | **Notes**                                                                                     |
|--------------------|----------------------|-----------------------------------------------------------------------------------------------|
| WiFi               | Working              | With `Airportitlwm`                                                                           |
| Bluetooth          | Working              | With appropriate Bluetooth kexts                                                              |
| Suspend / Sleep    | Working w/ fix       | Only with custom coreboot ROM                                                                 |
| Trackpad           | Working              | With `VoodooI2C` kexts                                                                        | 
| Graphics Accel.    | Working              | With `-igfxnotelemetryload` in the `boot-args`.                                               |
| Internal Speakers  | Not working          | Unsupported codec. (`max98927`)                                                               |
| Keyboard backlight | Working              | With `CrosEC.kext` and a modified VoodooPS2 kext                                              |           
| Keyboard & Remaps  | Working              | With a SSDT remap               .                                                             |
| eMMC Storage       | Working              | With `EmeraldSDHC.kext`and HPET SSDT                                                          |    
| SD Card Reader     | Not working          | WIP with `EmeraldSDHC.kext`.                                                                  |
| Headphone Jack     | Not working          | Unsupported codec                                                                             |
| HDMI Audio         | Working              | Working with AppleALC, thx [bernsgtx](https://reddit.com/u/ogridberns)                        |
| HDMI Video         | Working              | Working OOTB, thx [bernsgtx](https://reddit.com/u/ogridberns)                                 |                                                                             
| USB Ports          | Working              | Working with USB mapping **and** `SSDT-USB-RESET.aml`                                         |
| Webcam             | Working              | Working OOTB                                                                                  |
| Internal Mic.      | Not working          | Same reason why internal speakers don't work; unsupported codec. (`max98927`)                 |
| Logout / Lock      | Working              | Working OOTB.                                                                                 |
| Shutdown / Restart | Working              | Working with `ProtectMemoryReigons` set to true in `config.plist`.                            |    
| Continuity         | Not Working          | Limitation with Intel WiFI cards.                                                             |    



### Current Issues
1. None!

### Fixed Issues
1. Sleep/wake breaking video playback
2. Blank screens/ render 


### Versions Tested

#### macOS
1. macOS Lion (10.7)  
2. macOS Mojave (10.14)
3. macOS Catalina (10.15)
4. macOS Big Sur (11)
5. macOS Monterey (12)
6. macOS Ventura (13)
7. macOS Sonoma (14)

#### OpenCore
1 .0.8.6
2. 0.8.7
3. 0.8.8
4. 0.8.9
5. 0.9.0
6. 0.9.4
7. 0.9.6







