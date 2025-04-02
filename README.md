![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Surface-Go-2-OpenCore
macOS on the Core m3-8100Y Microsoft Surface Go 2 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

> [!IMPORTANT]
> This EFI folder only supports the `Core m3-8100Y Microsoft Surface Go 2`. Surface Go 2 models with the `Pentium Gold` processors are not supported.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target device    | Microsoft Surface Go 2 with Core m3-8100Y processor |
| Target OS        | Apple macOS 13 Ventura, 14 Sonoma and 15 Sequoia |
| OpenCore         | [MOD-OC v1.0.5](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.5_b56882c/OpenCore-Mod-1.0.5-RELEASE.zip) |
| SMBIOS           | MacBookAir8,1 |
| SSD format       | APFS file system, GPT partition table |

## Abstract
Apart from the front and rear cameras, the IR camera (Windows Hello) and the LTE modem, everything on the Core m3-8100Y version of the Surface Go 2 is working perfectly like on a real Mac. ACPI S3 Sleep is broken, but ACPI S4 Hibernate works great, though, and resuming from Hibernation takes around ten seconds. The advantage Hibernate has over Sleep is that the device doesn't drav any power while in a hibernated state.

The Surface Go 2 works great as a handy little macOS tablet. It won't entirely replace an iPad or even an Android tablet, but once set up properly, macOS is actually quite a nice tablet OS and almost on par with the Windows tablet experience. All the fancy Trackpad gestures available on macOS work on the Touchscreen as well and are very smooth and reliable. Folding the Type Cover behind the tablet disables the Keyboard and Trackpad and folding the Type Cover to the laptop position again re-enables both of them.

The battery runtime is around five hours.

> [!TIP]
> I recommend installing `macOS 13 Ventura` rather than the newer `macOS 14 Sonoma` or `macOS 15 Sequoia`. The builtin Intel Wireless chip works almost perfectly with Apple's iServices and Continuity features on Ventura while those features are partially broken at the moment on newer versions of macOS.

> [!IMPORTANT]
> For macOS to be able to boot on the Surface Go 2, the `Secure Boot` option _**must be disabled**_ [in the UEFI](https://github.com/jlempen/Surface-Go-2-OpenCore/blob/main/README.md#uefi-settings). The boot screen will then display a large red bar with a padlock symbol at the top of the display when Secure Boot is disabled.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources. For those who wish to improve their hackintoshing knowledge, [5T33Z0's OC-Little-Translated](https://github.com/5T33Z0/OC-Little-Translated) repository is the most comprehensive resource I've found on the subject.

Although this repository is a fork of [kingo132's Surface Go 2 hackintosh repository](https://github.com/kingo132/surface-go2-hackintosh), my OpenCore EFI folder was built from scratch by following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide) with the latest releases of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) and every required kexts. Nevertheless, [kingo132's repository](https://github.com/kingo132/surface-go2-hackintosh) provided an extremely valuable and helpful resource for building my OpenCore EFI folder and hacking the tablet's UEFI Firmware.

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Surface Go 2 with this repository's EFI folder.

The `kexts` required to enable the trackpad and the touchscreen are special versions of `VoodooI2C.kext` and `VoodooI2CHID.kext` [patched and improved by lazd](https://github.com/jlempen/Surface-Go-2-OpenCore/issues/1#issuecomment-1705597716). Updating those `kexts` with the official ones from [the VoodooI2C repo](https://github.com/VoodooI2C) will most certainly break trackpad and touchscreen functionality! They were renamed to `VoodooI2C-SurfaceTouch.kext` and `VoodooI2CHID-SurfaceTouch.kext` so as not to be auto-updated by tools such as [OCAT](https://github.com/ic005k/OCAuxiliaryTools).

`AirportItlwm-Ventura.kext`, `AirportItlwm-Sonoma140.kext` and `AirportItlwm-Sonoma144.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are required to enable the Wifi chip and were renamed for the same reason. This EFI will dynamically load the appropriate kext for macOS Ventura or Sonoma depending on the running kernel. No need to manually replace the kext file when updating your version of macOS. As the Intel Wifi chip does not yet work with the `AirportItlwm.kext` in macOS Sequoia, you'll need to use the `Itlwm.kext` and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) to connect to a Wifi network. You'll find the latest stable `HeliPort.dmg` in the [Tools folder](https://github.com/jlempen/Surface-Go-2-OpenCore/blob/main/Tools/HeliPort.dmg) of this repo. This EFI will dynamically load the `Itlwm.kext` instead of `AirportItlwm.kext` when you boot into macOS Sequoia.

Windows and Linux should be detected automagically by the OpenCore boot loader even when installed after macOS.

This repository uses the unofficial OpenCore_NO_ACPI_Build fork of OpenCore by [btwise](https://gitee.com/btwise/OpenCore_NO_ACPI), wich is not endorsed by Acidanthera (the dev team behind OpenCore). The main (and only) difference between this fork and the official OpenCore version is that it allows to prevent ACPI injection (e.g. patches, tables, boot parameters) into other OSes besides macOS.

<details>
  <summary>Computer Specifications</summary>
  
## Computer Specifications
| Device           | Hardware                           |
| ---------------- | ---------------------------------- |
| CPU              | Intel Core m3-8100Y (1.1 - 3.4 GHz, Amber Lake-Y) |
| iGPU             | Intel UHD Graphics 615 |
| Audio            | Realtek ALC298 |
| RAM              | 2x4 GB LPDDR3 1867 MHz |
| Wifi + Bluetooth | Wifi6 AX200, Bluetooth 5.0 |
| Storage          | Kioxia/Toshiba 128GB SSD |
| USB Type-C 3.1 Gen 1 | Supports Power Delivery and DisplayPort |
| MicroSD Card reader | Realtek PCI-E Card Reader |
| Front & Rear camera | Intel(R) AVStream Camera 2500, ISP Interface, 8 MPix/5 MPix |
| IR camera | Intel(R) AVStream Camera 2500, ISP Interface |
| Keyboard / Trackpad | Microsoft Type Cover |
| Display | 10.50 inch 3:2, 1920 x 1280 pixel 220 PPI, 10-Point Capacitive |
| Touchscreen | `ELAN9038`, `\_SB.PCI0.I2C1.TPL1` |
| Screen | BOE CQ NV105WAM-N31, BOE088B |
| NFC | NXP NFC Client Device, NXP3001 |
| Battery | 26.81Wh 7.66v 3500mAh |
| LTE (if available) | Surface Mobile Broadband, USB device, Qualcomm Snapdragon X16 LTE |
| GPS (if available) | |
| Accelerometers, gyroscopes, ambient light sensors | |
</details>

<details>
  <summary>What works</summary>
  
## What works
- [x] CPU power management
- [x] CPU SpeedStep
- [x] iGPU with full acceleration
- [x] SSD drive
- [x] USB-C port
- [x] WLAN
- [x] Bluetooth
- [x] Internal speakers, microphone and Combojack
- [x] Power, volume up and volume down buttons
- [x] Surface Type Cover keyboard with working brightness, volume and mute keys, working caps lock light
- [x] Surface Type Cover trackpad with native multi-touch gestures
- [x] Touchscreen
- [x] Surface Pen
- [x] MicroSD Card reader
- [x] Battery percentage and cycle count
- [x] Hibernation (hibernatemode 25) - the device successfully wakes up from hibernation mode
- [x] USB Type-C to HDMI
- [x] USB Type-C to USB3 & USB2
- [x] USB Type-C Power Delivery
</details>

<details>
  <summary>What needs some more work</summary>
  
## What needs some more work
- [ ] Sleep (hibernatemode 3) - the device wakes up from sleep displaying the Surface logo on the internal display
- [ ] Accelerometers, gyroscope
- [ ] Ambient light sensor
</details>

<details>
  <summary>What will probably never work</summary>
  
## What will probably never work
- [ ] Front & rear cameras
- [ ] IR camera (Windows Hello)
- [ ] NFC
- [ ] LTE
</details>

<details>
  <summary>UEFI Settings</summary>
  
## UEFI Settings
To enter the UEFI Settings, power on your Surface Go 2 and hold the `Volume Up Button` (the left volume button) on the tablet as soon as the Surface Logo is displayed on the screen.

Apart from the `Secure Boot` setting, which ***must be disabled to boot macOS***, the following settings are not mandatory for macOS to work, but keep in mind that every disabled device saves power and increases the battery runtime. As the front, rear and IR cameras, as well as the LTE modem won't work in macOS, disabling them is recommended unless you use another operating system on the device. I also recommend moving `USB Storage` to the top of the boot configuration list, which makes booting from an USB stick much easier.

| Security | |
| -------- | ----- |
| Secure Boot | Disabled |
| Trusted Platform Module (TPM) | Disabled |
| Simultaneous Multithreading | Enabled |

| Devices | |
| -------- | ----- |
| Front Camera | Disabled |
| Rear Camera | Disabled |
| IR Camera | Disabled |
| LTE | Disabled |

| Boot configuration | |
| -------- | ----- |
| USB Storage | Move the item to the top of the list |
</details>

<details>
  <summary>Disabling Sleep and enabling Hibernate</summary>
  
## Disabling Sleep and enabling Hibernate
As we still haven't found a solution for the Sleep/Wake issues on the Surface Go 2, disable Sleep altogether and use Hibernate for now. Open the `Terminal` and enter the following commands, then reboot for the changes to take effect:
```
sudo pmset restoredefaults
sudo pmset -a hibernatemode 25
```
If for whatever reason Hibernate is not working on your system, you should reset the `Power Management` settings and rebuild the `sleepimage` file. To do so, open the `Terminal` and enter the following commands, then reboot for the changes to take effect:
```
sudo rm /Library/Preferences/com.apple.PowerManagement*
sudo rm /var/vm/sleepimage
sudo pmset hibernatefile /var/vm/sleepimage
```
Once you are back in macOS, disable Sleep and enable Hibernate again, then reboot:
```
sudo pmset restoredefaults
sudo pmset -a hibernatemode 25
```

Keep in mind that once the Surface Go 2 hibernates, you need to let it hibernate for a couple of minutes before waking it up. Failing to do so will disrupt hibernation and the device will then wake up on the dreaded black screen with the Surface logo without the red bar and padlock. You'll have to turn it off forcefully by pressing on the power button for 10 seconds.
</details>

<details>
  <summary>Turning off BD PROCHOT</summary>
  
## Turning off BD PROCHOT
On this device, BD PROCHOT will activate at temperatures as low as 60°C ~ 70°C. When it kicks in, the CPU will throttle down to 0.4Ghz, making the device more or less unusable. That's why BD PROCHOT needs to be disabled in order to increase the performance of the machine.

You may use [arter97's DisablePROCHOT UEFI extension](https://github.com/arter97/DisablePROCHOT) which is already included in the EFI/OC/Drivers folder to disable BD PROCHOT. However, if the CPU continues to be fully loaded with BD PROCHOT disabled, the temperature may increase up to 90°C. Beyond 90°C, the device will become unstable and either crash or power off. To avoid this, the temperature needs to be kept under control.

The DisablePROCHOT.efi driver is included but disabled in both config.plist files, as enabling it leads to the CPU overheating and the computer shutting down during install. You may enable the driver once macOS is installed on the device.
</details>

<details>
  <summary>Undervolting to reduce heat and improve performance</summary>
  
## Undervolting to reduce heat and improve performance
The `VoltageShift.kext` undervolting tool is already included and enabled in this repository's `Kexts` folder. To be able to launch the `voltageshift` command line tool from anywhere, copy [VoltageShift from the Tools folder](https://github.com/jlempen/Surface-Go-2-OpenCore/blob/main/Tools/VoltageShift-EFI.zip) to your `/usr/local/bin` folder by entering `sudo cp voltageshift /usr/local/bin/` in the macOS terminal after navigating to the folder where you downloaded and unzipped the `voltageshift` executable file.

Please refer to the instructions found in the [VoltageShift repository](https://github.com/sicreative/VoltageShift), as well as to the excellent howto found in [zearp's repository](https://github.com/zearp/Nucintosh#undervolting).

Once you have found an undervolting configuration that works well on your device, make it permanent by entering the following command in the terminal after navigating to the folder where you downloaded and unzipped the `VoltageShift-EFI.zip` file:
> sudo ./voltageshift buildlaunchd -120 -50 -80 0 0 0 1 28 18 0.002 60

The above settings are quite conservative and seem to work well on pretty much every Surface Go 2.
</details>

<details>
  <summary>Disabling CFG Lock to improve Power Management</summary>
  
## Disabling CFG Lock to improve Power Management
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Disable CFG Lock`
4. Press enter
5. Restart

To verify this worked, press Space and select the `Check CFG Lock State` tool -- if it was successful, you'll see:

> This firmware has UNLOCKED MSR 0xE2 register!

Finally, edit `config.plist` and set `Kernel -> Quirks -> AppleCpuPmCfgLock` and `Kernel -> Quirks -> AppleXcpmCfgLock` to `false` and reboot.
</details>

<details>
  <summary>Increasing DVMT Gfx Memory to improve iGPU performance</summary>
  
## Increasing DVMT Gfx Memory to improve iGPU performance
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Set DVMT Pre-Allocated to 64M`
4. Press enter
5. Then select `Set DVMT Total Gfx Mem to MAX`
6. Press enter
7. Restart

Finally, edit `config.plist` and delete or comment out `framebuffer-fbmem`, `framebuffer-stolenmem` and `framebuffer-patch-enable` in `Device Properties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)` and restart.
</details>

<details>
  <summary>More information on the modified UEFI Firmware variables</summary>
  
## More information on the modified UEFI Firmware variables 
| VarName | VarOffset | VarStore | From | To |
| ---------------- | -- | -- | --------- | --------- |
| CFG Lock | 0x3C | 0x3 (CpuSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| VT-d | 0xE3 | 0x2 (SaSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| DVMT Pre-Allocated | 0xDF | 0x2 (SaSetup) | 0x1 (32M) | 0x2 (64M) |
| DVMT Total Gfx Mem | 0xE0 | 0x2 (SaSetup) | 0x2 (256M) | 0x3 (MAX) |

To revert all changes made to the UEFI Firmware variables to their default values, enable the corresponding entries in the `config.plist` file under `Misc` -> `Tools`, restart to the OpenCore menu, press space to see the list of tools and revert the changes by launching the option you wish to revert to its default value:
- `Enable CFG Lock`
- `Enable VT-d`
- `Set DVMT Pre-Allocated to 32M`
- `Set DVMT Total Gfx Mem to 256M`

Repeat for every UEFI variable you wish to revert to its default value.

***Please be aware that you need to revert any changes made to your `config.plist` file before reverting the UEFI variables to their default values, or macOS won't boot anymore!***
</details>

<details>
  <summary>Fix broken Bluetooth on Wake from Hibernation</summary>
  
## Fix broken Bluetooth on Wake from Hibernation
After the device wakes up from Hibernation, Bluetooth may be broken / unable to connect.

A very simple fix for this issue is to [download and install Bluesnooze](https://github.com/odlp/bluesnooze). Launch the app, enable `Launch at login` and you're done!
</details>

<details>
  <summary>Enabling native HiDPI display settings in macOS</summary>
  
## Enabling native HiDPI display settings in macOS
On the installed macOS system, the default display resolution is too small for a small device such as the Surface Go 2. To enable the native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(4) 1920x1280 Display`. This fork of [xzhih's one-key-hidpi tool](https://github.com/xzhih/one-key-hidpi) was modified to add the 1920x1280 resolution needed for the Surface Go 2.
</details>

<details>
  <summary>Rotating the display</summary>
  
## Rotating the display
### Rotating the display with the Display Rotation Menu widget
[Display Rotation Menu](https://www.magesw.com/displayrotation/) by Mage Software is a free app designed to quickly rotate the display between Landscape, Portrait, Landscape Flipped or Portrait Flipped right from a menu bar widget. There's also a handy keyboard shortcut to rotate the display back to Landscape: `CTRL-OPTION-COMMAND-0` (as in "zero").

<img width="282" alt="Display Rotation Menu" src="https://github.com/user-attachments/assets/ac058eda-6a75-4979-9f3c-907a8837f07f">

The first time you rotate the display to a portrait orientation, the resolution is set to 1280x1920, which is way too small for the SGO2's display. You'll need to head over to the `Displays` tab in the `System Settings` and set the only other available resolution, 640x960, which is quite perfect on our Surface Go 2 in portrait orientation.

However, once the display is in portrait orientation, it is very likely that the menu bar will not be able to show the Display Rotation Menu widget anymore because there's not enough space to display every widget in the narrow portrait-mode menu bar. To fix this, hold down the `Command` key while dragging the Display Rotation Menu widget next to the macOS `Control Center` widget. If there is still not enough space to show the widget, you could also change the size of the clock by disabling the `Show date` and/or `Show the day of the week` items or even change the style of the clock to `Analog` in the `Clock Options` of macOS.

[Download Mage Software's Display Rotation Menu v1.5](https://www.magesw.com/displayrotation/DisplayRotationMenu_1.5.zip)

### Rotating the display with BetterTouchTool
[BetterTouchTool](https://folivora.ai/) by folivora.ai is a great, feature packed app that allows you to customize various input devices on your Mac.

With BetterTouchTool, you could define customised Trackpad/Touchscreen gestures to rotate your Surface Go 2's display. 

You could for instance assign a Four-Finger Double-Tap gesture to the Display Rotation Menu widget's keyboard shortcut `CTRL-OPTION-COMMAND-0` (as in "zero") to switch back to landscape orientation. 

Or you could use BetterTouchTool together with jakehilborn's [displayplacer command line tool](https://github.com/jakehilborn/displayplacer) to define Rotate Left and Rotate Right gestures which would execute Async, non blocking Terminal Commands such as `/usr/local/bin/displayplacer degree:0` for landscape orientation and `/usr/local/bin/displayplacer degree:90` for portrait orientation.

[Download BetterTouchTool](https://folivora.ai/releases/BetterTouchTool.zip) by folivora.ai

[Download displayplacer](https://github.com/jakehilborn/displayplacer/releases/tag/v1.4.0) by jakehilborn
</details>

<details>
  <summary>Enabling the On-Screen Keyboard</summary>
  
## Enabling the On-Screen Keyboard
macOS has a very nice On-Screen Keyboard readily available in the Accessibility Settings. To enable it, navigate to `System Settings -> Accessibility -> Accessibility Keyboard -> Enable`. When you minimize the keyboard, it shrinks down to a small button that can be dragged to a convenient location in a corner of the screen. There are plenty of options to configure the keyboard to your liking.

### Enabling the On-Screen Keyboard on the Lock Screen
Navigate to `System Settings -> Lock Screen -> Accessibility Options -> Accessibility Keyboard -> Enable` to enable the On-Screen Keyboard on the Lock Screen.

### Showing/hiding the On-Screen Keyboard with touchscreen gestures
The most convenient way to show/hide the On-Screen Keyboard when using the Surface Go 2 as a tablet is with a touchscreen gesture. Here's how to set this up: navigate to `System Settings -> Accessibility -> Shortcuts` and uncheck every option but `Accessibility Keyboard`. This enables the keyboard shortcut `OPTION-COMMAND-F5` to show/hide the On-Screen Keyboard. Now use [BetterTouchTool](https://folivora.ai/) to assign this keyboard shortcut to a Trackpad/Touchscreen gesture. 
</details>

<details>
  <summary>Fixing broken Apple Messages and FaceTime</summary>
  
## Fixing broken Apple Messages and FaceTime
To fix issues with Apple Messages and FaceTime related to the [Intel Wireless driver](https://github.com/OpenIntelWireless/itlwm) on macOS Sonoma, disable all `AirportItlwm-***.kext` entries under `Kernel -> Add` in your `config.plist` file and use the [itlwm_v2.3.0_stable.kext.zip](https://github.com/OpenIntelWireless/itlwm/releases/download/v2.3.0/itlwm_v2.3.0_stable.kext.zip) and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases/download/v1.4.1/HeliPort.dmg) instead.
The latest version 2.3.0 of `itlwm.kext` is already included in the Kext folder and `config.plist` file.

In addition to the above, to enable `itlwm.kext` under macOS Ventura and macOS Sonoma, you need to delete any text (i.e. `24.0.0` and `24.99.99` respectively) in the `MinKernel` and `MaxKernel` fields under `Kernel -> Add -> itlwm.kext` in your `config.plist` file.
</details>

<details>
  <summary>Related repositories</summary>
  
## Related repositories
* https://github.com/kingo132/surface-go2-hackintosh
</details>
