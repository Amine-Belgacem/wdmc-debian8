# WDMC Debian 8 (Jessie)

This repository contains guides, configurations, and reference logs for running a bare-metal Debian Jessie (Debian 8) operating system on a first-generation WD My Cloud (Single Bay) NAS.

> **Disclaimer:** This is not a formal tutorial. These documents serve as a reference point based on a single successful deployment and have not been thoroughly tested. These steps involve modifying the fundamental operating system. Proceed at your own risk.

## Device Specifications
* **Device:** WD My Cloud Gen1 (Single Bay)
* **Target OS:** Debian 8 (Jessie)
* **Architecture:** ARM 

## Prerequisites
Before proceeding with the core installation, you must physically open the WD My Cloud enclosure and extract the internal hard drive. The extracted drive must then be connected to a separate computer running a Linux operating system (via a direct SATA connection or a USB-to-SATA adapter).

## Documentation

### Core Installation
* **[Bare-Metal Installation Guide](install-debian.md):** Instructions for installing Debian Jessie. Based on the original Fox-exe.ru tutorial, adjusted to bypass encountered issues.
* **[Installation Terminal Log](install-debian-terminal-log.md):** Raw terminal history of a successful Debian Jessie restore to a 2TB WD Red drive.

## Base Post-Installation Setup

Because Debian 8 is out of support, the default package repositories are no longer active. You must patch the package manager to point to the Debian archives *before* installing any additional software or services (like Samba or Tailscale). 

Run the following commands immediately after completing the bare-metal installation:

```bash
# Clean existing package information
apt-get clean

# Replace the default repository with the archived Jessie repository
echo "deb [trusted=yes] [http://archive.debian.org/debian/](http://archive.debian.org/debian/) jessie main" > /etc/apt/sources.list

# Disable repository expiration checks
echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99archive

# Update package lists
apt-get update
```

## Configuration & Services
Once the base setup and repository patches are complete, you can proceed with setting up additional services:

* **[Installing Samba](config-samba.md):** Notes on configuring Samba file sharing using standard Jessie repositories.
* **[Installing Tailscale](config-tailscale.md):** Reference steps for establishing a Tailscale mesh network node.

## Credits
* Original Debian installation concept by [Fox-exe.ru](https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/).
