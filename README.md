## Install the latest version(s) of macOS on an Asus C425/C433/C434¹ ²

[![License](https://img.shields.io/badge/license-GPL-blue)](https://www.gnu.org/licenses/gpl-3.0.en.html) [![Status](https://user-images.githubusercontent.com/77316348/230705808-40c7ba6b-9b4f-41fb-8c40-8e7db3b97ad0.png)](https://github.com/meghan06/ChromebookOSX)
```
  ____ _                              _                 _     ___  ______  __
 / ___| |__  _ __ ___  _ __ ___   ___| |__   ___   ___ | | __/ _ \/ ___\ \/ /
| |   | '_ \| '__/ _ \| '_ ` _ \ / _ \ '_ \ / _ \ / _ \| |/ / | | \___ \\  / 
| |___| | | | | | (_) | | | | | |  __/ |_) | (_) | (_) |   <| |_| |___) /  \ 
 \____|_| |_|_|  \___/|_| |_| |_|\___|_.__/ \___/ \___/|_|\_\\___/|____/_/\_\

```


--------------------------------------------------------------------------------------------------------------------------------------------------------

## Table of Contents
- [Current Status](#current-status)
- [Versions Tested](#versions-tested)
  - [macOS](#macos)
  - [OpenCore](#opencore)
- [**Disclaimer**](#%EF%B8%8F-disclaimer-%EF%B8%8F)
- [Requirements](#requirements)
- [Issues](#issues)
  - [Current Issues](#current-issues)
  - [Fixed Issues](#fixed-issues)
- [**1. Installation**](#1-installation)
   - [Required Steps](#these-steps-are-required-for-proper-functioning)
   - [Kext Folder](#kexts)
   - [ACPI Folder](#acpi-folder)
- [2. Post Install](#2-post-install)
   - [Continuity](#continuity-features)
   - [**Audio**](#audio)
   - [Misc. Information](#misc-information)
- [3. macOS Ventura](#3-macos-ventura)
  - [For those updating](#for-those-updating)
  - [For those installing directly](#preparations-for-installing-ventura-directly)
  - [Fixing WiFI](#fixing-wifi-on-ventura)
- [Credits](#credits)



--------------------------------------------------------------------------------------------------------------------------------------------------------

Turns out, this laptop works really well with the latest version(s) of macOS. For more information about the Chromebook's hardware, see [here](https://github.com/meghan06/ChromebookOSX/blob/main/Resources/Hardware%20Info.txt).

| macOS Ventura | eMMC Storage |
|------------|-------------|
|<img src="Resources/Screenshot.png" width="960">|<img src="Resources/Device Info.png" width="960">|

--------------------------------------------------------------------------------------------------------------------------------------------------------

## Current Status


| **Feature**        | **Status**           | **Notes**                                                                                     |
|--------------------|----------------------|-----------------------------------------------------------------------------------------------|
| WiFi               | Working              | With `itlwm.kext v2.2.0 alpha` and `Heliport v1.4.1` `(Latest)`.                              |
| Bluetooth          | Working              | With `IntelBluetoothFirmware` and `BlueToolFixup.kext`.                                       |
| Suspend / Sleep    | Working partially    | Only on battery power, working with `EmeraldSDHC.kext`.                                       |
| Trackpad           | Working              | With `VoodooI2C.kext` and `VoodooI2CELAN.kext`.                                               | 
| Graphics Accel.    | Working              | With `-igfxnotelemetryload` in the `boot-args`.                                               |
| Internal Speakers  | Not working          | Unsupported codec. (`max98927`)                                                               |
| Keyboard backlight | Working              | With `SSDT-KBBl.aml` _**and**_ the custom `VoodooPS2.kext`.                                  |           
| Keyboard & Remaps  | Working              | With the custom `VoodooPS2.kext`.                                                            |
| eMMC Storage       | Working              | With `EmeraldSDHC.kext`and IRQ patching (with SSDTTime)                                       |    
| SD Card Reader     | Not working          | Coming soon with `EmeraldSDHC.kext`.                                                          |
| Headphone Jack     | Not working          | Unsupported codec
| USB Ports          | Working              | Working with USB mapping **and** `SSDT-USB-RESET.aml`                                         |
| Webcam             | Working              | Working OOTB                                                                                  |
| Internal Mic.      | Not working          | Same reason why internal speakers don't work; unsupported codec. (`max98927`)                 |
| Logout / Lock      | Working              | Working OOTB.                                                                                 |
| Shutdown / Restart | Working              | Working with `ProtectMemoryReigons` enabled in `config.plist`.                                |    
| Recovery Combos    | Working              | Working OOTB with coreboot.                                                                   |
| Continuity         | Not Working          | Limitation with Intel WiFI cards / `itlwm`.                                                   |                                                                             
                                                                                    
--------------------------------------------------------------------------------------------------------------------------------------------------------
### Versions Tested

#### macOS:
Note: Image opens in new tab, either Reddit or Discord.

- [macOS Mojave (10.14)](https://preview.redd.it/du0a3cftqw7a1.png?width=1920&format=png&auto=webp&v=enabled&s=ac6d75fcfe423f12fe27aae947f89a55c00f7590)
- [macOS Catalina (10.15)](https://media.discordapp.net/attachments/302485086060937219/1064325342787026955/image.png?width=1119&height=629)
- [macOS Big Sur (11)](https://cdn.discordapp.com/attachments/1051619981642706947/1078427190129070183/image.png)
- [macOS Monterey (12)](https://cdn.discordapp.com/attachments/1084252068711247965/1089784117572423690/Screen_Shot_2023-03-26_at_10.31.51_PM.png)
- [macOS Ventura (13)](https://preview.redd.it/sdlqqbufnbfa1.png?width=1920&format=png&auto=webp&v=enabled&s=e38a2085eaf2021061b2b0a23ab3214a044eb50e)


macOS 10.1x were tested on external USB drives, so eMMC support may vary. For best experience, just install Big Sur (11) or newer.

#### OpenCore:
  - 0.8.6
  - 0.8.7
  - 0.8.8
  - 0.8.9
  - 0.9.0
  
--------------------------------------------------------------------------------------------------------------------------------------------------------

### ⚠️ Disclaimer ⚠️

 > **Warning**:  **By continuing, you acknowledge that you have read and understood the contents of [LICENSE.md](LICENSE.md) and the [disclaimer](#%EF%B8%8F-disclaimer-%EF%B8%8F), and consent to their terms.**

The instructions outlined in this [GitHub repo](https://github.com/meghan06/ChromebookOSX) have the potential to cause permanent harm to your laptop, and you should be aware of this potential outcome before proceeding. I cannot be held accountable for any damage caused from following or disregarding these instructions. I make no assurances concerning the dependability or efficacy of the materials referenced in this repository.

TL:DR: If you fuck up and break something, it's not my fault.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Requirements

Before you start, you'll need to have the following items to complete the process:

- **An understanding that this process has the potential to damage / brick your device, potentially causing it to become forever inoperable.**
- An external storage device (can range from a SD card to a USB Disk / Drive) for creating the installer USB.  
- The latest OpenCore version (**at least 0.8.8**) for eMMC boot drive support.   
- An internet connection.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Issues
> **Note**: Report other issues in [Issues](https://github.com/meghan06/ChromebookOSX/issues)


#### Current Issues
- https://github.com/meghan06/ChromebookOSX/issues/10 Chromium based apps breaking after sleep. [help needed] 
- Render issues / blank screen / green boxes as images / text not appearing on Electron and Chromium based apps. [help needed] 
  - Disable GPU acceleration / hardware acceleration in the app settings.

#### Fixed Issues
- ~~Weird lock ups randomly.~~
- ~~Signout not working~~
- ~~Kernel panic when shutting down / restarting~~
  - Fixed by setting `ProtectMemoryReigons` to `TRUE`.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 1. Installation

Here are the steps to go from chromeOS to macOS via OpenCore on your Chromebook. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

> **Warning** Pay _close_ attention to the Chromebook specific parts in the Dortania guide, specifically in `ACPI -> Booter` and the extra iGPU `boot-args`.

> **Warning** Pay _very_ close attention to the following steps, if you miss **even one**, your Chromebook will lose some functionally and might not even boot.

> **Note**: ASUS C434 users will need [VoodooI2CHID](https://github.com/VoodooI2C/VoodooI2C/releases/).kext for touchscreen to function. It is bundled with VoodooI2C and VoodooI2CELAN

> **Note**: Those who are installing to an external disk like a USB drive can skip steps 9 and 10.



--------------------------------------------------------------------------------------------------------------------------------------------------------

### **These steps are **required** for proper functioning.**

1. If you haven't already, flash your Chromebook with [MrChromebox's UEFI firmware](https://mrchromebox.tech) via his scripts. To complete this process, you must turn off write protection either by using a SuzyQable cable or temporarily removing the battery (latter is less cumbersome).
2. Setup your EFI folder using the [OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/). Use [Laptop Kaby Lake & Amber Lake Y](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/kaby-lake.html#starting-point) for your `config.plist`. 
3. Re-visit this guide when you're done setting up your EFI. There are a few things we need to tweak to ensure our Chromebook works with macOS. Skipping these steps will result in a **very** broken hack.
4. In your `config.plist`, under `Booter -> Quirks` set `ProtectMemoryRegions` to `TRUE`. It should look something like this in your `config.plist` when done correctly:

   | Quirk                | Type | Value    |
   | -------------------- | ---- | -------- |
   | ProtectMemoryRegions | Boolean | True  |
   
   > **Warning** **This must be enabled.**

4. Under `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)`, make the following modifications:
  
   | Key                  | Type | Value    |
   | -------------------- | ---- | -------- |
   | AAPL,ig-platform-id  | data | 0000C087 |
   | device-id            | data | C0870000 |
     
   > **Warning** **These should be the only two items `in PciRoot(0x0)/Pci(0x2,0x0)`.**
5. If you haven't already, add `igfxrpsc=1` and `-igfxnotelemetryload` to your `boot-args`, under `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82,`. Both are for iGPU support, **you will regret it if you don't add these.**
6. **Set your SMBIOS as MacBookAir8,1**. Ignore what the guide tells you to use, MacBookAir8,1 works better with our laptop.
     > **Note** If you choose to use `MacBook10,1`, which also works, you will not have Low Battery Mode.
7. Switch the VoodooPS2 from acidanthera with this [custom build that's designed for Chromebooks](https://github.com/one8three/VoodooPS2-Chromebook/releases) for keyboard backlight control + custom remaps. 
   - Keyboard backlight SSDT (`SSDT-KBBL.aml`) can be found [here](https://github.com/one8three/VoodooPS2-Chromebook/blob/master/SSDT-KBBL.aml). Drag it to your ACPI folder.
     > **Note**: This SSDT only works with the custom VoodooPS2 linked above.
8. Download [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC/releases) for eMMC storage support. Put it in your Kexts folder. 
9. Download corpnewt's SSDTTime, then launch it and select `FixHPET` as the first option. Next, select `'C'` for the default setting, and drag the SSDT it generated (`SSDT-HPET.aml`) into your `ACPI` folder. Finally, copy the patches from `oc_patches.plist` into your `config.plist` under `ACPI -> Patch`. This is to ensure eMMC storage is recognized by macOS.
10. Map your USB ports³ before installing ~~to prevent dead hard drives, thermonuclear war, or you getting fired.~~ See [Misc. Information](#misc-information) for a note to USBToolBox users.    
12. Using corpnewt's SSDTTime, dump your DSDT, generate `SSDT-USB-RESET.aml`, drag it to your ACPI folder, and reload your `config.plist`. **Required** for working USB ports.
13. Snapshot (cmd +r) or (ctrl + r) your `config.plist`. 
14. Install macOS and enjoy!

> **Note**: More information about specific quirks can be found [here.](https://dortania.github.io/docs/latest/Configuration.html)

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Kext's.

```
BlueToolFixup.kext
BrightnessKeys.kext
EmeraldSDHC.kext
IntelBluetoothFirmware.kext
IntelBTPatcher.kext
itlwm.kext
Lilu.kext
SMCBatteryManager.kext
SMCProcessor.kext
UTBMap.kext
USBToolBox.kext
VoodooPS2Controller.kext
WhateverGreen.kext
```

--------------------------------------------------------------------------------------------------------------------------------------------------------

### ACPI Folder

```
SSDT-EC-USBX-LAPTOP.aml
SSDT-HPET.aml
SSDT-KBBL.aml
SSDT-PNLF.aml
SSDT-USB-Reset.aml aka. SSDT-RHUB.aml
  - Note: This is not needed if using USBToolBox
