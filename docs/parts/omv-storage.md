# OpenMediaVault storage management

> This guide will help you set up your storage devices on OpenMediaVault. It will cover the checkup and monitoring of the drives, the creation of a Btrfs file system, and the creation of shared folders.

- **Want to go back to the index page?** [click here](../index.md).

## Table of Contents

- [OpenMediaVault storage management](#openmediavault-storage-management)
  - [Table of Contents](#table-of-contents)
  - [I - Checkup and monitoring](#i---checkup-and-monitoring)
  - [II - Btrfs](#ii---btrfs)
    - [First scenario: 2 portable HDD](#first-scenario-2-portable-hdd)
    - [Second scenario: 3 HDD](#second-scenario-3-hdd)
  - [III - Shared folders](#iii---shared-folders)
  - [IV - Samba shares](#iv---samba-shares)
  - [Next step](#next-step)

## I - Checkup and monitoring

> This section will be about checking the status of the drives and enabling monitoring on each one of them (if they are SMART compatible).

- Check that all the devices you connected appear in `Storage` > `Disks`. Note that `/dev/mmcblk0` is the SD card where the OS is installed.

- Then go to `Storage` > `SMART` > `Settings` and set:
  - `check interval` to `1800`
  - `power mode` to `standby`
  - `Temperature monitoring` to `disabled`.

![SMART settings](../assets/img/omv/smart-settings.png)

- Then go to `Storage` > `SMART` > `Devices` and enable monitoring on each device that supports it by editing the device and setting `Monitoring enabled` to `yes`.

![SMART devices](../assets/img/omv/smart-devices.png)

## II - Btrfs

> Btrfs is a modern copy-on-write file system for Linux that provides features like snapshots, subvolumes, RAID, and more. It's an excellent alternative to ZFS, offering similar benefits.

### First scenario: 2 portable HDD

> This scenario is about creating a RAID 1 (mirroring) array with 2 portable hard drives. The drives should be the same size and have the same partition layout. You may erase them before this process.

- Go to `Storage` > `File Systems` and click on `Create` to create a new file system. Choose `BTRFS` and select the devices you want to use.

![Btrfs creation](../assets/img/omv/filesystem-1.png)

- After completion, fill the fields as follows:

![Btrfs creation](../assets/img/omv/filesystem-2.png)

### Second scenario: 3 HDD

> Coming soon...

## III - Shared folders

> This section will tackle your first shared folder creation.

- Go to `Storage` > `Shared Folders` and click on `Add` and create a new shared folder named `containers_config`. When selecting the file system, choose the one you created earlier, not the boot drive.

![Shared folder creation](../assets/img/omv/shared-folder.png)

- Hit Save and Apply pending configuration changes.

- Then create 2 more shared folders: `containers_data` and `containers_backups`.

![Shared folder creation](../assets/img/omv/shared-folder-list.png)

- Then go to `Users` > `Groups`:

![Groups](../assets/img/omv/shared-folders-groups.png)

- Finally, update `shared folders permissions` to `read/write` for the `sysamdin` group.

![Permissions](../assets/img/omv/shared-folders-permissions.png)

## IV - Samba shares

> Samba shares will allow you to access your shared folders from Windows, macOS, and Linux remotely.

- First, I would recommend creating a new user that will only be used for Samba shares. Go to `Users` > `User` and click on `Add` to create a new user. Be sure to add the user to the `sysadmin` and `samabashare` groups.

- Go to `Services` > `SMB/CIFS` and enable the service.

- Then go to `Shares` and click on `Add` to create a new share. Fill the fields as follows:
- `Shared folder`: select the shared folder you want to share.
- `Comment`: a description of the share.
- `Public`: no.
- `Browsable`: yes.
- `Recycle bin`: yes.
- `Maximal file size`: Unrestricted.
- `Retention period`: 30 days.

- Hit save, and apply pending configuration changes.
- Then from a computer on the same network, you should be able to access the shared folders by accessing the `smb://<OMV_IP>/your_shared_folder` or `smb://myhomenas.local/your_shared_folder` address. That might differ from one OS to another, please check online.

## Next step

You may now proceed to the [OMV docker & Cloudflare tunnel](./omv-docker.md) setup.

---

Last update: Jan. 2025

Created: Jan. 2025
