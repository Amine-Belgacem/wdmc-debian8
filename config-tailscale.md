# Installing Tailscale on WDMC

> [!NOTE]
> This guide explains how to install and configure **Tailscale** on a **Debian 8** system running on a **WD My Cloud Gen 1**.
>
> It uses the **Tailscale 1.72.0 ARM static binary**, which is used here as a known-compatible version for this legacy Debian 8 installation.
>
> Complete [`post-install.md`](post-install.md) before following this guide.

## 1. Install Required Firewall Utility

Install `iptables`:

```bash
apt-get install iptables -y --allow-unauthenticated
```

## 2. Download Tailscale ARM Binary

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

## 3. Install Tailscale Binaries

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

## 4. Patch the systemd Service

The supplied service file causes `tailscaled` to crash on Debian 8 because of an invalid port argument.

Apply the fix:

```bash
sed -i 's/--port=\${PORT} \$FLAGS/--port=41641/g' /etc/systemd/system/tailscaled.service
```

This replaces the invalid dynamic port argument with the fixed Tailscale WireGuard port.


## 5. Enable and Start Tailscale

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

## 6. Authenticate the Device

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
