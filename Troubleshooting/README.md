# Hackintosh-General
Troubleshooting for Hackintosh on the HP EliteBook 8570p

// Tested on
* macOS 10.13 High Sierra
* macOS 10.14 Mojave
* macOS 10.15 Catalina

// SIP protection
* Even with SIP turned off mac has a super-super-root user in the form of a virtual disk. Disable it:
* `sudo mount -uw /`
* `killall Finder`

// KEXT | Not working
- AirportBrcmFixup.kext - Not working for Broadcom 4312 chips
- AppleIntelCPUPowerManagement.kext - No Effect on the i5 3210m
- AppleIntelE1000.kext - Use IntelMausiEthernet.kext and clover 4988 instead of 5000+ (bug with IntelMausi in newer builds)
- AppleIntelE1000e.kext - Use IntelMausiEthernet.kext and clover 4988 instead of 5000+ (bug with IntelMausi in newer builds)
- ApplePS2SmartTouchPad.kext - Use voodoops2controller.kext instead.
- BrcmFirmwareRepo.kext - Will cause macOS to boot slow (apfs_keybag_init: 1583: Failed to initialize volume keybag, err = 2)
- BrcmNonPatchRAM2.kext - Will cause macOS to boot slow (apfs_keybag_init: 1583: Failed to initialize volume keybag, err = 2)
- BrcmPatchRAM2.kext - Will cause macOS to boot slow (apfs_keybag_init: 1583: Failed to initialize volume keybag, err = 2)
- IntelMausi.kext - Use IntelMausiEthernet.kext and clover 4988 instead of 5000+ (bug with IntelMausi in newer builds)
- USBInjectAll.kext - This laptop only has USB 2.0. Eveytime you plugin a USB device it will say "USB Device Disabled, Unplug the device using too much power to re-enable USB Devices".

// KEXT | Partly working
- IO80211Family.kext (Broadcom 4312 | BRCM94312) - It will show the correct WiFi card but you cannot find any network or connect. The card isn't supported from Sierra and above.  

// Drivers | Not working
- AptioInputFix.efi - End RandomSeed / End LoadRamDisk error. Use OsxAptioFixDrv-64.efi instead
- DataHubDxe-64.efi - ACPI Error: Method parse/execution failed. Removing it will fix the problem. 
- EmuVariableUefi-64.efi - macOS not booting at all. Use OsxAptioFixDrv-64.efi instead
- OsxAptioFix2Drv-64.efi - End RandomSeed / End LoadRamDisk error. Use OsxAptioFixDrv-64.efi instead
- OsxAptioFix3Drv-64.efi - End RandomSeed / End LoadRamDisk error. Use OsxAptioFixDrv-64.efi instead
- OsxAptioFix3Drv.efi - End RandomSeed / End LoadRamDisk error. Use OsxAptioFixDrv-64.efi instead
- VBoxHfs.efi - Not needed for APFS files
