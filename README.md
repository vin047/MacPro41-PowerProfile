# MacPro41-PowerProfile kext

Fix for Intel SpeedStep and GPU Power Management on a MacPro4,1 configured as a MacPro6,1. Mainly used for Hackintosh.

Provides:
- ACPI_SMC_PlatformPlugin with a IOPlatformThermalProfile (taken from ACPI_SMC_PlatformPlugin.kext)
- AGPM with a Machine profile (taken from AppleGraphicsPowerManagement.kext)

The profiles are an exact copy of the MacPro4,1 profiles but with all references to "4,1" renamed to "6,1". Taken from macOS Mojave 10.14.4.

**Tested and confirmed working on ASRock X58 Deluxe with an Intel i7-920, macOS Mojave (10.14.5).**

**Extension is signed and notarized so you can load it from /Library/Extensions without disabling SIP, if you prefer.**

*NOTE: You can check CPU clock speed and multiplier by running HWMonitor (with FakeSMC sensor kexts installed) and running a CPU intensive task like prime95. You can check AGPM is loaded using IOReg, which should show AGPMEnabler and AGPMController in the device tree.*

## Background and Motivation
Using a MacPro6,1 (Mac Pro - Late 2013) SMBIOS on older hardware allows you to use Continuity (https://support.apple.com/en-gb/HT204689). But doing so on a pre-Ivy Bridge CPU means you lose Intel SpeedStep functionality unless you have proper DSDT/SSDTs. This is because pre-Ivy Bridge CPUs are matched by macOS to the ACPI_SMC_PlatformPlugin.kext instead of the newer X86PlatformPlugin.kext irrespective of your SMBIOS configuration, and ACPI_SMC_PlatformPlugin does not contain a profile for the MacPro6,1. The result is that your CPU will run at a low clock multiplier, significantly impacting performance.

This code-less kext injects the correct profile information needed by ACPI_SMC_PlatformPlugin. As there is no MacPro6,1 profile defined there, there's no conflict either.

As of version 2.0, it also adds a MacPro6,1 machine profile to AppleGraphicsPowerManagement, which also doesn't have a MacPro6,1 definition by default, so no conflict. This profile is required for macOS 10.14.4+, otherwise macOS will get stuck in a boot loop.

Profiles are exact copies of the respective kext's MacPro4,1 profile but with references changed to MacPro6,1.

Used the MacPro4,1 profile as thats the closest to my hardware (Intel  i7-920 Nehalem, X58 chipset).
