# Windows 11 VM Installation

## Overview

This document describes the installation and basic configuration of a Windows 11 virtual machine in my Proxmox VE homelab.

The VM is used to gain practical experience with Windows administration, networking and virtualization.

## Virtual Machine

The Windows 11 VM was created and configured through the Proxmox VE web interface.

The virtual machine includes:

- Windows 11 Pro
- 4 virtual CPU cores
- 4 GB RAM
- 64 GB virtual disk
- VirtIO virtual hardware

## Installation

Windows 11 was installed from an ISO image attached to the virtual machine.

After the operating system installation, the basic Windows configuration was completed.

The installation included:

- Installing Windows 11 Pro
- Installing Windows updates
- Configuring the virtual network adapter
- Configuring a static IPv4 address
- Testing local and Internet connectivity

Specific IP addresses and other sensitive network information are not included in this repository.

## VirtIO Guest Tools

The VirtIO Guest Tools were installed after the Windows installation.

They provide optimized drivers for Windows virtual machines running on KVM/QEMU.

The installation included drivers for virtual devices such as:

- Network adapter
- Storage controller
- Memory management
- Virtual devices

## QEMU Guest Agent

The QEMU Guest Agent was installed and configured to start automatically.

It improves communication between the Proxmox host and the Windows guest and allows Proxmox to retrieve additional information about the virtual machine.

## Network Configuration

The Windows VM is connected to the homelab network through the Proxmox virtual bridge.

A static IPv4 configuration was assigned outside the router's DHCP range.

Connectivity was successfully tested between:

- Windows VM and Proxmox host
- Windows VM and Debian server
- Windows VM and default gateway
- Windows VM and the Internet

DNS resolution and IPv6 connectivity were also verified.

## Snapshot

After completing the initial configuration and testing the system, a baseline snapshot was created.

The snapshot provides a known working state before further configuration changes are made.

## What I Learned

During the Windows 11 VM setup, I gained practical experience with:

- Creating Windows virtual machines in Proxmox
- Installing Windows in a virtualized environment
- Installing VirtIO drivers
- Using the QEMU Guest Agent
- Configuring static IPv4 addressing
- Testing network connectivity
- Understanding communication between virtual machines
- Creating VM snapshots