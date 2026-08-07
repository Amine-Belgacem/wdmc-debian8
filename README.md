# WD My Cloud Gen1 - Debian Jessie Guides

This repository contains guides, configurations, and reference logs for running a clean, bare-metal Debian Jessie (Debian 8) operating system on a first-generation WD My Cloud (Single Bay) NAS.

**Disclaimer:** These steps involve modifying the fundamental operating system of your NAS. Proceed at your own risk.

## Available Documentation

### 🛠️ Core Installation
* **[Bare-Metal Installation Guide](install-debian.md)** 
  Step-by-step instructions for installing Debian Jessie. Based on the original Fox-exe.ru tutorial, but adjusted to fix several encountered issues.
* **[Installation Terminal Log](install-debian-terminal-log.md)**
  A raw terminal history of a successful Debian Jessie restore to a 2TB WD Red drive. Useful as a reference if you encounter errors during your own install.

### ⚙️ Configuration & Services
* **[Installing Samba](config-samba.md)**
  Guide to installing and configuring Samba file sharing on the clean Debian system using the standard Jessie repositories.
* **[Installing Tailscale](config-tailscale.md)**
  Reference guide for getting a Tailscale mesh network node running on the WD My Cloud Gen1. 

## Device Specifications
* **Device:** WD My Cloud Gen1 (Single Bay)
* **Target OS:** Debian Jessie 8
* **Architecture:** ARM 

## Credits
* Original Debian installation concept and initial tutorials by [Fox-exe.ru](https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/).
