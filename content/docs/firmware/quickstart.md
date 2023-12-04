---
weight: 210
draft: false
title: "Flashing FW on CrOS"
icon: "rocket_launch"
description: "Enabling DevMode + Flashing FW"
---

## Requirements

- A Chromebook
- A USB drive larger than 8 gigabytes
- Internet connection
- Yo brain

## Step 1: Enabling Developer Mode

1. Press the `esc` + `refresh` + `power` buttons all at once on your Chromebook. Doing so will bring you to a screen that will prompt you to insert a USB stick or SD card.
2. On that screen, press `ctrl` + `d`. It will prompt you if you want to turn off OS verification. Press `ENTER`. Your Chromebook will restart and present a screen that says OS verification is OFF.
3. ChromeOS will then display a message for 30 seconds about transitioning the system to developer mode.
4. Once done, the Chromebook will restart into developer mode. Now, you can flash custom firmware.


## Step 2: Disabling Write Protection:

1. Open up the back of your Chromebook. There are screws under all 4 rubber feet.
2. Find the cable going from the battery to the motherbord
3. Pull back the metal cover protecting the cable.
4. Gently lift up the cable.
5. Plug in your charger, then boot chromeOS **with the battery unplugged.**
6. Flash FW (see below)

## Step 3: Firmware

To keep it breif, UEFI firmware allows the Chromebook to become any other laptop. This means you will lose access to chromeOS. RW_Legacy allows you to dualboot chromeOS along with Linux.

1. Open a terminal via VT-2 by pressing `ctrl` + `alt` + `→`.
2. Log in as user `chronos`. Note that there should be no password unless you set one yourself.
3. Run the Firmware Utility Script:
  ```shell
cd; curl -LO mrchromebox.tech/firmware-util.sh && sudo bash firmware-util.sh
```

If you encounter certificate related errors when downloading the script from ChromeOS, then add -k to the curl command and script command to bypass SSL certificate checking as so:
```shell
cd; curl -LOk mrchromebox.tech/firmware-util.sh && sudo bash firmware-util.sh
```

{{< alert context="warning" text="The Firmware Utility Script cannot be run from a crosh shell, or a crostini shell." />}}

4. Select the firmware you would like to flash.
5. Follow the on-screen prompts. Once you are done, you should reboot into the new firmware! Keep in mind that the first boot will always take longer due to a process called "memory training". DO NOT power off the device at any time during the first boot.

