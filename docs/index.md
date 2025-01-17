# My Home NAS - MHN

## Table of contents

- [My Home NAS - MHN](#my-home-nas---mhn)
  - [Table of contents](#table-of-contents)
    - [Glossary](#glossary)
    - [Builds](#builds)
  - [Sources](#sources)

### Glossary

- **NAS** : Network Attached Storage. A NAS is a device that is connected to your network and that can store data. It can be used to store backups, media files, documents, etc.
- **SBC** : Single Board Computer. A SBC is a computer that is built on a single circuit board. It is usually used for educational purposes, IoT projects, and small servers.
- **Server** : A server is a computer that is used to provide services to other computers. It can be a NAS, a web server, a database server, etc. In this guide, we will use the term "server" to refer to the NAS.
- **Client PC** : A client PC is a computer that is used to access services provided by a server. In this guide, we will use the term "client PC" to refer to your personal computer used to access and configure the NAS.
- **SSH** : Secure Shell. SSH is a protocol that is used to securely connect to a remote computer. It is used to access the command line interface of the server.
- **HDD** : Hard Disk Drive. A HDD is a data storage device that uses magnetic storage to store data. It is used to store large amounts of data.
- **SSD** : Solid State Drive. A SSD is a data storage device that uses flash memory to store data. It is faster than a HDD but more expensive.

### Builds

The Hardware requirements are the author's recommendations. You may choose to use different hardware based on your needs and your budget.

| Hardware                  | Price (€) | Quantity | Total (€) | Reference                                                                                                                                                                                                                  |
| ------------------------- | --------- | -------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SBC                       | 150       | 1        | 150       | [Orange Pi 5 Plus](https://fr.aliexpress.com/item/1005005585029938.html)                                                                                                                                                   |
| SD card                   | 15        | 1        | 15        | [Raspberry Pi 64GB SD Card](https://www.kubii.com/fr/support-de-stockage/4392-2102-carte-sd-officielle-raspberry-pi-3272496319158.html?mot_tcid=b4438e27-887f-47e1-89be-338c634ac0ea#/capacite_de_stockage-64_gb/os-noobs) |
| Power supply              | 15        | 1        | 15        | [Geekworm Raspberry Pi 5 Power Supply](https://www.amazon.fr/-/en/dp/B0CZCZF2QV?nsdOptOutParam=true)                                                                                                                       |
| HDD                       | 150       | 6        | 900       | [Seagate BarraCuda 8TB](https://www.amazon.fr/-/en/dp/B075WYBQXJ/)                                                                                                                                                         |
| NVMe to SATA adapter      | 25        | 1        | 25        | [Namvo M2 to SATA](https://www.amazon.fr/-/en/dp/B0BWHDPZ7D/?psc=1)                                                                                                                                                        |
| Router                    | -         | -        | -         | (you should already have one)                                                                                                                                                                                              |
| Ethernet cables           | -         | -        | -         | (Optional)                                                                                                                                                                                                                 |
| Switch                    | -         | -        | -         | (Optional)                                                                                                                                                                                                                 |
| Monitor, keyboard & mouse | -         | -        | -         | (Optional)                                                                                                                                                                                                                 |

| Software            | Chipset | Comment                         | Reference                                                                                                                                                       |
| ------------------- | ------- | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Balena Etcher       | ARM/x86 |                                 | [Balena Etcher](https://etcher.balena.io/)                                                                                                                      |
| Raspberry Pi Imager | ARM     | Only for Raspberry Pi SBC       | [Raspberry Pi Imager](https://www.raspberrypi.com/software/)                                                                                                    |
| OS image            | ARM     | For all other than Raspberry Pi | [Orange Pi OS](https://www.orangepi.org/downloadresources/) , [Armbian](https://www.armbian.com/download/), [x86 ISO](https://www.openmediavault.org/download/) |

> [!NOTE]
> You will need a `domain name` and a `Zero trust account` to use Cloudflare tunnels. Here is a [tutorial](https://www.youtube.com/watch?v=ey4u7OUAF3c) to set it up.

Budgets may vary depending on the hardware you choose, and HDD . Here are some configs:

- **Minimal** (~200€): 1 SBC, 1 SD card, 1 power supply and 1 ethernet cable. That configuration may suffice for testing purposes.
- **Standard** (~600€): 1 SBC, 1 SD card, 1 power supply, 2 HDD, 1 NVMe to SATA adapter and 1 ethernet cable. This configuration is recommended for a functional NAS.
- **Optimal** (~1200€): 1 SBC, 1 SD card, 1 power supply, 6 HDD, 1 NVMe to SATA adapter, 1 ethernet cables. This configuration is the complete setup for this guide.

## Sources

Here is the most exhaustive I could make of the sources I used to write this guide:

- [Orange Pi 5 Plus Specs](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-5-plus.html) from its [OrangePi](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-5-plus.html) documentation.
- [Orange Pi - Setting Up a Secure Web Server on a Single Board Computer](https://ambientnode.uk/orange-pi-webhost/) By [Zsolt Bizderi](https://ambientnode.uk/author/zsolt/)
- [Installation on Debian for OpenMediaVault](https://docs.openmediavault.org/en/stable/installation/on_debian.html) from the [OpenMediaVault](https://docs.openmediavault.org/en/stable/index.html) documentation
- [Wiki for the installation on Raspberry Pi and affiliates](https://wiki.omv-extras.org/doku.php?id=omv7:raspberry_pi_install) from the [omv-extras](https://wiki.omv-extras.org/doku.php?id=start) documentation
- [Wiki for the walkthrough of the setup on the WebGUI](https://wiki.omv-extras.org/doku.php?id=omv7:new_user_guide#web_console_login) from the [omv-extras](https://wiki.omv-extras.org/doku.php?id=start) documentation
- [Choosing The BEST Drive Layout For Your NAS](https://www.youtube.com/watch?v=ykhaXo6m-04) by [Hardware Haven](https://www.youtube.com/@HardwareHaven)
- [Mistral AI](https://chat.mistral.ai/) for the redaction and structure of the guide.

---

Last update: Jan. 2025
Created: Oct. 2024
