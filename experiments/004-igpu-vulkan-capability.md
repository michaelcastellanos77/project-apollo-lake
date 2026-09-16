Question: Does the HD 500 expose a usable modern GPU API in Alpine?

Result: Yes, Vulkan successfully enumerated it.

Key evidence:

deviceName = Intel(R) HD Graphics 500 (APL 2)
vendorID = 0x8086
deviceID = 0x5a85
deviceType = PHYSICAL_DEVICE_TYPE_INTEGRATED_GPU
driverID = DRIVER_ID_INTEL_OPEN_SOURCE_MESA
driverName = Intel open-source Mesa driver
driverInfo = Mesa 26.1.6
Vulkan Instance Version = 1.4.347


Conclusion:

Kernel-level i915 exposure and higher-level Vulkan userspace support were both demonstrated.
Practical AI acceleration remains unverified.
