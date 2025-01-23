# Docker and Cloudflare tunnels

> This section will teach you how to install and configure Docker on OpenMediaVault. It will also cover the installation of some useful containers.

- **Want to go back to the index page?** [click here](../index.md).

## Table of Contents

- [Docker and Cloudflare tunnels](#docker-and-cloudflare-tunnels)
  - [Table of Contents](#table-of-contents)
  - [I - Install docker](#i---install-docker)
  - [II - Some useful containers](#ii---some-useful-containers)
  - [III - Exposing your services to the web](#iii---exposing-your-services-to-the-web)
  - [IV - Adding Security layers](#iv---adding-security-layers)
  - [Conclusion](#conclusion)
  - [Sources](#sources)

## I - Install docker

> The tool we will need will be docker composer to be precise. A declarative way to define and run multi-container Docker applications. Some examples will be given to you later on.

- Then, go to the `omv-extras` tab and enable `Docker repo`.

![Docker](../assets/img/omv/docker-repo.png)

- Go to the `System` tab and click on `Plugins` in the left sidebar and look for `openmediavault-compose` and click on install.

![Docker](../assets/img/omv/compose-install.png)

- Once installed, a new category should have been created in the `Services` tab and click on `Compose` in the left sidebar.

![Docker](../assets/img/omv/compose-tabs.png)

- Under the `Settings` tab, assign these variables:
  - `Compose Files`:
    - `Shared folder`: `containers_config`
    - `Owner`: `sysadmin`
    - `Group`: `sysadmin`
    - `Permissions`: `Administrator - read/write, Users - read/write, Others - no access`
  - `Data`:
    - `Shared folder`: `containers_data`
  - `Backup`:
    - `Shared folder`: `containers_backup`
    - `Max Size`: `0` (unlimited)

![Docker](../assets/img/omv/compose-settings.png)

-

## II - Some useful containers

- LibreOffice
- Duplicati
- Calibre web

## III - Exposing your services to the web

## IV - Adding Security layers

## Conclusion

Thank you for following these guides. I hope you found them useful. If you have any questions or suggestions, feel free to reach out to me or [open an issue](https://github.com/MorganKryze/MyHomeNAS/issues) on the GitHub repository.

## Sources

- [Wiki of the compose plugin](https://wiki.omv-extras.org/doku.php?id=omv6:omv6_plugins:docker_compose) from the [omv-extras](https://wiki.omv-extras.org/doku.php?id=start) documentation
- [Umbrel OS app store](https://apps.umbrel.com/category/all) gave me some ideas for the containers to install.

---

Last update: Jan. 2025
Created: Jan. 2025
