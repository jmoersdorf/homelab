# Nextcloud Server Setup

## Overview

This document describes the initial setup of a dedicated Debian 13 virtual machine for hosting Nextcloud in my Proxmox homelab.

The goal is to create a reliable foundation for a self-hosted cloud storage service.

## Environment

- Hypervisor: Proxmox VE
- Virtual machine: nextcloud-server-01
- Guest operating system: Debian 13
- CPU: 2 cores
- Memory: 4 GiB RAM
- System disk: 64 GiB
- Additional data disk: 32 GiB
- Network: Proxmox Linux Bridge
- Storage backend: local-lvm

## Virtual Machine Creation

A new virtual machine was created using the Proxmox VE web interface.

The Debian 13 ISO image was selected as the installation medium.

The required virtual hardware was configured, including CPU cores, memory, system storage and network interface.

## Debian Installation

The virtual machine was started and Debian 13 was installed.

During the installation:

- The hostname was configured
- A regular user account was created
- The system disk was partitioned
- The base operating system was installed
- The SSH server was installed

After installation, the virtual machine was rebooted and Debian started successfully.

## Network Configuration

The virtual machine initially received an IPv4 address through DHCP.

The network configuration was checked using:

```bash
ip a
```

Connectivity within the local network was verified:

```bash
ping -c 4 <local-server-ip>
```

Internet connectivity was tested using:

```bash
ping -c 4 1.1.1.1
ping -c 4 debian.org
```

All tests were successful.

### Static IP Assignment

A permanent DHCP reservation was configured in the FRITZ!Box.

The reserved address is outside the regular DHCP address pool.

After restarting the virtual machine, the assigned address was verified:

```bash
ip -4 addr show ens18
```

The server is now consistently reachable using the reserved IPv4 address.

## SSH Configuration

The OpenSSH server was installed and enabled during the Debian installation.

The service status was verified:

```bash
sudo systemctl status ssh
```

Listening ports were checked:

```bash
sudo ss -tulpn
```

SSH was confirmed to be listening on TCP port 22.

A remote SSH connection from a Windows client was successfully established:

```bash
ssh <username>@<server-ip>
```

The server can now be administered remotely within the local network.

## System Updates

The package lists were updated and available upgrades were checked:

```bash
sudo apt update
sudo apt upgrade
```

The system was confirmed to be up to date.

## Initial System Verification

System resources and storage were checked using:

```bash
free -h
lsblk -f
```

The virtual machine has approximately 4 GiB of RAM and a 64 GiB system disk.

A Proxmox snapshot named `clean-debian-install` was created after the initial configuration.

This provides a recovery point before further changes.

## Additional Data Storage

A second virtual disk with 32 GiB was added through the Proxmox VE hardware configuration.

The disk was detected by Debian as `/dev/sdb`.

### Partitioning

A GPT partition table and a new partition were created:

```bash
sudo fdisk /dev/sdb
```

The new partition was identified as `/dev/sdb1`.

### Filesystem Configuration

An ext4 filesystem was created:

```bash
sudo mkfs.ext4 -L nextcloud-data /dev/sdb1
```

### Mounting

A dedicated mount point was created:

```bash
sudo mkdir -p /mnt/nextcloud-data
sudo mount /dev/sdb1 /mnt/nextcloud-data
```

### Persistent Mounting

The filesystem UUID was retrieved:

```bash
sudo blkid /dev/sdb1
```

An entry was added to `/etc/fstab`:

```text
UUID=<filesystem-uuid> /mnt/nextcloud-data ext4 defaults,nofail 0 2
```

The configuration was verified:

```bash
sudo findmnt --verify --verbose
sudo systemctl daemon-reload
```

Persistent mounting was tested:

```bash
sudo umount /mnt/nextcloud-data
sudo mount -a
```

The result was verified:

```bash
lsblk -f
df -h /mnt/nextcloud-data
```

The additional disk is now mounted successfully and provides approximately 30 GiB of usable storage.

## Result

The initial Nextcloud server infrastructure has been successfully prepared.

- Debian 13 installed and updated
- Network connectivity verified
- Permanent IPv4 address assigned through DHCP reservation
- SSH remote administration working
- Initial Proxmox snapshot created
- Additional ext4 data disk configured
- Persistent storage mounting verified

The server is ready for the next configuration steps.

## Next Steps

- Configure filesystem permissions
- Install the required web server and database
- Install and configure Nextcloud
- Configure the Nextcloud data directory
- Implement security hardening
- Configure backups