# MacPro41-PowerProfile kext

Fix for Intel SpeedStep and GPU Power Managment on a MacPro4,1 configured as a MacPro6,1. Mainly used for Hackintosh.

Provides:
- ACPI_SMC_PlatformPlugin with a IOPlatformThermalProfile (taken from ACPI_SMC_PlatformPlugin.kext)
- AGPM with a Machine profile (taken from AppleGraphicsPowerManagement.kext)

The profiles are an exact copy of the MacPro4,1 profiles but with all references to "4,1" renamed to "6,1". Taken from macOS Mojave 10.14.4.

**Tested and confirmed working on ASRock X58 Deluxe with an Intel i7-920, macOS Mojave (10.14.4).**

Extension is signed so you can load it from /Library/Extensions without disabling extension signing, if you prefer.

## Background and Motivation
Using a MacPro6,1 (Mac Pro - Late 2013) SMBIOS on older hardware allows you to use Continuity (https://support.apple.com/en-gb/HT204689). But doing so on a pre-Ivy Bridge CPU means you lose Intel SpeedStep functionality unless you have proper DSDT/SSDTs. This is because pre-Ivy Bridge CPUs are matched by macOS to the ACPI_SMC_PlatformPlugin.kext instead of the newer X86PlatformPlugin.kext irrespective of your SMBIOS configuration, and ACPI_SMC_PlatformPlugin does not contain a profile for the MacPro6,1. The result is that your CPU will run at a low clock multiplier, significantly impacting performance.

This codeless kext injects the correct profile information needed by ACPI_SMC_PlatformPlugin. As there is no MacPro6,1 profile defined there, there's no conflict either. It may help with sleep on these platforms too, but i'm not sure on that. As of version 2.0, it also overrides AppleGraphicsPowerManagement's MacPro6,1 profile. Profile information taken from the respective kext's MacPro4,1 profile.

Used the MacPro4,1 profile as thats the closest to my hardware (Intel  i7-920 Nehalem, X58 chipset).