```

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 2. Post Install 

- This section contains information to better maintain your Chromebook hack, general advice, and other cool things. Should be a good idea to read this. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Security 
- Please read [Security.md](SECURITY.md) as it provides in-depth information on sanitizing your serials, disk encryption, and more.
- **If you discover a vulnerability, please refer to [Security.md](SECURITY.md).** 

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Continuity Features

- the wireless chip is soldered on, you can't replace it. :(

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Audio

**For anybody looking to get audio working, here are a some bits of info you can use:**

- Intel DSP discussion:
  - https://github.com/acidanthera/bugtracker/issues/2084
- If you are willing to contribute, make a template driver that has 2 inputs + output endpoints that gets a DMA buffer + position callback from macOS
- C425 is not HD audio, it uses an Intel DSP (branded as Smart Sound) and is i2s from that coprocessor
- google src code for max98927 (for chromeOS)
  - [google src code for max98927](https://chromium.googlesource.com/chromiumos/platform/depthcharge/+/refs/heads/master/src/drivers/sound/max98927.c)
- Soundflower, a simple template that can be used for the driver
  - [github.com/RogueAmoeba/Soundflower-Original](https://github.com/RogueAmoeba/Soundflower-Original)
  - More info here: [cdn.discordapp.com](https://cdn.discordapp.com/attachments/1051619981642706947/1077064200490324019/image.png)
- Base HD Audio driver for Skylake and up. Used for HDMI Audio support on Windows.
  - [github.com/coolstar/sklhdaudbus](https://github.com/coolstar/sklhdaudbus)
- DMIC uses DA7219 as the mic array, max98927 as the audio codec.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Misc. Information

- When formatting the eMMC drive in Disk Utility, make sure to toggle "Show all Drives" and **erase the WHOLE drive**, not just the current partition.
- Format the drive as `APFS`
- Map your USB ports prior to installing macOS³ for a painless install. You **will** reget it if you don't. You can use [USBToolBox](https://github.com/USBToolBox/tool) to do that. If you are using USBToolBox (Mainly Windows users), you need a second kext that goes along with it. [Github repo here](https://github.com/USBToolBox/kext). USBToolBox will not work without this kext. 
- `itlwm` is more stable & faster than `AirportItlwm`
- You might have DRM issues, there's no fix for this. :(
- Control keyboard backlight with left `ctrl` + left `alt` and `<` `>`. 
    - `<` to decrease, `>` to increase.
- To fix the battery life on Ventura, you can set Low Battery Mode to be always enabled on battery. It's not perfect, but it helps. You can also use CPUFriend to tweak power settings but you may or may not cause your hack to die on boot.
- eMMC will come up as an external drive in the boot picker since eMMC is just an embedded SD card. Nothing you can do about it.
- To hide the drive picker, set `ShowPicker` to `False` in `Misc` ->` Boot` -> `ShowPicker`
- `AppleXcpmCfgLock` and `DisableIOMapper` can be enabled or disabled. Makes no difference.
- It's worth noting that while it's recommended, coreboot already includes mapped USB ports, meaning that USB mapping is not required. Proceed at your  own risk if you decide to skip USB mapping.
- Make sure your `ScanPolicy` is set to `0`. eMMC will not be recognized if it's some other value.
- Please report any broken links in issues. Half this guide was written while I was high. /s
- **USB ports will ONLY work with SSDT-USB-Reset / SSDT-RHUB.** 
  - Note: This is not needed if using USBToolBox

#### *Note: The hotkey to show drives **DOES NOT WORK**. Make a copy of your EFI with `ShowPicker` enabled if you need to boot from another drive.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. macOS Ventura
> **Note** Only for those who want macOS Ventura. 

Before we get started, you should know the following:
- **Your battery will drain faster on Ventura.** To avoid this, stay on Monterey or older.


With that, lets get started!

--------------------------------------------------------------------------------------------------------------------------------------------------------

### For those updating:
> **Note**: these steps can be done after updating, you just won't have WiFi. It is recommended to follow the steps below **before** updating for the least amount of pain and suffering.

1. Mount your EFI using corpnewt's MountEFI.
2. Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
3. Download and install Heliport if you haven't already. The most recent stable release will work fine.
4. Launch ProperTree and reload (`ctrl+r`) your `config.plist`. 
5. Start the update. (If you haven't already)
6. You are now ready for macOS Ventura! 
7. [See below](#fixing-wifi-on-ventura) for fixing WiFi.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Preparations for installing Ventura directly:

> **Note**: For Windows and Linux users only.

1. Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
2. Open up your kext folder, and locate `itlwm.kext`. 
3. Open it, and find `Info.plist`.
4. Open ProperTree, navigate to where your `Info.plist` is, and open it.
5. Under `IOKitPersonalities -> itlwm -> WiFiConfig`, enter your WiFI details. Save and close when done.
7. Launch ProperTree and reload (`ctrl+r`) your `config.plist`. 
8. Boot recovery. There will be no WiFi logo/symbol, but you will have WiFi. If you are able to install macOS, then you have not messed up.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Fixing WiFi on Ventura 

> **Note** **Only for those that have macOS installed but haven't edited their `Info.plist`.**

1. Mount your EFI
2. Open up your kext folder, and locate `itlwm.kext`. 
3. Click it, and select `Show Package Contents` and open the Contents folder. Once inside, find the `Info.plist` and copy it via  `ctrl + c`.
4. Exit `itlwm.kext` and go back to your Kext folder. Paste your `Info.plist`.
5. Open ProperTree, select the `Info.plist` in your Kext folder and open it.
6. Under `IOKitPersonalities -> itlwm -> WiFiConfig`, enter your WiFI details. Save and close when done.
7. Replace the old `Info.plist` with the one you just edited. 


Do note that Heliport will report no WiFi upon logging in but keep in mind you actually do thanks to the edits we just made. :) 

--------------------------------------------------------------------------------------------------------------------------------------------------------

other things:

¹ ASUS C434 users will need VoodooI2CHID.kext for touchscreen to function.

² ASUS C425 and C434 are both tested, C433 not yet.

³ USBToolBox is the reccomended USB mapping tool.
