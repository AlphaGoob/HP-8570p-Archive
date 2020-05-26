# HP-8570p-Archive
Troubleshooting and Archive with Tools, KEXT and Drivers for Hackintosh on HP EliteBook 8570p

// Notes
* All tested on HP EliteBook 8570p (Please see the full guide/repo)
* Working and not working kext and drivers are added to this repo. 
* Troubleshooting and workarounds are added to the readme.

// Tested on
* macOS 10.13 High Sierra
* macOS 10.14 Mojave
* macOS 10.15 Catalina

// Guides (Only as References, not fully working)
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

# Drivers and KEXTs

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

# Troubleshooting

// Battery status
- There was an issue in my DSDT. There wasn’t a specific patch available but with a DSDT from another user i managed to get it working. I just needed to patch his DSDT for use with the Intel HD 4000. If you combine it with ACPIBatteryManager.kext it will work. 

// macOS slow boot (apfs_keybag_init:1583: Failed to initialize volume keybag, err = 2
- In the Rehabman guide i used he automaticly installed BrcmPatchRam2.kext and BrcmFirmwareRepo.kext. On Mojave and High Sierra they worked fine, but in Catalina this caused the OS to took almost 5 times as long to boot (1:19 from clover). 

// Touchpad not working or slow
- I started to work with VirtualSMC.kext. But after some troubleshooting it seems that FakeSMC.kext works beter for this laptop. After i changes to FakeSMC the trackpad works. For some reason it also is very slow in the macOS installer.

// No internet via ethernet in macOS installer
- I could start the installer of macOS very easily but i didn’t have a internet connection via ethernet (en0). after some searching I found out that IntelMausiEthernet.kext won’t work on newer clover versions (i used 5103 at first). After downgrading to Clover 4988 my ethernet was working again.

// iMessage/FaceTime/iCloud not working
- The first time i booted macOS my iCloud services didn’t work. I need to adjust my serial numbers in the SMBios in Clover Configurator (see the steps in the guide above). You can check the serial number at https://checkcoverage.apple.com/. If you don’t see any devices to match your generated serial number you are good to use it. 

// Catalina won’t boot or give a black screen after loading
- The first time is started Catalina with Clover it wasn’t working. On the other hand Mojave was working like a charm. I troubleshooted everything on Mojave and then use the mac upgrade from the app store to upgrade to Catalina. It worked! After that i only needed to do some basic troubleshooting for after the upgrade to Catalina.  Catalina can be difficult to troubleshoot because of the APFS system it uses. Almost all faults at boot will result in a APFS error. 

// MacOS won’t boot (End LoadRAMDisk, End RandomSeed, +++++)
- I needed to map out my RAM. I did this with ProperTree. If you need it you can edit my config.plist and find my ram. Just edit your brand etc. and you are good to go! 

// MacOS won’t Boot (Error loading kernel cache (0x9) or ACPI Error: Method parse/execution failed or apfs_module_start:1689: load: com.apple)
- It seems that osxaptiofix3drv-64.efi is not working. You need to use OsxAptioFixDrv-64.efi. You will find this EFI in my files on GitHub. 

// Siri won't hear my voice
- If Siri doesn’t recognize your microphone or you have a lot of noise on your microphone please repeat step 5 to 8. macOS sometimes randomly set the monitor slider up again.

# Workarounds

// SIP protection
* Even with SIP turned off mac has a super-super-root user in the form of a virtual disk. Disable it:
* `sudo mount -uw /`
* `killall Finder`

