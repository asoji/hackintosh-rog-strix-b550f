# Hackintosh - ASUS ROG Strix B550-F Gaming (Wi-Fi)
OpenCore EFI for ASUS ROG Strix B550-F Gaming (Wi-Fi) + AMD Ryzen 5 5600x + RX 570 8GB in a dual different GPU system.

![Hackintosh Picture](https://github.com/asoji/hackintosh-rog-strix-b550f/assets/99072163/7c591098-6471-47be-9063-db749e1c5455)

This will be updated throughout time as I use my hack more and figure things out and try to document em. **This is currently an incomplete README.**

All files will be found [here](EFI).


## Tiny backstory and friends stuff

This isn't my first time hackintoshing, probably the 3rd time as of making this repository, but things got haha quirky oh my go- *cough*.

This time, this config was originally adapted from an OC EFI config that my boyfriend [@bluebeargreen-2](https://github.com/bluebeargreen-2) gave me and wasn't working originally for my system, even tho we have nearly identical motherboards, so I tried modifying it and cleaning it up to get it to work on my system, thank you Love. 💖

I would not have gotten to this point without the help from my best friends either, mainly my friend group's Queen of Hackintosh herself, [@dynamyc010](https://github.com/dynamyc010), and the other nerd who helped a bit along the way, [@CephalonCosmic](https://github.com/CephalonCosmic), who helped me with a few kexts stuff, and recommendations from her own hack. Of course my lovely boyfriend above helped me out as well, pointing out some small things along the way or making kext recommendations to each other to try to get each other's hacks working.

## Resources
- [The OpenCore Guide itself](https://dortania.github.io/OpenCore-Install-Guide/)
- [Ryzentosh Guide](https://github.com/mikigal/ryzen-hackintosh)
- [r/hackintosh subreddit](https://reddit.com/r/hackintosh/)
- [O3C](https://velickovicdj.github.io/O3C/) for Sanity Checking
- A fuck ton of Googling and asking friends

## What Works and Doesnt Work?

### Works
- Apple Services [kiss iMessage everyone!]
- GPU Acceleration and DRM [works with all 3 of my monitors too, so wooo]
- CPU Power Management
- Realtek Ethernet via TX201 Card
- Sleep/Wake
- USB [already done by my boyfriend so I didn't have to worry about that]
- My NVMes
- Updates
- Most Intel MKL/Intel Memfast Stuff [Apply [Ryzen Patch](https://github.com/mikigal/ryzen-hackintosh/blob/master/Resources/ryzen_patch.sh) and [Intel Memfast Patch](https://github.com/mikigal/ryzen-hackintosh/blob/master/Resources/adobe_patch.sh)]
- No clue, tell me!

### Doesn't Work [either just doesn't work or CBA to deal with currently]
- Wifi/BT
- Intel Ethernet

### Haven't Tested
- Front Panel Audio
- Motherboard 3.5mm Audio


## The Build Itself

OpenCore Version: 0.9.7

Operating Systems: Windows 11 23H2, macOS Sonoma 14.2.1

SMBIOS: MacPro7,1

Theme: [BsxM1](https://github.com/blackosx/BsxM1)

### Parts used for macOS [some not included because not needed]

| Part Type   | Part Model                                                            |
|-------------|-----------------------------------------------------------------------|
| Motherboard | ASUS ROG Strix B550-F Gaming (Wi-Fi)                                  |
| BIOS        | v2803 [Yes, I know I'm quite out of date.]                              |
| CPU         | AMD Ryzen 5 5600x                                                     |
| GPU 1       | GIGABYTE GeForce RTX 3070ti VISION OC 8G [Disabled in OpenCore/macOS] |
| GPU 2       | MSI Radeon RX 570 Armor 8GB                                           |
| RAM         | 4 x 8GB G.Skill Ripjaws V DDR4-3200 CL16-18-18-38                     |
| NVMe 1      | SanDisk Ultra 3D 1TB [Windows Drive]                                  |
| NVMe 2      | Crucial P1 500GB [macOS Drive]                                        |
| Wi-Fi/BT    | AX200 [Not functional in macOS, drops connection]                     |
| Network 1   | Intel i225-V 2.5GbE [Not functional in macOS, drops connection]       |
| Network 2   | TP-Link TX201/Realtek RTL8125 2.5GbE                                  |
| Audio       | Realtek ALCS1220A [Untested, I use USB Audio]                         |


## OpenCore Configuration

### ACPI

| ACPIs                    |
|--------------------------|
| SSDT-CPUR.aml            |
| SSDT-EC-USBX-DESKTOP.aml |

### Device Properties

Reason why I have this is to disable my top slot NVIDIA GPU, and use only my bottom slot AMD GPU

| Add/Delete | Devices                                | Key         | Value | Type    |
|------------|----------------------------------------|-------------|-------|---------|
| Add        | PciRoot(0x0)/Pci(0x3,0x1)/Pci(0x0,0x0) | disable-gpu | YES   | BOOLEAN |

### Drivers

| Driver Name     |
|-----------------|
| AudioDxe        |
| HfsPlus         |
| OpenCanopy      |
| OpenRuntime     |
| ResetNvramEntry |
| ToggleSipEntry  |

### Kernel Patches

[Apply the almighty wonderful AMD Vanilla Patches](https://github.com/AMD-OSX/AMD_Vanilla/tree/master).

### Kexts

Some of these might not even be needed, I'll clean em up later :sakaShrug:

| Kext Name [IN ORDER]                  |
|---------------------------------------|
| Lilu                                  |
| VirtualSMC                            |
| WhateverGreen                         |
| AMDRyzenCPUPowerManagement            |
| SMCAMDProcessor                       |
| RestrictEvents                        |
| RadeonSensor                          |
| SMCRadeonGPU                          |
| AppleALC                              |
| NVMeFix                               |
| AppleIGC [basically doesn't work lol] |
| IntelMausi                            |
| AppleMCEReporterDisabler              |
| USBToolBox                            |
| UTBMap                                |
| IntelBluetoothFirmware                |
| IntelBTPatcher                        |
| AirportItlwm                          |
| LucyRTL8125Ethernet                   |
| BlueToolFixup                         |
| BrcmPatchRAM3                         |

### NVRAM

The CPU Name stuff was an absolute pain in the ass for me, but this seems to be reliable enough, so just change the `revcpuname` string.

| Add/Delete | UUID                                 | Key                    | Value                                                                            | Type   |
|------------|--------------------------------------|------------------------|----------------------------------------------------------------------------------|--------|
| Add        | 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 | DefaultBackgroundColor | 00000000                                                                         | DATA   |
| Add        | 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 | UIScale                | 01                                                                               | DATA   |
| Add        | 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 | revcpu                 | 1                                                                                | NUMBER |
| Add        | 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 | revcpuname             | 6-Core AMD Ryzen 5 5600x                                                         | STRING |
| Add        | 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 | rtc-blacklist          |                                                                                  | DATA   |
| Add        | 7C436110-AB2A-4BBB-A880-FE41995C9F82 | SystemAudioVolume      | 46                                                                               | DATA   |
| Add        | 7C436110-AB2A-4BBB-A880-FE41995C9F82 | boot-args              | -v debug=0x100 keepsyms=1 alcid=1 -lilubetaall revpatch=sbvmm,cpuname,pci,memtab | STRING |
| Add        | 7C436110-AB2A-4BBB-A880-FE41995C9F82 | csr-active-config      | 00000000                                                                         | DATA   |
| Add        | 7C436110-AB2A-4BBB-A880-FE41995C9F82 | prev-lang:kbd          | en-US:0                                                                          | STRING |
| Add        | 7C436110-AB2A-4BBB-A880-FE41995C9F82 | run-efi-updater        | No                                                                               | STRING |

### Platform Info

Make sure to fill out all of the SMBIOS stuff using GenSMBIOS, since it's stripped out of this config.

| System Info Name | Processor Type |
|------------------|----------------|
| MacPro7,1        | 1537           | 


## License
**None**, just use this as reference, if you somehow have the same specs I do, this might work for you, but me and my boyfriend said that about their config and oh boi, it did not, but it was a good starting point.