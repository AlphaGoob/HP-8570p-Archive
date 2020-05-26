# Hackintosh-General
Troubleshooting, Tools, KEXT and Drivers for Hackintosh

// Notes
* All tested on HP EliteBook 8570p
* Workarounds and settings are added to the readme file
* Working and not working kext and drivers are added to this repo. Read the readme for descriptions and problems. 

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

// SIP protection work around
* Even with SIP turned off mac has a super-super-root user in the form of a virtual disk. Disable it:
* `sudo mount -uw /`
* `killall Finder`
