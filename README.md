# Hackintosh-General
Troubleshooting, Tools, KEXT and Drivers for Hackintosh

// Notes
* All tested on HP EliteBook 8570p (Please see the full guide/repo)
* Workarounds and settings are added to the readme file
* Working and not working kext and drivers are added to this repo. 
* Read the readme for descriptions and problems of "Not Working" kexts and drivers 

// Tested on
* macOS 10.13 High Sierra
* macOS 10.14 Mojave
* macOS 10.15 Catalina

// Guides (References)
* HP ProBook/EliteBook https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/
* Clover https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/
* RAM Mapping https://hackintosher.com/forums/thread/mapping-ram-and-dimm-slots-on-a-hackintosh-with-clover-smbios.365/

// Bios settings
- UEFI boot is enabled (hybrid/with CSM for best result)
- secure boot is disabled
- disable fast boot
- IGPU graphics memory set to 64mb, if available
- disable the serial port via BIOS option, if available
- disable "LAN/WLAN switching", if available
- disable "Extended Idle Power States" if you find it under "Power Management Options"
- disable "Wake on LAN" and "Wake on USB"
- disable Firewire/IEEE 1394, if your laptop has it
- for Skylake and KabyLake laptop, enable "Launch Hotkeys without Fn keypress"

# Troubleshooting
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
- AptioInputFix.efi
- DataHubDxe-64.efi
- EmuVariableUefi-64.efi
- OsxAptioFix2Drv-64.efi
- OsxAptioFix3Drv-64.efi
- OsxAptioFix3Drv.efi
- VBoxHfs.efi
