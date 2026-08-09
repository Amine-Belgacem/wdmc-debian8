# WDMC Debian 8 (Jessie)

This repository contains installation guides, configuration notes, and reference logs for running a bare-metal **Debian Jessie (Debian 8)** operating system on a **first-generation WD My Cloud (Single Bay)** NAS.

> **Disclaimer:** This repository is not intended to be a formal, production-ready tutorial. The documentation is based on a single successful deployment and has not been extensively tested across different hardware revisions, drives, or configurations. The procedures described here modify the device's underlying operating system and storage layout. **Proceed at your own risk.**

## Prerequisites
Before proceeding with the core installation, you must physically open the WD My Cloud enclosure and extract the internal hard drive. The extracted drive must then be connected to a separate computer running a Linux operating system (via a direct SATA connection or a USB-to-SATA adapter).

## Documentation

### Core Installation

- **[Installing Debian Jessie](install-debian.md)** Instructions for installing Debian Jessie on the WD My Cloud.
- **[Installation Terminal Log](install-debian-terminal-log.md)** Raw terminal output from a successful Debian Jessie installation and restoration on a 2 TB WD Red drive.

### Configuration & Services

- **[Post-Installation Setup](post-install.md)** Essential post-installation steps, including configuring APT to use the archived Debian Jessie repositories.
- **[Installing Samba](config-samba.md)** Notes on installing and configuring Samba for network file sharing using the standard Debian Jessie repositories.
- **[Installing Tailscale](config-tailscale.md)** Reference instructions for configuring the WD My Cloud as a Tailscale mesh-network node.

## Credits

The original Debian installation concept is based on the work published by [Fox-exe.ru](https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/).

This repository builds upon that work with additional installation notes, configuration steps, and reference logs.
