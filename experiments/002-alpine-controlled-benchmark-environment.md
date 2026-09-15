# Experiment 002 — Alpine Controlled Benchmark Environment

**Date:** 2026-09-14  
**Status:** In progress

## Question

Can Alpine Linux provide a minimal, controlled environment for establishing the practical hardware performance ceiling of the Lenovo IdeaPad 120S-11IAP?

## Hypothesis

A minimal Alpine Linux environment should introduce less unnecessary RAM usage and background system activity than the normal Ubuntu installation, allowing a larger proportion of the Lenovo's limited resources to be available for controlled benchmarking and later AI workloads.

## Hardware

- Device: Lenovo IdeaPad 120S-11IAP
- CPU: Intel Celeron N3350
- Physical RAM: 4 GB
- Internal storage: 32 GB SanDisk iNAND eMMC
- Architecture: x86-64

## Benchmark Environment

- OS: Alpine Linux 3.24.1
- Kernel: 6.18.35-0-lts
- Boot medium: external 128 GB microSD card via USB card reader
- Environment: minimal command-line Alpine system booted from external removable media
- Power: AC power
- Internal Ubuntu installation: left untouched

## Environment Preparation and Troubleshooting Log

### Alpine boot media

An Alpine Linux 3.24.1 x86-64 standard ISO was selected for the controlled benchmark environment.

The benchmark medium was prepared using a 128 GB microSD card connected through an SD adapter and USB card reader.

The ISO was initially stored directly on the microSD card. Before writing the boot image, a separate copy of the ISO was placed on the Windows laptop so that the source image would not be located on the device being overwritten.

Rufus 4.15 was used on a separate Windows laptop to write `alpine-standard-3.24.1-x86_64.iso` to the microSD card.

Rufus initially warned that the ISO could not be used while it was located on the target device. The ISO was therefore copied to the Windows laptop's internal storage and selected from there.

Rufus was configured with:

- Device: 128 GB microSD card, reported by Windows as 120 GB
- Image: `alpine-standard-3.24.1-x86_64.iso`
- Image mode: ISO Image mode
- Partition scheme: MBR
- Target system: BIOS or UEFI
- File system: Large FAT32

The resulting card was labelled `ALPINE-STD` and contained Alpine boot files including `boot/`, `apks/` and `efi/`.

### Chromebook preparation attempt

The initial attempt to prepare the boot medium was made from a Chromebook using the Linux (Penguin) environment.

ChromeOS could see the card as `Alpine Driv`, but the Linux container initially could not access the physical card.

USB access for the card reader was then enabled through ChromeOS's Linux USB device control. The card became visible to Penguin as `/dev/sdb`, with `/dev/sdb1` containing the exFAT filesystem.

An attempt to mount `/dev/sdb1` failed because the minimal Penguin environment did not have exFAT filesystem support:

`unknown filesystem type 'exfat'`

The ISO was subsequently copied to the Chromebook's local storage using ChromeOS, and the Windows laptop was used to perform the final boot-media creation instead.

### First Alpine boot

The Lenovo was powered off and the Alpine boot medium was connected through the USB card reader.

The Lenovo Novo Button Menu was used to access the Boot Menu.

Two entries appeared as:

`USB HDD: Generic MassStorageClass`

Selecting the first USB HDD entry booted the existing Ubuntu installation.

Selecting the second USB HDD entry found the Alpine boot image but produced:

`Secure Boot: Image failed to verify with security violation`

Secure Boot was disabled in the Lenovo firmware while leaving the boot mode otherwise unchanged.

The second USB HDD entry then successfully booted Alpine Linux.

### Initial Alpine environment

Alpine reached the `localhost login:` prompt and was entered using the `root` account.

The Alpine keyboard layout was initially unset. This caused problems entering some keyboard characters, including the backslash character.

`setup-keymap` was used to configure the keyboard layout as:

- Layout: `gb`
- Variant: `gb`

After this change, the keyboard behaved correctly for the UK keyboard.

### Initial system verification

Alpine reported:

- Alpine Linux: 3.24.1
- Architecture: x86_64
- Kernel: 6.18.35-0-lts

The initial minimal environment did not include `lscpu` or `lsblk`.

Initial memory state was:

