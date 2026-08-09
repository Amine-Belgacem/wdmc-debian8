# Installing Samba on Debian 8 WDMC
## 1. Install Samba

Update the package lists:

```bash
apt-get update
```

Install the Samba server:

```bash
apt-get install samba -y
```

This installs the Samba server, required services, and supporting utilities.


## 2. Verify Your Storage Location

List all mounted filesystems:

```bash
df -h
```

Identify the large data partition that will store your shared files.

For this guide, the shared directory will be:

```text
/srv/myshare
```


## 3. Create the Shared Directory

Create the directory:

```bash
mkdir -p /srv/myshare
```

Grant read, write, and execute permissions to all users:

```bash
chmod -R 0777 /srv/myshare
```

> [!WARNING]
> Using `0777` makes the directory writable by any local user.
>
> This is acceptable for a dedicated NAS appliance on a trusted network, but you may wish to use more restrictive permissions depending on your environment.


## 4. Create a Samba User

Add the `root` account to the Samba user database:

```bash
smbpasswd -a root
```

You will be prompted to enter a password twice.

This password is used when connecting to the SMB share and does not change the Linux root password.


## 5. Configure the SMB Share

Append the following share definition to the Samba configuration:

```bash
cat <<EOF >> /etc/samba/smb.conf

[WD_Share]
   path = /srv/myshare
   browseable = yes
   read only = no
   guest ok = no
   valid users = root
EOF
```

This creates an SMB share named **WD_Share** with the following settings:

- **path** – Directory shared over the network.
- **browseable** – The share is visible to clients.
- **read only = no** – Allows file uploads, modifications, and deletions.
- **guest ok = no** – Requires authentication.
- **valid users = root** – Only the Samba `root` user may access the share.


## 6. Restart Samba

Restart the Samba services:

```bash
systemctl restart smbd nmbd
```

The SMB share is now available on the network.


## Accessing the Share

From another computer, connect to the NAS using either its hostname or IP address.

Examples:

```text
\\wdmycloud\WD_Share
```

or

```text
\\192.168.x.x\WD_Share
```

When prompted, authenticate using:

```text
Username: root
Password: <the password created with smbpasswd>
```

## Notes

- This guide creates a single authenticated SMB share.
- Additional shares can be added by appending more sections to `/etc/samba/smb.conf`.
- If the share is not visible immediately, verify that the `smbd` and `nmbd` services are running and that your client and NAS are on the same network.
