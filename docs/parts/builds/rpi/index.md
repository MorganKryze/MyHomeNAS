# Level 1: Raspberry Pi 5 + 2 SSD (aka "la débrouille")

## Navigation

- [Get practical NAS experience before diving in](../../knowledge-base.md)
- [Go back home](../../../../README.md#lets-explore-the-builds)

## Glossary

- **NAS** : Network Attached Storage. A NAS is a device that is connected to your network and that can store data. It can be used to store backups, media files, documents, etc.
- **SBC** : Single Board Computer. A SBC is a computer that is built on a single circuit board. It is usually used for educational purposes, IoT projects, and small servers.
- **Server** : A server is a computer that is used to provide services to other computers. It can be a NAS, a web server, a database server, etc. In this guide, we will use the term "server" to refer to the NAS.
- **Client PC** : A client PC is a computer that is used to access services provided by a server. In this guide, we will use the term "client PC" to refer to your personal computer used to access and configure the NAS.
- **SSH** : Secure Shell. SSH is a protocol that is used to securely connect to a remote computer. It is used to access the command line interface of the server.
- **SSD** : Solid State Drive. A SSD is a data storage device that uses flash memory to store data. It is faster than a HDD but more expensive.
- **HDD** : Hard Disk Drive. A HDD is a data storage device that uses magnetic storage to store data. It is used to store large amounts of data.

## Hardware

### Build requirements

These components are needed to build a basic Raspberry Pi 5 based NAS with 2 SSDs. You may find more appropriate or affordable components depending on your needs or budget, however try not to go off the rails too much as compatibility issues may arise (keep a raspberry pi 5 at the core of the build, but you may choose how much RAM you want, keep ssd and sd card but you may choose how much storage you want, etc. ).

| Component      | Description                    | Price Estimate | Quantity | Total Estimate | Links                                                                                                                                                                                                  |
| -------------- | ------------------------------ | -------------- | -------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Raspberry Pi 5 | The main computer for the NAS. | 90€            | 1        | 90€            | [kubii](https://www.kubii.com/en/nano-computers/4106-1832-raspberry-pi-5-3272496315938.html#/ram-8_gb), [amazon](https://amzn.eu/d/5LiKxiZ)                                                            |
| SSDs           | Storage for the NAS. (1To)     | 65€            | 2        | 130€           | [kubii](https://www.kubii.com/en/storage-device/4446-2411-ssd-hard-drive-for-raspberry-pi-5-3272496319592.html#/storage_capacity-1tb), [amazon](https://amzn.eu/d/dkpEgvX)                             |
| Pironman 5 max | Case with cooling              | 80€            | 1        | 70€            | [kubii](https://www.kubii.com/en/ventilated-smart-cases/4785-pironman-5-max-3272496323865.html), [sunfounder](https://www.sunfounder.com/products/pironman-5-max), [amazon](https://amzn.eu/d/5glcb0B) |
| Power supply   | 5V 5A USB-C power supply       | 15€            | 1        | 15€            | [kubii](https://www.kubii.com/en/power-supplies/4107-1818-power-supply-raspberry-pi-27w-usb-c-3272496315761.html#/color-white/power_supply-european_union_eu), [amazon](https://amzn.eu/d/5R0RK0o)     |
| micro SD card  | For the OS installation        | 10€            | 1        | 10€            | [kubii](https://www.kubii.com/en/storage-device/4392-2099-official-raspberry-pi-sd-card-3272496319158.html#/storage_capacity-32_gb/os-without_os_activated), [amazon](https://amzn.eu/d/85VKNLo)       |

Base budget (without storage): **~185€**

Complete budget (with 2x1To SSDs): **~315€**

### Extras

These components are not part of the build itself but are useful to have for the setup and operation of the NAS.

- **Router & ethernet cables**: your home router from your ISP can do the job, ethernet connectivity is preferrable than wifi for reliabality and performance.
- **Monitor and keyboard**: only needed for the initial setup if you don't want to do it headless via SSH, or as an alternative to find the IP address of the Raspberry Pi on your network.

## Software

What you will need to install on your desktop to prepare the micro SD card for the Raspberry Pi 5:

- [**Raspberry Pi Imager**](https://www.raspberrypi.com/software): a tool that is used to flash OS images to SD cards given additional parameters (user, wifi, ssh, location, etc.).

## Get started

- [Setting up the raspberry pi](./raspberrypi.md) WIP
- [First boot and OMV installation](./omv-first-boot.md) WIP
- [Storage setup](./omv-storage.md) WIP
- [Docker setup](./omv-docker.md) WIP
- [Deploy services with containers](./containers.md) WIP
