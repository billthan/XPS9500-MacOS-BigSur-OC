# XPS9500-MacOS-BigSur-OC

This is a work in progress of the XPS 9500 (2020) running macOS 11.2.1

![About macOS](https://github.com/billthan/XPS9500-MacOS-BigSur-OC/blob/main/Screen%20Shot%202021-02-24%20at%207.24.24%20PM.png?raw=true)

## Introduction
The EFI included in this repository is to boot macOS 11.2.1 Dell XPS 15 9500. There are minor problems which will be discussed below. Do not use the EFI as-is, you must at least know how to generate your own serials with GenSMBIOS. There is a great guide on how to get started:[Opencore Install guide](https://dortania.github.io/OpenCore-Install-Guide/). Use this, as I have used it for creating this EFI.

## Specs

| Componenet             | Value                                                        |
| ---------------------- | ------------------------------------------------------------ |
| CPU                    | Intel Core i5 10300H                                         |
| GPU                    | Intel UHD Graphics 630                                       |
| Screen                 | 15.6" HD IPS Display.                                        |
| RAM                    | 8GB DDR4 2933MHz                                             |
| Internal SSD - MacOS   | KIOXIA  M.2 PCIe NVMe Class 35 2230 SSD - 256GB              |
| Internal SSD - macOS   | WD Blue™ NVMe SN550 M.2 2280 SSD 1TB.                        |
| Audio                  | Unknown                                                      |
| Wireless               | Intel Killer AX1650                                          |


Quick Note: My serial number, MLB, and UUID have been removed from the config.plist. Please use CorpNewt's [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to create your own.


## Functionality

|Function / Hardware|Status|
|-|-|
|iGPU UHD630 Acceleration|Working|
|CPU Power Management|Working - idles at 800MHz, boosts to max Turbo frequency|
|Laptop Keyboard|Working|
|Laptop Trackpad|Working|
|Laptop Headphones Jack|Working|
|Built-in Speakers|Working, distortion at high volumes|
|Built-in Mic|Working|
|Hotkeys for audio|Working|
|USB 3.x|Working|
|USB 2.0|Working|
|Fingerprint Sensor|Not working|
|SD Card Slot|Working|
|Screen brightness|Working, hotkeys fn+S/fn+B to decrease/increase brightness|
|Built-in Wifi|Working, intermittently drops.|
|Built-in Bluetooth|Working|
|Dell USB3.1 dock|Working|
|Built-in webcam|Working|
|Sleep|Dell BIOS bug (Enable "block sleep" in BIOS to disable S3 for now)|

---

## BIOS Settings ([source](https://github.com/zachs78/MacOS-XPS-9500-OpenCore))

Disable the following
 - SATA Mode set to `AHCI`
 - `VT-d` disabled
 - `TPM` disabled
 - Sleep (Enable block sleep)
 - SD Card slot
 - Touchscreen (if you have one)
 - Secure boot
 - Fingerprint reader
 - Disable CFG Lock (via modGRUBShell)

---

## How to disable CFG Lock ([source](https://github.com/zachs78/MacOS-XPS-9500-OpenCore))

This is specific to XPS 15 9500 only (along with its sibling models and previous gen).

Select the modGRUBShell option at startup (OpenCore boot selection page).
At the grub prompt, enter the following commands (the first line unlocks CFG, the second line unlocks overclocking).

```
setup_var CpuSetup 0x3e 0x0
setup_var CpuSetup 0xda 0x0
exit
```

Restart your laptop and boot into the BIOS. Do a factory reset. Now your CFG lock will be disabled. You can confirm that by running the VerifyMSR2 option.

If you update your BIOS, you may need to do this again but so far Dell has been kind to us.

---

