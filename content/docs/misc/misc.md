---
weight: 510
title: "Misc"
description: "Misc"
icon: menu_book
lead: ""
draft: false
---

# Misc

Random shit I thought was cool to share


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
- Format the drive as `APFS` and `GUID Partition Table / GPT`
- Map your USB ports prior to installing macOS³ for a painless install. You **will** reget it if you don't. You can use [USBToolBox](https://github.com/USBToolBox/tool) to do that. You *will* need a second kext that goes along with it for it to work. [Repo here.](https://github.com/USBToolBox/kext). USBToolBox will not work without this kext. 
- AppleTV and other DRM protected services may not work.
- Control keyboard backlight with left `alt` + `brightness up` or `brightness down`
- To fix  battery life, use CPUFriend to tweak power settings. 
- To hide the OpenCore boot menu, set `ShowPicker` to `False` in `Misc` ->` Boot` -> `ShowPicker`
- `AppleXcpmCfgLock` and `DisableIOMapper` can be enabled or disabled. There is no difference.
- It's worth noting that while it's recommended, coreboot already includes mapped USB ports, meaning that USB mapping is not required. Proceed at your own risk if you decide to skip USB mapping.
- eMMC will not be recognized if `ScanPolicy` is set to `0`.
- **USB ports will ONLY work with SSDT-USB-Reset / SSDT-RHUB.** 
- macOS will not sleep if you have USB devices plugged in 
>**Note**: SSDT-USB-Reset / SSDT-RHUB is not needed if using USBToolBox.

>**Warning**: The hotkey to show bootdrives does not work. Make a copy of your EFI with `ShowPicker` enabled if you need to boot from another drive in OpenCore.