- Total: 3.7 GiB
- Used: 124.5 MiB
- Free: 3.2 GiB
- Available: 3.3 GiB
- Swap: 0

Initial system activity was very low:

- Uptime: approximately 7 minutes
- Load average: 0.03 / 0.03 / 0.00

### Network configuration

The initial network interface check showed that `wlan0` existed but was down.

`setup-interfaces` was used to configure the wireless interface.

The interface was subsequently brought up manually with:

`ip link set wlan0 up`

`iw dev wlan0 link` confirmed that the Lenovo was associated with the Wi-Fi network.

However, no IPv4 address or routing table was initially present.

The lightweight Alpine DHCP client was then used:

`udhcpc -i wlan0`

A DHCP lease was successfully obtained.

Connectivity was verified using:

`ping -c 3 1.1.1.1`

and:

`ping -c 3 dl-cdn.alpinelinux.org`

Both succeeded, confirming IP connectivity, routing and DNS resolution.

### Package repository configuration

The initial Alpine repository configuration contained only:

`/media/usb/apks`

This provided only 95 available packages.

The repository configuration was replaced with the official Alpine v3.24 repositories:

`https://dl-cdn.alpinelinux.org/alpine/v3.24/main`

`https://dl-cdn.alpinelinux.org/alpine/v3.24/community`

After running `apk update`, Alpine reported:

`OK: 28648 distinct packages available`

This confirmed that the full online Alpine package repositories were available.

## Setup Lessons

The setup process demonstrated several practical differences between a minimal Alpine environment and a general-purpose Linux installation:

- Minimal Alpine does not necessarily include common diagnostic utilities by default.
- Removable storage access from a Chromebook Linux container may require explicit USB passthrough.
- The minimal environment initially lacked exFAT support.
- Lenovo firmware Secure Boot prevented the Alpine boot image from starting until Secure Boot was disabled.
- The keyboard layout must be explicitly configured for reliable command-line input.
- Wi-Fi association and obtaining an IPv4 address through DHCP are separate steps.
- The Alpine ISO repository bundled on the boot medium is much smaller than the full online Alpine repositories.

These issues were resolved without modifying the Lenovo's internal Ubuntu installation.

### Ephemeral Configuration

The Alpine environment is currently booted from external removable media without a persistent Alpine installation/configuration.
As a result, runtime configuration changes made during a session, including /etc/apk/repositories, do not persist after reboot.
After reboot, the repository configuration reverted to the ISO-provided /media/usb/apks repository. 
The online Alpine v3.24 main and community repositories therefore need to be reconfigured at the start of a new session when additional packages are required.


### Basic Hardware Inspection Tools

After restoring access to the online Alpine v3.24 repositories via ethernet as opposed to wifi, the lscpu and lsblk packages were searched for and installed successfully.

lscpu was used to verify that CPU, cache, instruction-set, virtualisation, NUMA and CPU vulnerability information could be inspected within the Alpine environment.

lsblk was used to verify storage topology. The environment detected both the approximately 112.3 GiB removable benchmark boot device and the approximately 29.1 GiB internal SanDisk iNAND eMMC device. 
No modifications were made to the internal eMMC.

The commands produced more output than could be practically captured in a single photograph; 
their successful execution and representative output were nevertheless verified directly on the Lenovo.

### SSH Remote Control Setup

OpenSSH server functionality was installed and configured in the Alpine environment to allow remote administration from the Chromebook. SSH host keys were generated and the SSH service was started successfully.

The default Alpine SSH configuration permitted password authentication but prohibited root password login. For this temporary laboratory environment, root password login was explicitly enabled to allow remote administration. This configuration is considered temporary and is not intended as the security model for a final Apollo Lake installation.

A successful SSH login from the Chromebook to the Lenovo was verified. The SSH session provides remote terminal access to the Lenovo; computation remains local to the Lenovo and is not offloaded to the Chromebook.

The SSH connection currently depends on the Lenovo's local network connection. Networking will be disabled during actual offline benchmark measurements.


### CPU Frequency Management Inspection

The Alpine cpupower package (version 7.1.5-r0) was installed successfully. The N3350 was reported as using the intel_cpufreq driver, with hardware frequency limits of 800 MHz to 2.40 GHz.

