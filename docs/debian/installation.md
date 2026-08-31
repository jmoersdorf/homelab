# Debian Server Installation

## Objective

The goal was to deploy a Debian server as a virtual machine on the Proxmox VE host. This virtual machine will serve as a foundation for further homelab services and Linux administration practice.

## Environment

- Hypervisor: Proxmox VE
- Guest operating system: Debian
- Hostname: `debian-server-01`
- Virtualization type: Virtual Machine (VM)

## Debian ISO

A Debian ISO image was uploaded to the Proxmox VE host and used as the installation medium for the virtual machine.

An ISO image contains the files required to install an operating system and can be attached to a virtual machine like a virtual installation disc.

## Virtual Machine Creation

A new virtual machine was created in Proxmox VE.

The Debian ISO image was selected as the installation medium and the required virtual hardware was assigned to the VM.

The virtual hardware includes resources such as:

- CPU cores
- Memory (RAM)
- Virtual disk storage
- Network interface

## Debian Installation

The virtual machine was started and booted from the Debian ISO image.

Debian was then installed using the installer. During the installation, the basic system configuration and user account were created.

After the installation was completed, the system was rebooted and Debian started from the virtual disk.

## Initial Verification

After the first boot, the Debian server was checked to verify that the operating system was running correctly.

The server is identified by the following hostname:

```text
debian-server-01
```

The system can be administered remotely over the local network using SSH.

## Result

A functional Debian virtual machine is running on the Proxmox VE host and provides the Linux server environment for the homelab.

Further configuration, including networking, firewall configuration and SSH hardening, is documented separately.