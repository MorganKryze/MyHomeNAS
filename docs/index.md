# My Home NAS - MHN

## Table of contents

- [My Home NAS - MHN](#my-home-nas---mhn)
  - [Table of contents](#table-of-contents)
  - [Glossary](#glossary)
  - [Builds](#builds)
    - [Common](#common)
    - [Test: SBC + 0 HDD](#test-sbc--0-hdd)
    - [Minimal: SBC + 2 portable HDD](#minimal-sbc--2-portable-hdd)
    - [Intermediate: SBC + 3 HDD](#intermediate-sbc--3-hdd)
    - [Advanced](#advanced)
  - [Guides](#guides)
    - [Get your SBC ready](#get-your-sbc-ready)
    - [Setup OMV](#setup-omv)
    - [Deploy services](#deploy-services)

## Glossary

- **NAS** : Network Attached Storage. A NAS is a device that is connected to your network and that can store data. It can be used to store backups, media files, documents, etc.
- **SBC** : Single Board Computer. A SBC is a computer that is built on a single circuit board. It is usually used for educational purposes, IoT projects, and small servers.
- **Server** : A server is a computer that is used to provide services to other computers. It can be a NAS, a web server, a database server, etc. In this guide, we will use the term "server" to refer to the NAS.
- **Client PC** : A client PC is a computer that is used to access services provided by a server. In this guide, we will use the term "client PC" to refer to your personal computer used to access and configure the NAS.
- **SSH** : Secure Shell. SSH is a protocol that is used to securely connect to a remote computer. It is used to access the command line interface of the server.
- **HDD** : Hard Disk Drive. A HDD is a data storage device that uses magnetic storage to store data. It is used to store large amounts of data.
- **SSD** : Solid State Drive. A SSD is a data storage device that uses flash memory to store data. It is faster than a HDD but more expensive.

## Builds

### Common

Some elements will be common to all builds, we will list them here in regard of their category.

Hardware:

- **Router & ethernet cables**: You should already have a router to connect your server to the internet.
- **Fans & heatsinks**: Try to keep your SBC cool by adding fans and heatsinks. This will help to prevent overheating and improve the performance of your SBC. These are not added to the builds.
- **SD card**: You will need an SD card to flash the OS image of your choice. The size of the SD card will depend on the OS image you choose but I recommend a minimum of 16GB to 32GB for most OS images. If you intend to keep the SD card as the boot storage, you should consider a high-quality SD card (A2 class) to avoid data corruption.
- **Power supply**: You will need a power supply to power your SBC. Make sure to choose a power supply that is compatible with your SBC.
- **Hard drives**: I would recommend you to take the time to choose the right hard drives for your NAS. Wether you want to Keep It Simple and Stupid (KISS) and go for [classic hdd](https://www.westerndigital.com/products/portable-drives/wd-my-passport-ultra-usb-c-hdd?sku=WDBC3C0010BSL-WESN) or you want to go for a more complex setup, you should consider the following:
  - **RAID**: If you want to use RAID, you will need at least two hard drives of the same size. The most common RAID configurations are RAID 0, RAID 1, and RAID 5[^1].
  - **File system**: You will need to choose a file system for your hard drives. The most common file systems are ext4, btrfs, and zfs.[^2]
  - **Storage capacity**: You will need to choose the storage capacity of your hard drives. The storage capacity will depend on your needs and budget. I would recommend you to choose hard drives with a storage capacity of at least 1TB to 2TB.
- **Monitor, keyboard & mouse**: For certain configurations, you may need a monitor, keyboard, and mouse to connect to your SBC. This is optional as you should be able to access your SBC remotely using SSH or a web interface.

Software:

- [**Raspberry Pi Imager**](https://www.raspberrypi.com/software/): a tool that is used to flash OS images to SD cards given additional parameters **only** for Raspberry Pi SBC.
- [**Balena Etcher**](https://www.raspberrypi.com/software/): a tool that is similarly used to flash OS images to SD cards and USB drives as RPI Imager. However, you won't be able to add additional parameters to the image. It can be used for both arm and amd architectures.[^3]
- **OS image**: The image of the OS you want to install on your SBC. You can find the official images on the official websites of the SBCs. Here are some links to the official websites of the most common SBCs:
  - [Orange Pi OS](https://www.orangepi.org/downloadresources/)
  - [Armbian](https://www.armbian.com/download/)
  - [Amd architectures](https://www.openmediavault.org/download/)
- **Domain name & Zero trust account**: If you intend to access your server remotely and deploy services online, you will need a domain name and a Zero trust account[^4] to create Cloudflare tunnels[^5].

> [!IMPORTANT]
> By default, the builds will be based on the Raspberry Pi 5. However, these are mockups for you to freely adapt to your needs and budget.

### Test: SBC + 0 HDD

> A build without storage is not recommended for a NAS as the storage would be very limited. See this build as a test to see if the server configuration stands.

| Hardware     | Price (€) | Quantity | Total (€) | Reference                                                                                                                                                                                                                           |
| ------------ | --------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SBC          | 100       | 1        | 100       | [Raspberry Pi 5 8GB](https://www.kubii.com/fr/cartes-nano-ordinateurs/4106-1832-raspberry-pi-5-3272496315938.html#/ram-8_gb)                                                                                                        |
| SD card      | 15        | 1        | 15        | [Raspberry Pi 64GB SD Card](https://www.kubii.com/fr/support-de-stockage/4392-2100-carte-sd-officielle-raspberry-pi-3272496319158.html?mot_tcid=b4438e27-887f-47e1-89be-338c634ac0ea#/capacite_de_stockage-64_gb/os-sans_os_active) |
| Power supply | 15        | 1        | 15        | [Geekworm Raspberry Pi 5 Power Supply](https://amzn.eu/d/esncmz2)                                                                                                                                                                   |

**Overall budget**: ~130€

### Minimal: SBC + 2 portable HDD

> A minimal build with 2 portable HDDs. This build is recommended for a small NAS with limited storage capacity but exploring a wide portion of the features of a NAS.

| Hardware     | Price (€) | Quantity | Total (€) | Reference                                                                                                                                                                                                                           |
| ------------ | --------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SBC          | 100       | 1        | 100       | [Raspberry Pi 5 8GB](https://www.kubii.com/fr/cartes-nano-ordinateurs/4106-1832-raspberry-pi-5-3272496315938.html#/ram-8_gb)                                                                                                        |
| SD card      | 15        | 1        | 15        | [Raspberry Pi 64GB SD Card](https://www.kubii.com/fr/support-de-stockage/4392-2100-carte-sd-officielle-raspberry-pi-3272496319158.html?mot_tcid=b4438e27-887f-47e1-89be-338c634ac0ea#/capacite_de_stockage-64_gb/os-sans_os_active) |
| Power supply | 15        | 1        | 15        | [Geekworm Raspberry Pi 5 Power Supply](https://amzn.eu/d/esncmz2)                                                                                                                                                                   |
| HDD          | 80        | 2        | 160       | [WD portable HDD 2TB](https://amzn.eu/d/bWB4Jla)                                                                                                                                                                                    |

**Overall budget**: ~290€

### Intermediate: SBC + 3 HDD

> This build is starting to look like a real NAS. With 3 HDDs, you can start to explore RAID configurations and more complex file systems to preserve data integrity and redundancy.

| Hardware             | Price (€) | Quantity | Total (€) | Reference                                                                                                                                                                                                                           |
| -------------------- | --------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SBC                  | 100       | 1        | 100       | [Raspberry Pi 5 8GB](https://www.kubii.com/fr/cartes-nano-ordinateurs/4106-1832-raspberry-pi-5-3272496315938.html#/ram-8_gb)                                                                                                        |
| SD card              | 15        | 1        | 15        | [Raspberry Pi 64GB SD Card](https://www.kubii.com/fr/support-de-stockage/4392-2100-carte-sd-officielle-raspberry-pi-3272496319158.html?mot_tcid=b4438e27-887f-47e1-89be-338c634ac0ea#/capacite_de_stockage-64_gb/os-sans_os_active) |
| Power supply         | 15        | 1        | 15        | [Geekworm Raspberry Pi 5 Power Supply](https://amzn.eu/d/esncmz2)                                                                                                                                                                   |
| POE and NVMe RPi Hat | 40        | 1        | 40        | [GeekPi P33](https://amzn.eu/d/5xsOfVs)                                                                                                                                                                                             |
| NVMe to SATA adapter | 25        | 1        | 25        | [Namvo M2 to SATA](https://amzn.eu/d/fAt3BhD)                                                                                                                                                                                       |
| HDD                  | 150       | 3        | 450       | [Seagate BarraCuda 8TB](https://www.amazon.fr/-/en/dp/B075WYBQXJ/)                                                                                                                                                                  |

**Overall budget**: ~650€

### Advanced

> If the previous builds were not enough for you, you might consider going for a real board like an ITX motherboard with a dedicated CPU and RAM. This build will be elaborated in the future.

## Guides

### Get your SBC ready

- **Orange Pi 5 Plus** -> [here](./parts/orangepi.md)
- **Raspberry Pi 5** -> [here](./parts/raspberrypi.md)

### Setup OMV

- **First boot** -> [here](./parts/omv-first-boot.md)
- **Storage** -> [here](./parts/omv-storage.md)
- **Docker** -> [here](./parts/omv-docker.md)

### Deploy services

- **Containers** -> [here](./parts/containers.md) WIP

[^1]: [Hardware Haven](https://www.youtube.com/@HardwareHaven) did a great video on the subject, you can find it [here](https://www.youtube.com/watch?v=ykhaXo6m-04).
[^2]: To learn more about file systems explore this thread on [reddit](https://www.reddit.com/r/linuxquestions/comments/kptm30/zfs_vs_btrfs_vs_ext4_for_a_consumer/).
[^3]: To learn more about amd and arm processors [here](https://medium.com/@aebbatlfgt/breaking-down-the-differences-between-amd-and-arm-processors-cd5dbf5f8c21).
[^4]: To learn more about Zero trust [here](https://www.cloudflare.com/learning/security/what-is-zero-trust/).
[^5]: I would recommend this [tutorial](https://www.youtube.com/watch?v=ey4u7OUAF3c) from [NetworkChuck](https://www.youtube.com/channel/UC9x0AN7BWHpCDHSm9NiJFJQ) to learn how to set up Cloudflare tunnels
