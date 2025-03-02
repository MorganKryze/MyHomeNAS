# OpenMediaVault Installation

> This guide will help you to set up a NAS using OpenMediaVault on a Raspberry Pi.

- **Want to go back to the index page?** [click here](../index.md).

## Table of Contents

- [OpenMediaVault Installation](#openmediavault-installation)
  - [Table of Contents](#table-of-contents)
  - [I - Installation on the Pi](#i---installation-on-the-pi)
    - [Install OpenMediaVault on the Pi](#install-openmediavault-on-the-pi)
    - [First login](#first-login)
  - [II - Networking](#ii---networking)
    - [Firewall configuration](#firewall-configuration)
      - [Allow your client PC](#allow-your-client-pc)
      - [Block access to other devices](#block-access-to-other-devices)
      - [Allow DNS Traffic](#allow-dns-traffic)
      - [Allow ICMP Traffic \& Loopback](#allow-icmp-traffic--loopback)
      - [Allow Cloudflare traffic](#allow-cloudflare-traffic)
      - [Default rules](#default-rules)
      - [IPv6 rules](#ipv6-rules)
    - [Installing Fail2Ban](#installing-fail2ban)
  - [Installing ClamAV](#installing-clamav)
  - [III - Finishing process](#iii---finishing-process)
    - [Create a backup](#create-a-backup)
  - [Next step](#next-step)
  - [Sources](#sources)

## I - Installation on the Pi

In this section, we will install OpenMediaVault on the Pi to have a nice web interface to manage the NAS.

### Install OpenMediaVault on the Pi

> OpenMediaVault is a free network-attached storage server based on the Debian operating system. It contains services like SSH, (S)FTP, SMB, NFS, Rsync, and more.

- First, we will run the pre-installation script, and fill the password as requested:

```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/preinstall | sudo bash && sudo reboot
```

- After the reboot, we run the installation script and provide the password, this may take a while depending on your internet connection:

```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```

- The installation script will automatically reboot. This time, do not connect to the Pi, but go to your browser and type the IP address of the Pi. (ex: `http://192.168.1.x` or [`http://myhomenas.local`](http://myhomenas.local))

> [!WARNING]
> The default page is not secure (e. g. no HTTPS). Your browser may block the connection. You can bypass this by clicking on `Advanced` and then `Proceed to the IP address` or find another browser that does not block the connection.

- The default username is `admin` and the default password is `openmediavault`.

### First login

> We will scroll down from the `Dashboard` to the `Diagnostics` menu and stop by every relevant section to check the status of the services.

- Once logged in, I strongly recommend you to change the default password by clicking to your profile and then `Change Password` (choose a strong password, at least 30 characters and store it in your password manager).

![Password](../assets/img/omv/password.png)

- You will be prompted to change the widgets on your dashboard, here is my selection:

![Widgets](../assets/img/omv/widgets.png)

- As we saw earlier, the default protocol is HTTP. We will change it to HTTPS to secure the connection by creating self-signed certificates in the `Workbench` section. I woumd also recommend to enforce the use of HTTPS by checking the `Force SSL/TLS` box.

![Certificates](../assets/img/omv/certificates.png)

- Now in `Date & Time`, you may want to change the timezone to your location:

![Timezone](../assets/img/omv/timezone.png)

- It could also prove useful to configure an email client that would notify you on update or issues with the NAS. You can find the settings in the `Notifications` > `Settings` (client) & `Events` (selection) sections.

![Email](../assets/img/omv/email.png)

- Under the `Power Management` > `Scheduled Tasks` section, you can configure the NAS to reboot everyday at 3am for example.

![Reboot](../assets/img/omv/reboot.png)

- In the `Monitoring` section, enable the setting. This will allow you to monitor the CPU, memory, and network usage of the Pi later on.

![Monitoring](../assets/img/omv/monitoring.png)

- In the `Update Management` section, be sure to stay up-to-date with the latest updates. I would recomment checking the `Community-maintained updates` box.

![Updates](../assets/img/omv/updates.png)

![Settings](../assets/img/omv/settings.png)

- We will explore the `Plugins` section later.

- In the `Network` > `General` ensure that the `Hostname` is set to `nas` or `myhomenas`.

![Network](../assets/img/omv/network.png)

- In the `Network` > `Interfaces` section, select the interface that is connected by ethernet and fill the `DNS servers` with `9.9.9.9,149.112.112.112` and the `Search domain` with `localdomain`.

![Interfaces](../assets/img/omv/interfaces.png)

- Finally, go to `Services` > `SSH` and update the port to `42`, then disable `root login` and `password authentication`.

![SSH](../assets/img/omv/ssh.png)

## II - Networking

### Firewall configuration

> The firewall is a security feature that blocks unwanted traffic from entering your network. We will configure the firewall to allow only the necessary traffic.

- Go to the `Network` > `Firewall` section. Note that there are two panels: `IPv4` and `IPv6`. The order matters, so we will start with the `IPv4` panel.

> [!NOTE]
> You may apply the pending changes at the end of each section.

#### Allow your client PC

- We will allow the IP address of your **Client PC** to connect to the Pi to manage it considering that your IP is `192.168.1.51`.

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `192.168.1.51`
> - Protocol: `All`
> - Tags: `Admin`

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Destination: `192.168.1.51`
> - Protocol: `All`
> - Tags: `Admin`

- Here we allow the Pi to connect to itself assuming the IP is `192.168.1.101`.

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `192.168.1.101`
> - Protocol: `All`
> - Tags: `Self`

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Destination: `192.168.1.101`
> - Protocol: `All`
> - Tags: `Self`

#### Block access to other devices

- Consider setting the Pi to block access to other devices on the network.

> - Direction: `INPUT`
> - Action: `REJECT`
> - Source: `192.168.0.0/16`
> - Protocol: `All`
> - Tags: `Isolement`

> - Direction: `OUTPUT`
> - Action: `REJECT`
> - Destination: `192.168.0.0/16`
> - Protocol: `All`
> - Tags: `Isolement`

#### Allow DNS Traffic

- We will allow DNS traffic to resolve domain names.

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Destination: `9.9.9.9`
> - Destination Port: `53`
> - Protocol: `UDP`
> - Tags: `DNS`

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Destination: `149.112.112.112`
> - Destination Port: `53`
> - Protocol: `UDP`
> - Tags: `DNS`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `9.9.9.9`
> - Source Port: `53`
> - Protocol: `UDP`
> - Tags: `DNS Response`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `149.112.112.112`
> - Source Port: `53`
> - Protocol: `UDP`
> - Tags: `DNS Response`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `127.0.0.11`
> - Protocol: `UDP`
> - Destination Port: `53`
> - Tags: `Docker DNS`

#### Allow ICMP Traffic & Loopback

- We will allow ICMP traffic to ping the Pi.

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Protocol: `ICMP`
> - Tags: `Ping`

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Protocol: `ICMP`
> - Tags: `Ping`

- Allow loopback traffic:

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `127.0.0.0/8`
> - Protocol: `All`
> - Tags: `Loopback`

#### Allow Cloudflare traffic

- We allow Cloudflare's IP addresses to target the Pi only to the port `7844`.

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `103.21.244.0/22`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `103.22.200.0/22`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `103.31.4.0/22`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `104.16.0.0/13`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `104.24.0.0/14`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `108.162.192.0/18`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `131.0.72.0/22`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `141.101.64.0/18`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `162.158.0.0/15`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `172.64.0.0/13`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `173.245.48.0/20`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `188.114.96.0/20`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `190.93.240.0/20`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `197.234.240.0/22`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `198.41.128.0/17`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

#### Default rules

- We allow HTTP and HTTPS traffic:

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `0.0.0.0/0`
> - Destination Port: `80`
> - Protocol: `TCP`
> - Tags: `HTTP`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `0.0.0.0/0`
> - Destination Port: `443`
> - Protocol: `TCP`
> - Tags: `HTTPS`

- These default rules will block all incoming traffic and allow all outgoing traffic on the IPv4 channel by default.

> - Direction: `INPUT`
> - Action: `REJECT`
> - Protocol: `All`
> - Tags: `Default deny`

> - Direction: `OUTPUT`
> - Action: `ACCEPT`
> - Protocol: `All`
> - Tags: `Default allow`

#### IPv6 rules

> We will now add similar rules for the IPv6 channel.

- Allow Cloudflare traffic:

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2400:cb00::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2606:4700::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2803:f800::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2405:b500::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2405:8100::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2a06:98c0::/29`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

> - Direction: `INPUT`
> - Action: `ACCEPT`
> - Source: `2c0f:f248::/32`
> - Destination Port: `7844`
> - Protocol: `TCP`
> - Tags: `Cloudflare`

- Finally, block unwanted traffic:

> - Direction: `INPUT`
> - Action: `REJECT`
> - Protocol: `All`
> - Tags: `Default deny`

> - Direction: `OUTPUT`
> - Action: `Accept`
> - Protocol: `All`
> - Tags: `Default allow`

### Installing Fail2Ban

> Fail2Ban is a service that scans log files and bans IPs that show malicious signs. It is useful to protect your server from brute-force attacks.

- We install the program from the `Plugins` section.

![Fail2Ban](../assets/img/omv/fail2ban-install.png)

- Once installed, we go to the `Services` > `Fail2Ban` section and enable the service. Additionally, you may add the IP address of your **Client PC** to the `Ignore IP` list and an email address to receive notifications if you configured the email client earlier. Leave blank if to disable these settings.

![Fail2Ban](../assets/img/omv/fail2ban-enable.png)

## Installing ClamAV

> ClamAV is an open-source antivirus engine for detecting trojans, viruses, malware, and other malicious threats. We will install it to scan files uploaded to the NAS.

- We start by setting up a shared folder for the `quarantine` section in the `Storage` > `Shared Folders` section. I recommend choosing the default file system (not the on containing your data) and setting the `Permissions` to `read/write` for the Administrator only.
- We install the program from the `Plugins` section: `openmediavault-clamav`.
- Once installed, we go to the `Services` > `Antivirus` > `Settings` sectio, then:
  - Enable the service.
  - Set the `Quarantine directory` to the shared folder we created earlier.
  - Toggle `Detect broken executables` and `Detect broken media files` to `yes`.
  - Let the other settings to their default values.
- Finally, create scans for each of your shared folders using these settings:

![ClamAV](../assets/img/omv/clamav.png)

## III - Finishing process

### Create a backup

> Before proceeding to the next steps, we will now create a backup. This will help you to restore your system in case of a failure up to this point.

- Back to your Pi, create your backup location:

```bash
sudo mkdir /backup
cd /backup
```

- Create a backup script:

```bash
sudo nano /backup/backup.sh
```

```plaintext
#!/bin/bash

# Variables
BACKUP_DIR="/backup"
DATE=$(date +"%Y%m%d-%H%M%S")

# Backup
sudo dd if=/dev/mmcblk0 bs=4M status=progress | gzip > $BACKUP_DIR/backup-$DATE.img.gz
```

- Make the script executable:

```bash
sudo chmod +x /backup/backup.sh
```

- And we execute it (it will take a while):

```bash
sudo ./backup.sh
```

- Then get the name of the backup:

```bash
ls -l /backup
```

- Exit the Pi and copy the backup to your **Client PC** (replace the date with the one of your backup):

```bash
cd ~/Downloads
scp myhomenas:/backup/backup-20240101-000000.img.gz .
```

- You may decompress the backup with the following command:

```bash
gunzip backup-20240101-000000.img.gz
```

- To restore the backup, you will need to flash the image to a new SD card using [Balena Etcher](https://etcher.balena.io/) as we did in the first section but use this image instead. If everything goes well, you should be able to boot the Pi with the new SD card and have the same configuration as before.

## Next step

You may now proceed to the [OpenMediaVault](./omv-storage.md) storage management section.

## Sources

Here is the most exhaustive I could make of the sources I used to write this guide:

- [Wiki for the installation on Raspberry Pi and affiliates](https://wiki.omv-extras.org/doku.php?id=omv7:raspberry_pi_install) from the [omv-extras](https://wiki.omv-extras.org/doku.php?id=start) documentation.
- [Wiki for the walkthrough of the setup on the WebGUI](https://wiki.omv-extras.org/doku.php?id=omv7:new_user_guide#web_console_login) from the [omv-extras](https://wiki.omv-extras.org/doku.php?id=start) documentation.
- Quad9 DNS servers from [Quad9](https://www.quad9.net/).
- Cloudflare IP ranges from [Cloudflare](https://www.cloudflare.com/ips/).
- Cloudflare [ports list](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/deploy-tunnels/tunnel-with-firewall/) from their zero trust network documentation.

---

Last update: Jan. 2025

Created: Oct. 2024