The available CPU frequency governors were ondemand, performance and schedutil. The active governor was schedutil, which dynamically selects a frequency within the available range according to CPU utilisation.

At the time of inspection, the kernel asserted a CPU frequency of approximately 2.29 GHz. Direct hardware frequency reporting was unavailable through the queried interface, so this value should be treated as a kernel-asserted frequency rather than a direct physical clock measurement.

Linux reported boost state support as supported and active. This should not be interpreted as Intel Turbo Boost; the N3350's Intel specifications do not list Intel Turbo Boost Technology.

### CPU Utilisation Monitoring

The Alpine sysstat package (version 12.7.8-r0) was installed successfully and mpstat was verified.

An initial five-second observation using mpstat 1 5 showed an average CPU utilisation of 0.60% non-idle time and 99.40% idle time. CPU steal time remained at 0.00%, consistent with the Lenovo running the Alpine environment directly on physical hardware rather than as a guest virtual machine.

This observation represents an idle-state monitoring check rather than a performance benchmark.

### CPU Thermal Monitoring

The Alpine environment exposed five kernel thermal zones. Their reported types were INT3400 Thermal, SEN1, acpitz, iwlwifi_1 and TCPU.

thermal_zone4 was identified as TCPU and selected as the primary CPU thermal measurement source for subsequent experiments.

The initial TCPU reading was 35,000 millidegrees Celsius (35°C). The first reported thermal trip point was 105,050 millidegrees Celsius (105.05°C).

The thermal trip point is treated as a safety threshold rather than a target operating temperature. Subsequent CPU stress experiments will monitor temperature continuously and will not intentionally target the thermal limit.

### Integrated GPU Device Exposure

The Alpine environment was inspected for Linux DRM graphics devices. /sys/class/drm exposed card1, connectors for the internal eDP-1 display and HDMI-A-1, and the render node renderD128. The corresponding /dev/dri directory exposed card1 and renderD128.

The presence of renderD128 confirms that the integrated Intel GPU is exposed through a Linux DRM render node in the Alpine environment. This establishes GPU device availability but does not by itself demonstrate useful compute or AI acceleration.

Further experiments will investigate the active GPU driver, available compute/graphics APIs and practical AI-runtime compatibility before determining whether the Intel HD Graphics 500 is useful for Apollo Lake inference.



### Integrated GPU Driver Identification

The DRM device was inspected to identify the active kernel driver. /sys/class/drm/card1/device/driver resolved to the Linux i915 PCI driver.

The GPU reported PCI vendor ID 0x8086 (Intel) and device ID 0x5a85, corresponding to the Intel HD Graphics 500 integrated GPU used by the N3350 platform.

This confirms that Alpine's kernel has detected the integrated GPU and attached the Intel i915 driver without requiring a separate driver installation. Combined with the previously observed DRM render node (/dev/dri/renderD128), this establishes functional kernel-level GPU device exposure.

Higher-level graphics/compute API support and practical AI acceleration remain unverified and will be investigated separately.

















## Initial Observations

Alpine successfully booted from the external removable media and reached the root shell.

Initial memory state:

- Total memory reported: 3.7 GiB
- Used memory: 124.5 MiB
- Free memory: 3.2 GiB
- Available memory: 3.3 GiB
- Swap: 0

Initial system activity:

- Uptime: approximately 7 minutes
- Load average: 0.03 / 0.03 / 0.00

The Alpine environment is therefore very lightly loaded immediately after boot.

## Tool Availability

The following commands were not available in the initial minimal environment:

- `lscpu`
- `lsblk`

This is expected for a highly minimal Alpine environment and means that additional measurement utilities will need to be installed deliberately before detailed benchmarking.

## Interpretation

The initial Alpine environment has a very low idle memory footprint and negligible CPU load. Compared with the previously recorded Ubuntu baseline, Alpine currently leaves more memory available to applications.

This does not by itself demonstrate higher application or AI performance. CPU frequency behaviour, sustained CPU performance, storage performance, thermal behaviour and software overhead must be measured separately.

## Next Step

Install only the required system monitoring and benchmarking utilities, then establish a controlled Alpine hardware baseline before running AI-specific benchmarks.
