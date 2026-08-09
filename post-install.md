# Post-Installation Setup (Debian 8 Jessie)

Because Debian 8 (Jessie) has reached End-of-Life (EOL), the standard Debian repositories are no longer active or reachable at their original URLs. Attempting to run `apt-get update` on a fresh Debian Jessie installation will result in repository fetch errors.

Before installing any additional software packages or services (such as Samba or Tailscale), you must patch the package manager configuration to point to the Debian Archive repositories and bypass expiration checks.

## Repository Patch Instructions

Run the following commands as `root` (or with `sudo`) immediately after completing the bare-metal Debian 8 installation:

### 1. Clean Existing Package Cache
Clear out local repository indexes and cached package files to prevent conflicts with stale metadata.

```bash
apt-get clean
```

### 2. Update Repository Sources
Replace the default repositories with the official archived Jessie repository. This points `apt` to the permanent archive server and skips GPG signature verification errors caused by expired archive keys.

```bash
echo "deb [trusted=yes] [http://archive.debian.org/debian/](http://archive.debian.org/debian/) jessie main" > /etc/apt/sources.list
```

### 3. Disable Expiration Checks
Disable repository expiration checks (required for EOL releases). This prevents `apt` from failing when checking the Release file timestamp, which has long expired for Debian 8.

```bash
echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99archive
```

### 4. Update Package Lists
Fetch the updated package lists from the archived repositories.

```bash
apt-get update
```

## Verification

After running the final `apt-get update` command, you should see output indicating that package lists were successfully retrieved from `archive.debian.org` without fatal errors:

```text
Get:1 [http://archive.debian.org](http://archive.debian.org) jessie InRelease [144 kB]
Get:2 [http://archive.debian.org](http://archive.debian.org) jessie/main armhf Packages [6,762 kB]
Fetched 6,906 kB in ...
Reading package lists... Done
```

Once this step succeeds, your package manager is ready to install system utilities, file shares, or network tools.
