# Post-Installation Setup (Debian 8 Jessie)

Because Debian 8 (Jessie) has reached End-of-Life (EOL), the standard Debian repositories are no longer active or reachable at their original URLs. Attempting to run `apt-get update` on a fresh Debian Jessie installation will result in repository fetch errors.

Before installing any additional software packages or services (such as Samba or Tailscale), you must patch the package manager configuration to point to the Debian Archive repositories and bypass expiration checks.

## Repository Patch Instructions

Run the following commands as `root` (or with `sudo`) immediately after completing the bare-metal Debian 8 installation:

```bash
# Clean existing package cache
apt-get clean

# Replace default repositories with the official archived Jessie repository
echo "deb [trusted=yes] http://archive.debian.org/debian/ jessie main" > /etc/apt/sources.list

# Disable repository expiration checks (required for EOL releases)
echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99archive

# Update package lists
apt-get update
```

After running `apt-get update`, you should see output indicating that package lists were successfully retrieved from `archive.debian.org` without fatal errors:

Once this step succeeds, your package manager is ready to install system utilities, file shares, or network tools.
