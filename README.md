# Installing macOS on an ASUS Chromebook C425

Turns out, this laptop works quite well with macOS.

<img src="Screenshot.png" width="1920">


I will **NOT** be providing the download for the EFI, make it yourself and you might learn a thing or two along the way. All of the BS I encountered has been documented here so you don't have to suffer as much :)

## Current Status


| **Feature**        | **Status**           | **Notes**                                                         |
|--------------------|----------------------|-------------------------------------------------------------------|
| WiFi               | Working              | Working                                                           |
| Bluetooth          | Working              | Working                                                           |
| Suspend / Sleep    | Working partially    | Only on battery power, working with `EmeraldSDHC.kext`            |
| Trackpad           | Working              | With `VoodooI2C.kext` and `VoodooI2CELAN.kext`                    |
| Graphics Accel.    | Working              | With `-igfxnotelemetryload` in the boot-args                      |
| Sound              | Not Working          | Only works with Bluetooth / USB sound adapter                     |
| Keyboard backlight | Working              | With `SSDT-KBBl.aml` and `VoodoolPS2-Chromebook.kext`             |                                           
| Keyboard & Remaps  | Working              | With `VoodoolPS2-Chromebook.kext`                                 |
| eMMC Storage       | Working              | With `EmeraldSDHC.kext`                                           |

### Requirements

Before you start, you'll need to have the following things to complete the process:

  - A external storage device (can range from a SD card to a USB Disk) for creating the installer.  
  - The latest OpenCore version (at least 0.8.8)   
# - An understanding that this process has the potential to damage / brick your device, potentially causing it to become inoperable.


### Disclaimer

The instructions outlined in this document have the potential to cause permanent harm to your laptop, and you should be aware of this potential outcome before proceeding. I cannot be held accountable for any damage resulting from following or disregarding these instructions and make no promises regarding the reliability or efficiency of the software contained in this repository.

### Installation

Here are the steps to go from a regular Chromebook to a macOS Install using OpenCore. 

### **The following steps are **requried** for proper functioning.
1. Flash your Chromebook with [MrChromebox's UEFI firmware](https://mrchromebox.tech) via his scripts. To complete this process, you must turn off write protection either by using a SuzyQable cable or temporarily removing the battery (latter is less cumbersome).

2. Setup your EFI folder using the [OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/).
    - Use Laptop Kaby Lake for your config.plist 
    - In your `config.plist`, search for `ProtectMemoryReigons`, and set it to `TRUE` if you want working shutdown/restart.
    - In your `boot-args`, add `watchdog=0` and `-igfxnotelemetryload`
    - Despite what the guide says, your SMBIOS should be `MacBookAir8,1`

3. Do switch VoodoolPS2 with this [custom build](https://github.com/one8three/VoodooPS2-Chromebook/releases) for keyboard backlight control + custom       remaps 
   - Keyboard backlight SSDT can be found [here](https://github.com/one8three/VoodooPS2-Chromebook/blob/master/SSDT-KBBL.aml). 

4. Download corpnewt's SSDTTime, open it, select the first option `FixHEPT`, choose `C` for default, and drag the SSDT it makes into your `ACPI` folder. Then, in the same folder, copy the patches from `oc_patches.plist` into your config.plist under `ACPI -> Patch`. Without it, eMMC won't be recognized by macOS. (this is a bug with EmeraldSDHC)

5. Install macOS and enjoy!

## macOS Ventura
#### Only for those who want to update to macOS Ventura.
Before we get started, you should know the following:
- Your battery will drain faster on Ventura. To avoid this, stay on Monterey or older.
- Intel WiFi works, but is a little iffy during startup. It'll take a few seconds (`~30s)` after login for it to connect.
- Logging out **will** hang your device. The cause for this is unknown.

With that, l3ts get started!

### Preparations
1. Mount your EFI using corpnewt's MountEFI.
2. Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
3. Download and install Heliport. The most recent stable release will work fine.
4. Open ProperTree and reload (`ctrl+r`) your `config.plist`. 
5. Start the update. 

You are now ready for macOS Ventura! 





## Credits
- **Goldfish64** for the eMMC driver and iGPU acceleration 
- **corpnewt** for his tools
- **olm3ca** for the help along the way 


### Last Updated: 02/01/2023
