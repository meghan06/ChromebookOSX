---
weight: 220
draft: false
title: "Flashing custom coreboot UEFI"
icon: "circle"
description: "Flashing custom UEFI firmware for bug fixes"
---

{{< alert context="danger" text="Flashing firmware always has the ability to brick your device. Please proceed with caution." />}}


# Flashing Custom Firmware 
This is done to fix several notorious bugs in Chromeintoshes. You will suffer greatly if you don't do this.


## Obtaining a ROM
1. Visit the Chromeintosh repo for a zip of ROMs.
2. Download


## Flashing
1. Boot a live Linux USB. Fedora or Ubuntu are all valid choices.
2. Obtain your ROM (See above)
3. Download flashrom, then `chmod +x` it;
   ```cd; curl -LO https://tree123.org/chrultrabook/utils/flashrom-libpci38; chmod +x flashrom-libpci38```
4. Flash
   1. Backup your current rom, just in case things go wrong: `./flashrom-libpci38 -p internal -r current.rom`
   2. Flash your custom firmware: `sudo ./flashrom-libpci38 -p internal --ifd -i bios -w ROM.rom`
5. Assuming it said success on all checks, reboot.

{{< alert context="danger" text="Do not reboot if any of the checks failed. DO NOT SHUTDOWN THE LAPTOP. Ask for help in the chrultraook forum." />}}
