## Date: 15/09/26
## Status: Completed

## Main Question

What useful information can be obtained about the internal eMMC storage without modifying the installed Ubuntu storage layout?


## Method Summary

First, the Lenovo was booted into Alpine.

lsblk was used to distinguish the removable benchmark media from the internal storage.

The internal eMMC was identified as:

mmcblk0

with the existing partition layout.

The eMMC partitions were not mounted in Alpine, and no LVM mapping was active.

Because the benchmark environment's / was tmpfs, care was taken not to benchmark /tmp, which would have measured RAM-backed storage instead.

A direct raw sequential read benchmark was therefore performed against /dev/mmcblk0.

Ethernet was disconnected during the actual measurement.

The command read 256 MiB directly from the internal eMMC and discarded the data:

dd if=/dev/mmcblk0 of=/dev/null bs=4M count=64 iflag=direct

The result was recorded and copied to the Chromebook.

The system was then rebooted into Ubuntu.

Ubuntu confirmed that the internal eMMC was exposed through:

mmcblk0 → LVM → ext4 root filesystem

A short filesystem-level write/read diagnostic was then performed using a 128 MiB temporary file and direct I/O.

Those numbers were substantially higher than the raw-device result and were therefore judged unsuitable for representing physical eMMC throughput.

The temporary file was deleted afterwards.



## Results

Raw sequential read:

161.2 MB/s

Filesystem diagnostic:

apparent write: 844 MB/s
apparent read: 1.7 GB/s

The latter two measurements were deliberately excluded from physical-storage performance conclusions.


## Conclusion

The controlled raw-device read provides the most useful Phase 1 storage-performance measurement: 161.2 MB/s.

The filesystem-level test also demonstrated why short cached/filesystem measurements should not automatically be interpreted as physical eMMC throughput.
