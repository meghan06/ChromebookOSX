# Installing macOS on a ASUS Chromebook C425


## Current Status


| **Feature**        | **Status**           | **Notes**                                                         |
|--------------------|----------------------|-------------------------------------------------------------------|
| WiFi               | Working              | Working                                                           |
| Bluetooth          | Working              | Working                                                           |
| Suspend / Sleep    | Working partially    | Only on battery power, working with EmeraldSDHC.kext              |
| Trackpad           | Working              | With VoodooI2C.kext and VoodooI2CELAN.kext                        |
| Graphics Accel.    | Working              | With -igfxnotelemetryload in the boot-args                        |
| Sound              | Not Working          | Only works with Bluetooth / USB sound adapter                     |
| Keyboard backlight | Working              | With SSDT-KBBl.aml and VoodoolPS2-Chromebook.kext                 |                                           
| Keyboard & Remaps  | Working              | With VoodoolPS2-Chromebook.kext                                   |
| eMMC Storage       | Working              | With EmeraldSDHC.kext                                             |

### Requirements

Before you start, you'll need to have the following things to complete the process:

- A external storage device (can range from a SD card to a USB Disk) for creating the installer.
- A willingness to accept that this is a potentially destructive process that may brick your whole device.

### Disclaimer

The process described in this document could cause irreversible damage to your laptop, and you should prepare yourself for that outcome before you begin. I accept absolutely no responsibility for the consequences of anyone choosing to follow or ignore any of the instructions in this document, and make no guarantees about the quality or effectiveness of the software in this repo.

### Installation

Here are the steps to go from a regular Chromebook to a macOS Install using OpenCore. 

#The following steps are **requried** for proper functioning.
1. Flash UEFI firmware. Read and follow [MrChromebox's instructions](https://mrchromebox.tech) on how to flash the UEFI firmware using MrChromebox's scripts. To do this, you will need to disable write protect with either the SuzyQable cable or by removing the battery, with the second option being less painful.

2. Setup your EFI folder using the [OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/).
    - Use Laptop Kaby Lake for your config.plist 
    - In your config.plist, search for `ProtectMemoryReigons`, and set it to `TRUE` if you want working shutdown/install/WiFi.
    - In your boot-args, add `watchdog=0` and `-igfxnotelemetryload`
    - Despite what the guide says, your SMBIOS should be `MacBookAir8,1`

3. Do switch VoodoolPS2 with this [custom build](https://github.com/one8three/VoodooPS2-Chromebook/releases) for keyboard backlight control + custom       remaps 
   - Keyboard backlight SSDT can be found [here](https://github.com/one8three/VoodooPS2-Chromebook/blob/master/SSDT-KBBL.aml). 

4. Download corpnewt's SSDTTime, open it, select the first option `FixHEPT`, choose `C` for default, and drag the SSDT it makes into your ACPI folder. Then, in the same folder, copy the patches from `oc_patches.plist` into your config.plist under `ACPI -> Patch`. Without it, eMMC won't be recognized by macOS. (this is a bug with EmeraldSDHC)

5.) Install macOS and enjoy!

## macOS Ventura
Before we get started, you should know the following:
- Your battery will drain faster on Ventura. To avoid this, stay on Monterey or older.
- Intel WiFi works, but is a little wonky during startup. It'll take a few seconds after login for it to connect.
- Logging out **will** hang your device 

With that, l3ts get started!

### Preparations
- Mount your EFI using corpnewt's MountEFI.
- Under OC/Kexts, delete your old itlwm/AirportItlwm kext and replace it with `itlwm v.2.2.0 alpha`
- Download and install Heliport.
- Open ProperTree and reload (`ctrl+r`) your `config.plist`. 
You are now ready for macOS Ventura!



### Credits
- Goldfish64 for the eMMC driver and iGPU acceleration 
- corpnewt for his tools
- olm3ca for the help along the way 
