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

## Explanation of Changes

1. **`apt-get clean`**: Clears out local repository indexes and cached package files to prevent conflicts with stale metadata.
2. **`[trusted=yes] http://archive.debian.org/debian/`**: Points `apt` to the permanent archive server for Debian 8 and skips GPG signature verification errors caused by expired archive keys.
3. **`Acquire::Check-Valid-Until "false"`**: Prevents `apt` from failing when checking the Release file timestamp, which has long expired for Debian 8.

## Verification

After running `apt-get update`, you should see output indicating that package lists were successfully retrieved from `archive.debian.org` without fatal errors:

```text
Get:1 http://archive.debian.org jessie InRelease [144 kB]
Get:2 http://archive.debian.org jessie/main armhf Packages [6,762 kB]
Fetched 6,906 kB in ...
Reading package lists... Done
```

Once this step succeeds, your package manager is ready to install system utilities, file shares, or network tools.
