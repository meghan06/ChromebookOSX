## Install the latest version(s) of macOS on an Asus C425/C433/C434¹ ²

[![License](https://img.shields.io/badge/license-GPL-blue)](https://www.gnu.org/licenses/gpl-3.0.en.html) [![Status](https://user-images.githubusercontent.com/77316348/230705808-40c7ba6b-9b4f-41fb-8c40-8e7db3b97ad0.png)](https://github.com/meghan06/ChromebookOSX)

### coreboot 4.20 (5/15/2023 release) is known to hang while booting. A fix has been published [below](#fixing-coreboot-420).

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
  - [For those updating](#for-those-updating)
  - [For those installing directly](#installing-ventura-directly)
  - [Fixing WiFI](#fixing-wifi-on-ventura)
- [4. Misc](#other)


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
| Keyboard backlight | Working              | With `SSDT-KBBl.aml` _**and**_ the custom `VoodooPS2.kext`.                                   |           
| Keyboard & Remaps  | Working              | With the custom `VoodooPS2.kext`.                                                             |
| eMMC Storage       | Working              | With `EmeraldSDHC.kext`and patched HPET                                                       |    
| SD Card Reader     | Not working          | WIP with `EmeraldSDHC.kext`.                                                                  |
| Headphone Jack     | Not working          | Unsupported codec                                                                             |
| HDMI Audio         | Working              | Working with AppleALC, thx [@bernsgtx](https://reddit.com/u/ogridberns)                       |
| HDMI Video         | Working              | Working OOTB, thx [@bernsgtx](https://reddit.com/u/ogridberns)                                |                                                                             
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


macOS 10.1x were tested on external USB drives, so eMMC support may vary. For best experience, just install Big Sur (11) or newer.

#### OpenCore:
  - 0.8.6
  - 0.8.7
  - 0.8.8
  - 0.8.9
  - 0.9.0
  
--------------------------------------------------------------------------------------------------------------------------------------------------------

### ⚠️ Disclaimer 

 > **Warning**:  **By continuing, you acknowledge that you have read and understood the contents of [LICENSE.md](LICENSE.md) and the [disclaimer](#%EF%B8%8F-disclaimer-%EF%B8%8F), and consent to their terms.**

The instructions outlined in this [GitHub repo](https://github.com/meghan06/ChromebookOSX) have the potential to cause permanent harm to your laptop, and you should be aware of this potential outcome before proceeding. I cannot be held accountable for any damage caused from following or disregarding these instructions. I make no assurances concerning the dependability or efficacy of the materials referenced in this repository.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### Requirements

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

> **Warning** Pay _close_ attention to the Chromebook specific parts in the Dortania guide, specifically in `Booter -> Quirks` and the iGPU `boot-args`.

> **Warning** Pay _very_ close attention to the following steps, if you miss **even one**, your Chromebook will lose some functionally and might not even boot.

> **Note**: C434 and C433 users (SHYVANA) will need [VoodooI2CHID](https://github.com/VoodooI2C/VoodooI2C/releases/).kext for touchscreen to function. It is bundled with the VoodooI2C package.

> **Note**: Those who are installing to an external disk like a USB drive can skip steps 9 and 10.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### **These steps are **required** for proper functioning.**

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
8. **Set your SMBIOS as MacBookAir8,1**. Ignore what Dortania tells you to use, MacBookAir8,1 works better with our Chromebook.
    > **Note** If you choose to use `MacBook10,1`, which also works, you will not have Low Battery Mode.
9. Switch the VoodooPS2 from acidanthera with this [custom build that's designed for Chromebooks](https://github.com/one8three/VoodooPS2-Chromebook/releases) for keyboard backlight control + custom remaps. 
   - Keyboard backlight SSDT (`SSDT-KBBL.aml`) can be found [here](https://github.com/one8three/VoodooPS2-Chromebook/blob/master/SSDT-KBBL.aml). Drag it to your ACPI folder.
     > **Note**: This SSDT only works with the custom VoodooPS2 linked above.
10. Download [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC/releases) for eMMC storage support. Put it in your Kexts folder. 
11. Download corpnewt's SSDTTime, then launch it and select `FixHPET`. Next, select `'C'` for default, and drag the SSDT it generated (`SSDT-HPET.aml`) into your `ACPI` folder. Finally, copy the patches from `oc_patches.plist` into your `config.plist` under `ACPI -> Patch`. 

    > **Warning** Steps 9 and 10 are **required** for macOS to recognize the internal eMMC disk. 

12. Map your USB ports³ before installing ~~to prevent dead hard drives, thermonuclear war, or you getting fired.~~ See [Misc. Information](#misc-information) for a note to USBToolBox users.    
13. Using corpnewt's SSDTTime, dump your DSDT, generate `SSDT-USB-RESET.aml`, drag it to your ACPI folder, and reload your `config.plist`. **Required** for working USB ports.

    > **Note**: You must do this or your USB ports won't work. USBToolBox users can skip this step.

14. Snapshot (cmd +r) or (ctrl + r) your `config.plist`. 

    > **Warning**: NEVER do clean snapshots (`ctrl/cmd+shift+r`) after adding your HPET patches, they will be **wiped**. Only do regular snapshots. (`ctrl/cmd+r`)

15. Install macOS and enjoy!

> **Note**: In depth information about OpenCore can be found [here.](https://dortania.github.io/docs/latest/Configuration.html)

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Fixing coreboot 4.2.0
coreboot 4.2.0 (5/15/2023 release) has a known issue where macOS will hang on boot. This is due to coreboot not defining CPU cores by default. To fix this, we'll use a SSDT to manually define them. Credits to [ExtremeXT](https://github.com/ExtremeXT) for the fix.

- If you haven't already, add the compiled version of [SSDT-PLUG-ALT](https://github.com/meghan06/croscorebootpatch) to your ACPI folder.


--------------------------------------------------------------------------------------------------------------------------------------------------------

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
```
>**Note**: These SSDTs were generated with [SSDTTime](https://github.com/corpnewt/SSDTTime), with the exception of SSDT-HDAS-OFF and SSDT-SDXC.

>**Note**: USBToolBox users don't need `SSDT-USB-RESET` or `SSDT-RHUB`

>**Note**: `SSDT-HPET` and it's `config.plist` patch is only required for eMMC support.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 2. Post Install 

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
- Format the drive as `APFS`
- Map your USB ports prior to installing macOS³ for a painless install. You **will** reget it if you don't. You can use [USBToolBox](https://github.com/USBToolBox/tool) to do that. You *will* need a second kext that goes along with it for it to work. [Repo here.](https://github.com/USBToolBox/kext). USBToolBox will not work without this kext. 
- `itlwm` is more stable & faster than `AirportItlwm` on macOS Ventura. 
- AppleTV and other DRM protected services may not work.
- Control keyboard backlight with left `ctrl` + left `alt` and `<` `>`. 
    - `<` to decrease, `>` to increase.
- To fix the battery life on Ventura, you can set Low Battery Mode to be always enabled on battery. You can also use CPUFriend to tweak power settings but it might break stability.
- eMMC will come up as an external drive in the boot picker since eMMC is just an embedded SD card. 
- To hide the drive picker, set `ShowPicker` to `False` in `Misc` ->` Boot` -> `ShowPicker`
- `AppleXcpmCfgLock` and `DisableIOMapper` can be enabled or disabled, they make no difference.
- It's worth noting that while it's recommended, coreboot already includes mapped USB ports, meaning that USB mapping is not required. Proceed at your  own risk if you decide to skip USB mapping.
- Make sure your `ScanPolicy` is set to `0`. eMMC will not be recognized if it's some other value.
- **USB ports will ONLY work with SSDT-USB-Reset / SSDT-RHUB.** 
- macOS will not sleep if you have USB devices plugged in 
>**Note**: SSDT-USB-Reset / SSDT-RHUB is not needed if using USBToolBox.

>**Warning**: The hotkey to show bootdrives does not work. Make a copy of your EFI with `ShowPicker` enabled if you need to boot from another drive in OpenCore.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. macOS Ventura
> **Note** Only for people installing macOS Ventura. 

Before we get started, you should know the following:
- Ventura will run a little hotter
- `AirportItlwm` is very broken on Ventura.

Stay on macOS 12 (Monterey) to avoid these issues.


--------------------------------------------------------------------------------------------------------------------------------------------------------

### For those updating:
> **Note**: The steps mentioned can be completed after updating, but you won't have WiFi. Recommended to do before updating.

1. Mount your EFI using corpnewt's MountEFI.
2. Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
3. Download and install Heliport if you haven't already. The most recent stable release will work fine.
4. Launch ProperTree and reload (`ctrl+r`) your `config.plist`. 
5. Start the update. (If you haven't already)
6. You are now ready for macOS Ventura! 
7. [See below](#fixing-wifi-on-ventura) for fixing WiFi.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Installing Ventura directly:

> **Note**: Windows and Linux users only.

1. Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
2. Open up your kext folder, and locate `itlwm.kext`. 
3. Open it, and find `Info.plist`.
4. Open ProperTree, navigate to where your `Info.plist` is, and open it.
5. Under `IOKitPersonalities -> itlwm -> WiFiConfig`, enter your WiFI details. Save and close when done.
7. Launch ProperTree and reload (`ctrl+r`) your `config.plist`. 
8. Boot recovery. There will be no WiFi logo/symbol, but you will have WiFi.

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


Do note that Heliport will report no WiFi upon logging in but keep in mind you actually do thanks to the edits we just made. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

#### Other

¹ SHYVANA users (C434 and C433) will need `VoodooI2CHID.kext` for touchscreen to function.

² All variants (C425, C433, C434) will work.

³ USBToolBox is the reccomended USB mapping tool.
