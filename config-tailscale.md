# Installing Tailscale on WDMC

> [!NOTE]
> This document is a reference for installing **Tailscale** on **Debian Jessie (Debian 8)** running on a **WD My Cloud Gen1**.

## 1. Patch the Package Manager for Legacy Debian 8

Clean existing package information:

```bash
apt-get clean
```

Replace the default Debian repository with the archived Jessie repository:

```bash
echo "deb [trusted=yes] http://archive.debian.org/debian/ jessie main" > /etc/apt/sources.list
```

Disable repository expiration checks:

```bash
echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99archive
```

Update package lists:

```bash
apt-get update
```

## 2. Install Required Firewall Utility

Install `iptables`:

```bash
apt-get install iptables -y --allow-unauthenticated
```

`iptables` is required by Tailscale for network configuration.

## 3. Download Tailscale ARM Binary

Download the compatible Tailscale release:

```bash
wget https://pkgs.tailscale.com/stable/tailscale_1.72.0_arm.tgz
```

Extract the archive:

```bash
tar xzvf tailscale_1.72.0_arm.tgz
```

Enter the extracted directory:

```bash
cd tailscale_1.72.0_arm
```

## 4. Install Tailscale Binaries

Copy the Tailscale executables:

```bash
cp tailscaled /usr/sbin/
cp tailscale /usr/bin/
```

Install the systemd service:

```bash
cp systemd/tailscaled.service /etc/systemd/system/
```

Create required runtime directories:

```bash
mkdir -p /var/lib/tailscale
mkdir -p /var/run/tailscale
```

## 5. Patch the systemd Service

The supplied service file causes `tailscaled` to crash on Debian Jessie because of an invalid port argument.

Apply the fix:

```bash
sed -i 's/--port=\${PORT} \$FLAGS/--port=41641/g' /etc/systemd/system/tailscaled.service
```

This replaces the invalid dynamic port argument with the fixed Tailscale WireGuard port.


## 6. Enable and Start Tailscale

Reload systemd configuration:

```bash
systemctl daemon-reload
```

Enable Tailscale at boot:

```bash
systemctl enable tailscaled
```

Start the service:

```bash
systemctl start tailscaled
```

Check service status:

```bash
systemctl status tailscaled
```

## 7. Authenticate the Device

Start the Tailscale login process:

```bash
tailscale up
```

The command will provide an authentication URL.

Open the URL in a browser and authorize the device.

Verify the connection:

```bash
tailscale status
```
