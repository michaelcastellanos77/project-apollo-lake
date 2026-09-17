## Main Question: 

Is the Intel HD Graphics 500 merely visible to the kernel, or does Alpine expose a usable higher-level GPU API?

## Method Summary

The /sys/class/drm and /dev/dri interfaces were inspected first.

This established the presence of:

card1
renderD128
display connectors

The kernel driver was then identified through the DRM device path and was confirmed as Intel i915.

PCI vendor/device information identified the GPU as:

vendorID = 0x8086
deviceID = 0x5a85

The minimal Alpine environment initially lacked Mesa/Vulkan/OpenCL userspace packages.

The Alpine package repositories were queried to determine what GPU software was available before installing anything.

The available packages included:

mesa-vulkan-intel
vulkan-loader
vulkan-tools
mesa-rusticl
opencl
llama.cpp-vulkan

Rather than installing the entire graphics stack or running a heavy benchmark, the smallest useful Vulkan stack was installed:

mesa-vulkan-intel
vulkan-loader
vulkan-tools

vulkaninfo --summary was then used to enumerate physical Vulkan devices.

The system produced warnings regarding DISPLAY and XDG_RUNTIME_DIR, which were expected because the test was performed in a headless command-line environment.

The actual Vulkan device enumeration succeeded.

The complete summary was saved to a text file and transferred to the Chromebook.

## Results

Vulkan successfully detected:

Intel(R) HD Graphics 500 (APL 2)

using the Intel open-source Mesa driver.



Key evidence:

deviceName = Intel(R) HD Graphics 500 (APL 2)
vendorID = 0x8086
deviceID = 0x5a85
deviceType = PHYSICAL_DEVICE_TYPE_INTEGRATED_GPU
driverID = DRIVER_ID_INTEL_OPEN_SOURCE_MESA
driverName = Intel open-source Mesa driver
driverInfo = Mesa 26.1.6
Vulkan Instance Version = 1.4.347


## Conclusion:

The HD Graphics 500 is not only kernel-visible but also accessible through a functioning Vulkan userspace stack in Alpine.

Practical usefulness for LLM inference remains unmeasured and is therefore not assumed.

Kernel-level i915 exposure and higher-level Vulkan userspace support were both demonstrated.
Practical AI acceleration remains unverified.
