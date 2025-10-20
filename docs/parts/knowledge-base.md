# Learning path

## Storage

### RAID Arrays

Source video: https://www.youtube.com/watch?v=UuUgfCvt9-Q

Source article: https://www.geeksforgeeks.org/dbms/raid-redundant-arrays-of-independent-disks/

:::info
RAID: Redundant Array of Independent Disks
:::

#### RAID 0

Minimum disks: 2

Speed: Very Fast

Reliability: Low

Highest disk failure tolerance: 0 disk

Description: Data is spread across all disk without redundancy. All the array is lost.

Effective space: All disk space is allocated to storing files.

![](https://docs.libresoftware.cloud/api/files/0199589f-de5e-7139-913d-1d21f25e5136/Screenshot_2025-09-17_at_19.01.28.png)

#### RAID 1

Minimum disks: 2

Speed: Slow

Reliability: Normal

Highest disk failure tolerance: 1 disk

Description: Data is spread evenly between two disks. If one fails, you may integrate a new one in the pool to replace it.

Effective space: Half of the disk space is used to store files, the other is used to replicate the files.

![](files/019958a0-ff8d-719e-868a-289bb9028bb0/Screenshot_2025-09-17_at_19.02.45.png)

#### RAID 5

Minimum disks: 3

Speed: Fast

Reliability: Normal

Highest disk failure tolerance: 1 disk

Description: Parity data is stored across the disks (name Px on the picture) to enable a reconstruction of the file system in case of a failure. If On disk fails, it can be replaced and reconstructed. More than one disk, or an additional disk failure during the reconstruction would mean the break of the entire pool.

Effective space: You should consider losing the equivalent of the space of one disk.

![](files/019958a1-bd19-756f-a293-03d922108e9b/Screenshot_2025-09-17_at_19.03.32.png)

#### RAID 6

Minimum disks: 4

Speed: Normal

Reliability: High

Highest disk failure tolerance: 2 disks

Description: Parity data is now stored twice allowing up to two disk failure on the pool.

Effective space: You should consider losing the equivalent of the space of two disks.

![](files/019958a2-2208-77ab-a34b-dd3cc42b5ea7/Screenshot_2025-09-17_at_19.03.59.png)

### ZFS

Source article: https://en.wikipedia.org/wiki/ZFS

:::info
ZFS: Zettabyte File System
:::

| Conventional File System | ZFS      |
| ------------------------ | -------- |
| RAID 0                   | Striped  |
| RAID 1                   | Mirrored |
| RAID 5                   | RAIDZ-1  |
| RAID 6                   | RAIDZ-2  |
| RAID 7                   | RAIDZ-3  |

#### Read speed

Source video: https://www.youtube.com/watch?v=3T5wBZOm4hY&t=431s

:::warning
ARC: Adaptive Replacement cache

Stores in RAM most accessed files to speed up read speed. Advised to get 1GB of RAM for 1TB of storage roughly, it highly depends on the use of the NAS (archiving files would require less resources than video editing or virtualization).
:::

:::success
L2ARC: Level 2 ARC

Stores on a faster storage disk random and large files accessed regularly to improve read speed compared to HDD. Generally a large SSD. If the amount of RAM is already high you would not notice a large difference.
:::

#### Write speed

Source article: https://jrs-s.net/2019/05/02/zfs-sync-async-zil-slog/

:::warning
ZIL: ZFS Intent Log

A section inside the storage pool. It stores the files that will later be written on the storage pool. In total, the files will be written twice on HDD drives, meaning slow write speed.
:::

:::success
SLOG: Separate LOG

A vdev storing ZIL, usually mirrored SSDs. This greatly improves speed by writing first to a SSD drive, before committing to the slow storage pool. Usually, nothing is really read from the SLOG, or ZIL for the matter, their intent is a backup in case of a crash and the system could not write the files at the right place in time.
:::

Note that SLOG devices rarely have more than 4GB in use at any given time, so the smaller sized devices are generally the best choice in terms of cost, with larger sizes giving no benefit. Larger sizes could be a good choice for other vdev types, depending on performance needs and cost considerations.

The intents (file writes) are sent to the ZIL or SLOG on a Sync mode, then written to the storage pool. And then verified. On an Async mode, the process is directly sent and not verified which is faster but less safe. Synchronous file write is rarely used, mainly for virtualization, SMB share and similar will by default use asynchronous file write and bypass any ZIL or SLOG use. So choose carefully depending on your projected usage.

### CMR vs SMR drives

Source article: https://buffaloamericas.com/resources/cmr-vs-smr-hard-drives-in-network-attached-storage-nas-msp

:::warning
SMR: Shingled Magnetic Recording

SMR is a relatively new technology that utilizes the fact that write tracks are wider than read tracks on a drive. Data is written sequentially onto a track, and then the track is partially overlapped over another track of data, creating a pattern similar to the shingles on a house roof. SMR removes the aforementioned gaps between tracks, allowing more data tracks to be written onto a drive’s magnetic surface and thereby increasing its storage capacity.
:::

:::success
CMR: Conventional Magnetic Recording

When data is written onto a CMR drive, it is written onto magnetic tracks on the drive surface that are laid side-by-side, with small gaps being placed between the tracks so that they do not overlap. These separator gaps affect the overall areal density of the drive, as they mean portions of the drive’s surface are not being utilized.

With a CMR drive, data can be freely rewritten over an existing track as it has no effect on neighboring tracks, allowing them to handle more random write operations.
:::

While SMR drives increase capacity for lower cost (because the drives can use fewer platters than a CMR drive at the same capacity), the way they work also comes with a speed penalty. When you copy data to an SMR drive, the drive temporarily stores the data in a special cache area and uses idle time later to organize it into shingled regions on the platter. Long, sustained writes suffer speed penalties because if the cache fills up, each time an SMR drive overwrites part of a previous track, it must read and re-write the "partially covered" underlying data as well. So SMR drives can [perform dramatically slower](https://www.servethehome.com/wd-red-smr-vs-cmr-tested-avoid-red-smr/2/) than CMR drives.

![](files/019975ce-c89e-7778-9977-b831ee618e35/Screenshot_2025-09-23_at_11-01-34_CMR_vs_SMR_Hard_Drives_in_Network_Attached_Storage_%28NAS%29_Buffalo_Americas.png)

### Disk cooling

Source article: https://www.truenas.com/docs/core/13.0/gettingstarted/corehardwareguide/#storage-device-cooling

Some may find unuseful to add a proper cooling mechanism for storage cooling. But it is recommended to keep disk at a 26-30°C with ventilation for better efficiency and reliability. Expect to double the error rate each 12°C reached. In general, try to keep drive temperatures below the drive specification provided by the vendor.

## Motherboard

### ECC RAM

Source article: https://www.truenas.com/docs/core/13.0/gettingstarted/corehardwareguide/#error-correcting-code-memory

:::info
ECC: Error Correcting Code Memory

Error-correcting code or ECC RAM detects and corrects in-memory bit errors as they occur. If errors are severe enough to be uncorrectable, ECC memory causes the system to hang (become unresponsive) rather than continue with errored bits. For ZFS and TrueNAS, this behavior virtually eliminates any chances that RAM errors pass to the drives to cause corruption of the ZFS pools or file errors.
:::

Most users _strongly_ recommend ECC RAM as another data integrity defense.

However:

- Some CPUs or motherboards support ECC RAM but not all
- Many TrueNAS systems operate every day without ECC RAM
- RAM of any type or grade can fail and cause data loss
- RAM failures usually occur in the [first three months](https://media.kingston.com/images/usb/pdf/Server_Burn-in.pdf), so test all RAM before deployment.

### Memory sizing

Source article: https://www.truenas.com/docs/core/13.0/gettingstarted/corehardwareguide/#memory-sizing

RAM rarely goes unused on a TrueNAS system, and enough RAM is vital to maintaining peak performance. You should have 8 GB of RAM for basic TrueNAS operations with up to eight drives. Other use cases each have distinct RAM requirements:

- Add 1 GB for each drive added after eight to benefit most use cases.
- Add extra RAM (in general) if more clients connect to the TrueNAS system. A 20 TB pool backing many high-performance VMs over iSCSI might need more RAM than a 200 TB pool storing archival data. If using iSCSI to back up VMs, plan to use at least 16 GB of RAM for good performance and 32 GB or more for optimal performance.
- Add more RAM for plugins and jails, as each has specific application RAM requirements.
- Add more RAM for virtual machines with a guest operating system and application RAM requirements.
- Add the suggested 5 GB per TB of storage for deduplication that depends on an in-RAM deduplication table.
- Add approximately 1 GB of RAM (conservative estimate) for every 50 GB of L2ARC in your pool. Attaching an L2ARC drive to a pool uses some RAM, too. ZFS needs metadata in ARC to know what data is in L2ARC.

### CPU

Choosing ECC RAM limits your CPU and motherboard options, but that can be beneficial. Intel<sup>®</sup> limits ECC RAM support to their lowest and highest-end CPUs, cutting out the mid-range i5 and i7 models.

Which CPU to choose can come down to a short list of factors:

- An underpowered CPU can create a performance bottleneck because of how Open ZFS compresses and encrypts (optional) data and performs checksums.
- A higher-frequency CPU with fewer cores usually performs best for SMB-only workloads because of Samba, the lightly-threaded TrueNAS SMB daemon.
- A higher-core-count CPU is better suited for parallel encryption and virtualization.
- A CPU with AES-NI encryption acceleration support improves the speed of the file system and network encryption.
- A server-class CPU is recommended for power and ECC memory support.
- A Xeon E5 CPU (or similar) is recommended for software-encrypted pools.
- An Intel Ivy Bridge CPU or later is recommended for virtual machine use.

We will focus on AMD Ryzen series. Why? Because.

Here is how to understand the nomenclature of their CPU:

![](files/0199a0e9-0bff-709d-9857-001875e87352/image.png)

### Remote Management: IPMI

As a courtesy to further limit the motherboard choices, consider the Intelligent Platform Management Interface or IPMI (a.k.a. baseboard management controller, BMC, iLo, iDrac, and other names depending on the vendor) if you need:

- Remote power control and monitoring of remote systems
- Remote console shell access for configuration or data recovery
- Remote virtual media for TrueNAS installation or reinstallation

### Power Supply Units

The top criteria to consider for a power supply unit (or PSU) on a TrueNAS system are:

- Power capacity (in watts) for the motherboard and the number of drives it must support
- Reliability
- Efficiency rating
- Relative noise
- Optional redundancy to keep critical systems running if one power supply fails

:::info
UPSU: Uninterrupted Power Supply Units

PSU with an additional battery to keep the NAS in uptime to mitigate the power outage issue. Usually overkill for most home builds.
:::

### PCIe expansion slots and cards

Source article: https://www.crystalrugged.com/knowledge/what-is-pcie-slots-cards-lanes/

:::info
PCIe: Peripheral Component Interconnect Express

A high-speed interface standard used for connecting various internal components in a computer system. PCIe is primarily used for connecting expansion cards (graphics cards, network cards, storage controllers) to the motherboard.
:::

#### Lanes size

The standard includes 4 different lanes layouts:

- x1 for less demanding expansion cards, such as sound cards, network cards, and Wi-Fi adapters.
- x4 for various expansion cards, including storage controllers, RAID cards, and some sound cards.
- x8 for high-performance expansion cards that require greater data transfer rates, such as some network adapters and specialized data acquisition cards.
- x16 are associated with graphics cards (GPUs) and provide the highest bandwidth available on a standard consumer motherboard. High-end gaming, content creation, and workstation systems often feature PCIe x16 slots for powerful graphics processing.

![](files/0199be95-ce11-76bd-81a4-e2e07ac2ce9f/image.png)

:::warning
You can use PCIe cards of a lower lane width (e.g., x2, x4, or x8) in a higher lane width slot (e.g., x16).

PCI-Express will negotiate how many lanes will be used, and your card will work at its full speed, assuming the PCIe slot has the same or higher PCIe version.
:::

#### PCIe version

- PCIe 1.0 (2003): outdated
- PCIe 2.0 (2007): outdated
- PCIe 3.0 (2010): standard for various expansion cards
- PCIe 4.0 (2017): standard for graphics cards
- PCIe 5.0 (2019)
- PCIe 6.0 (2022)
