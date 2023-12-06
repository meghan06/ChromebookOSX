## Install the latest version(s) of macOS on an Asus C425/C433/C434¹ ²

# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

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
- [**Disclaimer**](#%EF%B8%8F-disclaimer)
- [**Is it stable?**](#is-it-stable)
- [Requirements](#requirements)
- [Issues](#issues)
  - [Current Issues](#current-issues)
  - [Fixed Issues](#fixed-issues)
- [**1. Installation**](#1-installation)
   - [Required Steps](#these-steps-are-required-for-proper-functioning)
   - [Fixing coreboot 4.2.0](#fixing-coreboot-420)
   - [Kext Folder](#kexts)
   - [ACPI Folder](#acpi-folder)
- [2. Post Install](#2-post-install)
   - [Fixing Battery Life](#battery-life)
   - [Fixing Trackpad](#fixing-trackpad)
   - [Continuity](#continuity-features)
   - [**Audio**](#audio)
   - [Security](SECURITY.md)
   - [Misc. Information](#misc-information)
- [3. macOS Ventura](#3-macos-ventura)
- [4. macOS 14 - Sonoma](#4-macos-14---sonoma)
- [5. Misc](#other)


--------------------------------------------------------------------------------------------------------------------------------------------------------

Turns out, this laptop works really well with the latest version(s) of macOS. For more information about the Chromebook's hardware, see [here](https://github.com/meghan06/ChromebookOSX/blob/main/Resources/Hardware%20Info.txt).


| macOS Sonoma | eMMC Storage |
|------------|-------------|
|<img src="https://preview.redd.it/plpokliv6h4b1.png?width=1920&format=png&auto=webp&v=enabled&s=6d4cff00272fed54d56a1ec485e61d33da9356d4" width="960">|<img src="Resources/Device Info.png" width="960">|

--------------------------------------------------------------------------------------------------------------------------------------------------------

## Current Status
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.


| **Feature**        | **Status**           | **Notes**                                                                                     |
|--------------------|----------------------|-----------------------------------------------------------------------------------------------|
| WiFi               | Working              | With `Airportitlwm`                                                                           |
| Bluetooth          | Working              | With `IntelBluetoothFirmware` and `BlueToolFixup.kext`.                                       |
| Suspend / Sleep    | Working w/ fix       | Only with custom coreboot ROM                                                                 |
| Trackpad           | Working              | With `VoodooI2C.kext` and `VoodooI2CELAN.kext`.                                               | 
| Graphics Accel.    | Working              | With `-igfxnotelemetryload` in the `boot-args`.                                               |
| Internal Speakers  | Not working          | Unsupported codec. (`max98927`)                                                               |
| Keyboard backlight | Working              | With `SSDT-KBBl.aml` _**and**_ the custom `VoodooPS2.kext`.                                   |           
| Keyboard & Remaps  | Working              | With the custom `VoodooPS2.kext`.                                                             |
| eMMC Storage       | Working              | With `EmeraldSDHC.kext`and patched HPET                                                       |    
| SD Card Reader     | Not working          | WIP with `EmeraldSDHC.kext`.                                                                  |
| Headphone Jack     | Not working          | Unsupported codec                                                                             |
| HDMI Audio         | Working              | Working with AppleALC, thx [bernsgtx](https://reddit.com/u/ogridberns)                        |
| HDMI Video         | Working              | Working OOTB, thx [bernsgtx](https://reddit.com/u/ogridberns)                                 |                                                                             
| USB Ports          | Working              | Working with USB mapping **and** `SSDT-USB-RESET.aml`                                         |
| Webcam             | Working              | Working OOTB                                                                                  |
| Internal Mic.      | Not working          | Same reason why internal speakers don't work; unsupported codec. (`max98927`)                 |
| Logout / Lock      | Working              | Working OOTB.                                                                                 |
| Shutdown / Restart | Working              | Working with `ProtectMemoryReigons` set to true in `config.plist`.                            |    
| Continuity         | Not Working          | Limitation with Intel WiFI cards / `itlwm`.                                                   |    

--------------------------------------------------------------------------------------------------------------------------------------------------------

## Is it stable?
Yes.
                                                                                    
--------------------------------------------------------------------------------------------------------------------------------------------------------
### Versions Tested

#### macOS:
Note: Image opens in new tab, either Reddit or Discord.

- [macOS Mojave (10.14)](https://preview.redd.it/du0a3cftqw7a1.png?width=1920&format=png&auto=webp&v=enabled&s=ac6d75fcfe423f12fe27aae947f89a55c00f7590)
- [macOS Catalina (10.15)](https://media.discordapp.net/attachments/302485086060937219/1064325342787026955/image.png?width=1119&height=629)
- [macOS Big Sur (11)](https://cdn.discordapp.com/attachments/1051619981642706947/1078427190129070183/image.png)
- [macOS Monterey (12)](https://cdn.discordapp.com/attachments/1084252068711247965/1089784117572423690/Screen_Shot_2023-03-26_at_10.31.51_PM.png)
- [macOS Ventura (13)](https://preview.redd.it/sdlqqbufnbfa1.png?width=1920&format=png&auto=webp&v=enabled&s=e38a2085eaf2021061b2b0a23ab3214a044eb50e)
- [macOS Sonoma (14)](https://preview.redd.it/plpokliv6h4b1.png?width=1920&format=png&auto=webp&v=enabled&s=6d4cff00272fed54d56a1ec485e61d33da9356d4)


#### OpenCore:
  - 0.8.6
  - 0.8.7
  - 0.8.8
  - 0.8.9
  - 0.9.0
  - 0.9.4
  
--------------------------------------------------------------------------------------------------------------------------------------------------------

### ⚠️ Disclaimer 

 > **Warning**:  **By continuing, you acknowledge that you have read and understood the contents of [LICENSE.md](LICENSE.md) and the [disclaimer](#%EF%B8%8F-disclaimer-%EF%B8%8F), and consent to their terms.**

The instructions outlined in this [GitHub repo](https://github.com/meghan06/ChromebookOSX) have the potential to cause permanent harm to your laptop, and you should be aware of this potential outcome before proceeding. I cannot be held accountable for any damage caused from following or disregarding these instructions. I make no assurances concerning the dependability or efficacy of the materials referenced in this repository.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### Requirements
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

Before you start, you'll need to have the following items to complete the process:

- **An understanding that this process has the potential to damage and/or brick your device, potentially causing it to become inoperable.**
- An external storage device (can range from a SD card to a USB Disk / Drive) for creating the installer USB.  
- The latest OpenCore version (**at least 0.8.8**) for eMMC boot drive support.   
- An internet connection.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Issues
> **Note**: Report other issues in [Issues](https://github.com/meghan06/ChromebookOSX/issues)


#### Current Issues
>**Note**: coreboot 4.20 (5/15/2023 release) is known to cause issues with booting macOS. A fix can be found [below](#fixing-coreboot-420).


#### Fixed Issues
- https://github.com/meghan06/ChromebookOSX/issues/10 Chromium based apps breaking after sleep.  
- Broken apps after waking from sleep (not to be confused with issue 1 below)
- Render issues / blank screen / green boxes as images / text not appearing on Electron and Chromium based apps. [help needed] 
  - Fixed by `SSDT-SBUS-MCHC.aml`, you can create the SSDT [here.](https://dortania.github.io/Getting-Started-With-ACPI/Universal/smbus.html)
- Kernel panic when shutting down / restarting
  - Fixed by setting `ProtectMemoryReigons` to `TRUE`.
- Weird lock ups randomly.
- Signout not working

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 1. Installation
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

Here are the steps to go from chromeOS to macOS via OpenCore on your Chromebook. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

> **Warning** Pay _close_ attention to the Chromebook specific parts in the Dortania guide, specifically in `Booter -> Quirks` and the iGPU `boot-args`.

> **Warning** Pay _very_ close attention to the following steps, if you miss **even one**, your Chromebook will lose some functionally and might not even boot.

> **Note**: C434 and C433 users (SHYVANA) will need [VoodooI2CHID](https://github.com/VoodooI2C/VoodooI2C/releases/).kext for touchscreen to function. It is bundled with the VoodooI2C package.

> **Note**: Those who are installing to an external disk like a USB drive can skip steps 10 and 11.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### **These steps are **required** for proper functioning.**
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

1. If you haven't already, flash your Chromebook with [MrChromebox's UEFI firmware](https://mrchromebox.tech) via his scripts. To complete this process, you must turn off write protection either by using a SuzyQable  or temporarily removing the battery, with latter being less cumbersome.
2. Setup your EFI folder using the [OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/). Use [Laptop Kaby Lake & Amber Lake Y](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/kaby-lake.html#starting-point) for your `config.plist`. 
3. Re-visit this guide when you're done setting up your EFI. There are a few things we need to tweak to ensure our Chromebook works with macOS. Skipping these steps will result in a **very** broken hack.
4. Add the compiled version of [SSDT-PLUG-ALT](https://github.com/meghan06/croscorebootpatch) to your ACPI folder.
5. In your `config.plist`, under `Booter -> Quirks` set `ProtectMemoryRegions` to `TRUE`. It should look something like this in your `config.plist` when done correctly:

   | Quirk                | Type | Value    |
   | -------------------- | ---- | -------- |
   | ProtectMemoryRegions | Boolean | True  |
   
   > **Warning** **This must be enabled.**

6. Under `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)`, make the following modifications:
  
   | Key                  | Type | Value    |
   | -------------------- | ---- | -------- |
   | AAPL,ig-platform-id  | data | 0000C087 |
   | device-id            | data | C0870000 |
     
   > **Warning** **These should be the only two items `in PciRoot(0x0)/Pci(0x2,0x0)`.**
7. If you haven't already, add `igfxrpsc=1` and `-igfxnotelemetryload` to your `boot-args`, under `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82,`. Both are for iGPU support, **you will regret it if you don't add these.**
8. **Set your SMBIOS as `MacBookAir8,1`**. Ignore what Dortania tells you to use, `MacBookAir8,1` works better with our Chromebook.
9. Switch the VoodooPS2 from acidanthera with this [custom build that's designed for Chromebooks](https://github.com/meghan06/ChromebookPS2) for keyboard backlight control + custom remaps. 
   - Keyboard backlight SSDT (`SSDT-KBBL.aml`) can be found [here](https://github.com/meghan06/ChromebookPS2/blob/master/Docs/SSDT-KBBL.aml). Drag it to your ACPI folder.
     > **Note**: This SSDT only works with the custom VoodooPS2 linked above.
10. Download [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC/releases) for eMMC storage support. Put it in your Kexts folder. 
11. Download corpnewt's SSDTTime, then launch it and select `FixHPET`. Next, select `'C'` for default, and drag the SSDT it generated (`SSDT-HPET.aml`) into your `ACPI` folder. Finally, copy the patches from `oc_patches.plist` into your `config.plist` under `ACPI -> Patch`. 

    > **Warning** Steps 10 and 11 are **required** for macOS to recognize the internal eMMC disk. 
    > **Note** If eMMMC isn't recognized in Disk Utility, you probably made a mistake in steps 10 & 11.

12. Map your USB ports³ before installing ~~to prevent dead hard drives, thermonuclear war, or you getting fired.~~ See [Misc. Information](#misc-information) for a note to USBToolBox users.    
13. Using corpnewt's SSDTTime, dump your DSDT, generate `SSDT-USB-RESET.aml`, drag it to your ACPI folder, and reload your `config.plist`. **Required** for working USB ports.

    > **Note**:  USBToolBox users can skip this step, otherwise you must do this or your USB ports won't work.

14. Snapshot (cmd +r) or (ctrl + r) your `config.plist`. 

    > **Warning**: NEVER do clean snapshots (`ctrl/cmd+shift+r`) after adding your HPET patches, they will be **wiped**. Only do regular snapshots. (`ctrl/cmd+r`)

15. Install macOS and enjoy!

> **Note**: In depth information about OpenCore can be found [here.](https://dortania.github.io/docs/latest/Configuration.html)

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Fixing coreboot 4.2.0
coreboot 4.2.0 (5/15/2023 release) has a known issue where macOS will hang on boot due to coreboot not defining CPU cores by default. To fix this, we'll use a SSDT to manually define them. Credits to [ExtremeXT](https://github.com/ExtremeXT) for the fix.

- If you haven't already, add the compiled version of [SSDT-PLUG-ALT](https://github.com/meghan06/croscorebootpatch) to your ACPI folder.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### Fixing Sleep / Wake Issues
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

1. Clone the MrChromebox coreboot repo, see here for the repo: [https://github.com/mrchromebox/coreboot](https://github.com/mrchromebox/coreboot)
2. Go to `/src/mainboard/google/poppy/Kconfig`
3. Find `DISABLE_HECI1_AT_PRE_BOOT`
4. Change the default value (`y`) to `n`

            -    default y
            +    default n
5. Save and exit.
6. Build and flash.

Note: The first 2 builds will fail. Try to build the ROM 3 times, it should suceeed on the 3rd try.

--------------------------------------------------------------------------------------------------------------------------------------------------------

# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.


### Kexts

```
BlueToolFixup.kext
BrightnessKeys.kext
EmeraldSDHC.kext
IntelBluetoothFirmware.kext
IntelBTPatcher.kext
AirportItlwm.kext
Lilu.kext
SMCBatteryManager.kext
SMCProcessor.kext
UTBMap.kext
USBToolBox.kext
VoodooPS2Controller.kext
WhateverGreen.kext
```

>**Note**: If you plan to install macOS Big Sur or older (11≥), your Bluetooth kexts will be different. Read the Dortania guide to find out what you need.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### ACPI Folder

```
SSDT-EC.aml
SSDT-HDAS-OFF.aml
SSDT-HPET.aml
SSDT-KBBL.aml
SSDT-PNLF.aml
SSDT-SDXC.aml
SSDT-USBX.aml
SSDT-I2C.aml
SSDT-SBUS-MCHC.aml
```
>**Note**: These SSDTs were generated with [SSDTTime](https://github.com/corpnewt/SSDTTime), with the exception of SSDT-HDAS-OFF, SSDT-SDXC, SSDT-I2C, and SSDT-SBUS-MCHC.aml.

>**Note**: USBToolBox users don't need `SSDT-USB-RESET` or `SSDT-RHUB`

>**Note**: `SSDT-HPET` and it's `config.plist` patch is only required for eMMC support.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 2. Post Install 
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Battery Life

Grab the following SSDTs and drag the compiled (`.aml`) into your ACPI folder:

1. [cros**sdxc**disable](https://github.com/meghan06/crossdxcdisable)
2. [croshdasdisable](https://github.com/meghan06/croshdasdisable)

These SSDTs disable unsupported devices, saving battery life and improving stability

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Fixing Trackpad
The trackpad in the Asus C425/C433/C434 should work OOTB, but they operate in polling mode which drains battery significantly. To fix this, we'll be using a config.plist patch, a modified version of VoodooGPIO.kext, and a SSDT.

- To start off, download the contents from [crosgpiopatch](https://github.com/meghan06/crosgpiopatch).
- Drag the compiled version of `SSDT-I2C.kext` (thx 1Revenger1) into your ACPI folder
- Then, in your `config.plist`, copy `patches_GPIO.plist` to `ACPI -> Patch`
- Finally, replace VoodooGPIO.kext from Kexts\VoodooI2C.kext\Contents\PlugIns with the one from crosgpiopatch.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### Continuity Features

The WLAN chipset is soldered on, you **cannot** replace it. It will never work.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Audio
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

Unless a dedicated Intel SST driver is written, speakers, mic, and 3.5mm will **never** work.

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
- max98927 for speakers, DMIC for mic, and DA7219 for headphone jack.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Misc. Information

- When formatting the eMMC drive in Disk Utility, make sure to toggle "Show all Drives" and erase the entire drive.
- Format the drive as `APFS` and `GUID Partition Table / GPT`
- Map your USB ports prior to installing macOS³ for a painless install. You **will** reget it if you don't. You can use [USBToolBox](https://github.com/USBToolBox/tool) to do that. You *will* need a second kext that goes along with it for it to work. [Repo here.](https://github.com/USBToolBox/kext). USBToolBox will not work without this kext. 
- `itlwm` is more stable & faster than `AirportItlwm` on macOS Ventura. 
- `itlwm` is the only way to connect to Wi-Fi on Sonoma. 
- AppleTV and other DRM protected services may not work.
- Control keyboard backlight with left `ctrl` + left `alt` and `<` `>`. 
    - `<` to decrease, `>` to increase.
- To fix  battery life, use CPUFriend to tweak power settings. 
- To hide the OpenCore boot menu, set `ShowPicker` to `False` in `Misc` ->` Boot` -> `ShowPicker`
- `AppleXcpmCfgLock` and `DisableIOMapper` can be enabled or disabled. There is no difference.
- It's worth noting that while it's recommended, coreboot already includes mapped USB ports, meaning that USB mapping is not required. Proceed at your own risk if you decide to skip USB mapping.
- eMMC will not be recognized if `ScanPolicy` is set to `0`.
- **USB ports will ONLY work with SSDT-USB-Reset / SSDT-RHUB.** 
- macOS will not sleep if you have USB devices plugged in 
>**Note**: SSDT-USB-Reset / SSDT-RHUB is not needed if using USBToolBox.

>**Warning**: The hotkey to show bootdrives does not work. Make a copy of your EFI with `ShowPicker` enabled if you need to boot from another drive in OpenCore.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. macOS Ventura
> **Note** Only for people installing macOS Ventura. 

> **Note**: Make sure you have the latest version of AirportItlwm. To download the **latest release of AirportItlwm,** [see here](https://github.com/OpenIntelWireless/itlwm/releases).

Before we get started, you should know the following:
- Ventura will run a little hotter
- The AirportItlwm variant for macOS 13 is slightly less stable than the macOS 12 version.
- No AirDrop (duh) 
- HandOff has been disabled to increase stabillity and speed.

Stay on macOS 12 (Monterey) to avoid these issues.


--------------------------------------------------------------------------------------------------------------------------------------------------------

## 4. macOS 14 - Sonoma

>**Warning** **DO NOT USE SONOMA FOR DAILY USE! YOU ARE ON YOUR OWN.**

**Please be patient while the chrultrabook community, hackintosh community, and I iron out all issues.**

6/5/2023 note: macOS Sonoma does not have a internet recovery at the time of writing. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Notes: 

- AirportItlwm does not work on Sonoma. Use Itlwm + Heliport for now.
  - Apple dropped `IO80211FamilyLegacy` in the DB of Sonoma, which Airportitlwm relies on. Itlwm works because it uses `IOEthernet` instead.
- Bugs are expected, please report them.
- Bluetooth requires a workaround to work. Please see [here](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/issues/437#issuecomment-1579931908)
--------------------------------------------------------------------------------------------------------------------------------------------------------

### How to install:
- In macOS, download the [InstallAssistant.pkg](https://swcdn.apple.com/content/downloads/23/44/032-94352-A_DB05J15QWT/4x91v0yzolyiat5cat76ieu0h78aeu3d03/InstallAssistant.pkg) from Apple 
- Follow the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#setting-up-the-installer) for making a USB within macOS via `createinstallmedia`. You can skip this if you are peforming an OS upgrade (clean install people will need to do this step)
- Add `-lilubetaall` to your boot-args. This is the most important step.
- Update or install macOS 14.


>**Note**: Only clean install has been verified working. OS upgrade is untested.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### FAQ
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

> Can I do this on Windows?

No. You will need macOS to create this installer. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Need help?
You have several options if you still need help. 

- Join the Chrultrabook Discord: https://discord.com/invite/tkPTk5w. Ask for help in #hackintosh.
- ~~Post in r/chrultrabook. https://www.reddit.com/r/chrultrabook/~~ Unavaliable as of June 12 2023 due to the blackout.
- Open a Issue in this repo (less effective)


--------------------------------------------------------------------------------------------------------------------------------------------------------

#### Other
# Note: THIS IS A OUTDATED VERSION. DO NOT FOLLOW THIS.

¹ SHYVANA users (C434 and C433) will need `VoodooI2CHID.kext` for touchscreen to function.

² All variants (C425, C433, C434) will work.

³ USBToolBox is the reccomended USB mapping tool.
