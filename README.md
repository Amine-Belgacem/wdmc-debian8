# WDMC Debian 8 (Jessie)

This repository contains guides, configurations, and reference logs for running a bare-metal Debian Jessie (Debian 8) operating system on a first-generation WD My Cloud (Single Bay) NAS.

**Disclaimer:** This is not a formal tutorial. These documents serve as a reference point based on a single successful deployment and have not been thoroughly tested. These steps involve modifying the fundamental operating system. Proceed at your own risk.

## Prerequisites
Before proceeding with the core installation, you must physically open the WD My Cloud enclosure and extract the internal hard drive. The extracted drive must then be plugged into a separate computer running a Linux operating system (via a direct SATA connection or a USB-to-SATA adapter).
## Documentation

### Core Installation
* **[Bare-Metal Installation Guide](install-debian.md)** 
  Instructions for installing Debian Jessie. Based on the original Fox-exe.ru tutorial, adjusted to bypass encountered issues.
* **[Installation Terminal Log](install-debian-terminal-log.md)**
  Raw terminal history of a successful Debian Jessie restore to a 2TB WD Red drive.

### Configuration & Services
* **[Installing Samba](config-samba.md)**
  Notes on configuring Samba file sharing using standard Jessie repositories.
* **[Installing Tailscale](config-tailscale.md)**
  Reference steps for establishing a Tailscale mesh network node.

## Device Specifications
* **Device:** WD My Cloud Gen1 (Single Bay)
* **Target OS:** Debian Jessie 8
* **Architecture:** ARM 

## Credits
* Original Debian installation concept by [Fox-exe.ru](https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/).
