# Installing Debian Jessie on WD My Cloud Gen1

> [!NOTE]
> This guide documents the installation of a clean **Debian Jessie** system on a **WD My Cloud Gen1 (Single Bay)**.
>
> The original tutorial was created by **Fox-exe.ru**:
>
> https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/
>
> While following the original guide, I encountered several issues and had to adjust some steps.  
> The procedure below is the version that successfully worked on my **WD My Cloud Gen1**.
>

> [!WARNING]
> **This procedure will erase the entire drive.**
>
> The examples below assume the WD My Cloud drive is detected as:
>
> ```bash
> /dev/sdd
> ```
>
> Verify the correct device first:
>
> ```bash
> lsblk
> ```
>
> Replace `/dev/sdd` with the correct drive on your system.
>
> Using the wrong device will permanently destroy your data.

## 1. Wipe the Disk

Clear existing partition information:

```bash
dd if=/dev/zero of=/dev/sdd bs=1M count=100
```

## 2. Create the Partition Layout

Start `parted`:

```bash
parted /dev/sdd
```

Create a GPT partition table:

```text
mklabel gpt
```

Create the required partitions:

```text
mkpart primary 528M 2576M
mkpart primary 2576M 4624M
mkpart primary 16M 528M
mkpart primary 4828M 100%
mkpart primary 4624M 4724M
mkpart primary 4724M 4824M
mkpart primary 4824M 4826M
mkpart primary 4826M 4828M
```

Enable RAID flags:

```text
set 1 raid on
set 2 raid on
```

Exit:

```text
quit
```

## 3. Format Partitions

Create swap:

```bash
mkswap /dev/sdd3
```

Format the data partition:

```bash
mkfs.ext4 -O ^64bit,^metadata_csum /dev/sdd4
```

## 4. Download Debian Jessie Image

Create a working directory:

```bash
mkdir -p /tmp/wd_jessie
cd /tmp/wd_jessie
```

Download the Debian Jessie image:

```bash
wget https://github.com/abskmj/wd-mycloud-gen1/releases/download/packages/clean-debian-jessie.tgz
```

Extract:

```bash
tar zxvf clean-debian-jessie.tgz
```

The extracted files should include:

```text
kernel.img
config.img
rootfs.img
```

## 5. Write Kernel Images

Write the kernel image:

```bash
dd if=kernel.img of=/dev/sdd5 bs=1M
dd if=kernel.img of=/dev/sdd6 bs=1M
```

## 6. Write Configuration Images

Write the configuration image:

```bash
dd if=config.img of=/dev/sdd7 bs=1M
dd if=config.img of=/dev/sdd8 bs=1M
```

## 7. Write Root Filesystem

Write the Debian root filesystem:

```bash
dd if=rootfs.img of=/dev/sdd1 bs=1M
dd if=rootfs.img of=/dev/sdd2 bs=1M
```

## 8. Create RAID1 Root Array

Stop any detected RAID arrays:

```bash
mdadm --stop /dev/md*
```

Create the RAID1 array:

```bash
mdadm --create /dev/md0 \
  --level=1 \
  --metadata=0.9 \
  --assume-clean \
  --run \
  --force \
  --raid-devices=2 \
  /dev/sdd1 /dev/sdd2
```

Stop the array:

```bash
mdadm --stop /dev/md0
```

## 9. Flush Pending Writes

Sync all pending writes:

```bash
sync
```

The drive is now ready to be installed back into the WD My Cloud Gen1 enclosure.

## First Boot

After reinstalling the drive:

1. Power on the WD My Cloud.
2. Wait several minutes for the first boot process to complete.
3. Find the device IP address from your router DHCP list.
4. Connect using SSH.

## Default SSH Credentials

The default SSH login credentials are:

```text
Username: root
Password: mycloud
```

> [!WARNING]
> Change the default password immediately after the first login:

```bash
passwd
```

Do not expose the device to the internet using the default password.

## Credits

Original tutorial:

https://fox-exe.ru/WDMyCloud/WDMyCloud-Gen1/

Debian Jessie image source:

https://github.com/abskmj/wd-mycloud-gen1/releases
