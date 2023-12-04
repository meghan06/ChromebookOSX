---
weight: 320
title: "Installation"
description: "Installing macOS"
icon: menu_book
lead: ""
draft: false
---

# Installation

{{< alert context="info" text="Read through this more than once, to prevent errors. Read these steps carefully, they are required for proper functioning." />}}

Time for the soul-sucking stuff /j
 
 **You want to start with the [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) first.** Afterwards, we'll modify your EFI a little bit to make it work with our Chromebook.
   - Take your time and read everything in there throughly, it's good practice to do so.
   - For your `config.plist` setup, you want to follow the Laptop Kaby Lake section.
   - The Dortania guide is actually pretty up-to-date with Chromebook items so it should be a pretty smooth experience overall. We will cover them here anyway just in case you missed it.

### ACPI Folder
Everything else you need to do in your ACPI folder

1. Add the compiled version of [SSDT-PLUG-ALT.aml](https://github.com/meghan06/croscorebootpatch) into your ACPI folder.
2. Compile and add [the top row keyboard remap SSDT](https://github.com/1Revenger1/Acer-Spin-713-Hackintosh/blob/main/src/ACPI/SSDT-ChromeKeys.dsl)
3. Compile and add a [fake ambient light sensor SSDT](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-ALS0.dsl) to your ACPI folder. This is needed for working keyboard backlight.
4. Download SSDTTime, then generate a FixHPET SSDT. Select C for default, then drag the compiled version into your ACPI folder

### Kext Folder¹
Everything else you need in your Kext folder

1. Add the [eMMC driver](https://github.com/acidanthera/EmeraldSDHC/releases) to your kext folder.
2. Replace the default VoodooPS2 kext with one modified for [Chromebook keyboards](https://github.com/1Revenger1/VoodooPS2/releases)
3. Add the [EC kext](https://github.com/1Revenger1/CrosEC/releases). for a functional EC/kb backlight

### config.plist Patches
1. Your SMBIOS should be `MacBookAir8,1`
2. under `Booter -> Quirks` set `ProtectMemoryRegions` to `TRUE`. It should look something like this in your `config.plist` when done correctly:

   | Quirk                | Type | Value    |
   | -------------------- | ---- | -------- |
   | ProtectMemoryRegions | Boolean | True  |

   {{< alert context="danger" text="This MUST be enabled if you want working WiFi or NVRAM" />}}


4. Under `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)`, make the following modifications:
  
   | Key                  | Type | Value    |
   | -------------------- | ---- | -------- |
   | AAPL,ig-platform-id  | data | 0000C087 |
   | device-id            | data | C0870000 |
   | disable-telemetry-load | data | 01000000 |
   | rps-control | data | 01000000 |
     
{{< alert context="danger" text="These should be the only two items `in PciRoot(0x0)/Pci(0x2,0x0)`" />}}

5. Merge the HPET patches from Step 4 in ACPI to ACPI -> Patches.
6. Map USB using USBToolBox


#### Notes:

* ¹ SHYVANA users (C434 and C433) will need `VoodooI2CHID.kext` for touchscreen to function.
