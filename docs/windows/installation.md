# Windows 11 VM Installation on Proxmox VE

## Overview

This document describes the installation and initial configuration of a Windows 11 virtual machine on my Proxmox VE homelab.

The goal of this VM is to gain practical experience with Windows administration, networking, virtualization, and remote management.

---

## VM Information

| Setting | Value |
|---|---|
| VM Name | windows11 |
| VM ID | 101 |
| Operating System | Windows 11 Pro |
| Hypervisor | Proxmox VE |
| CPU | 4 vCPUs |
| Memory | 4 GiB |
| Disk | 64 GiB |
| Network Adapter | VirtIO |
| IPv4 Address | 192.168.178.21/24 |
| Default Gateway | 192.168.178.1 |
| DNS Server | 192.168.178.1 |

---

## Windows Installation

Windows 11 was installed using an ISO image attached to the virtual machine.

During the installation, the VM initially did not have a working network connection because the VirtIO network driver was not yet available to Windows.

The Windows installation was completed first and the required VirtIO drivers were installed afterwards.

---

## VirtIO Guest Tools

After Windows was installed, the VirtIO driver ISO was mounted to the VM.

The following installer was started from the VirtIO CD:

`virtio-win-guest-tools.exe`

The VirtIO Guest Tools provide optimized drivers for running Windows inside a KVM/QEMU virtual machine.

After installation, the Windows Device Manager was checked to verify that the required virtual hardware was detected correctly.

---

## QEMU Guest Agent

The QEMU Guest Agent was installed together with the VirtIO Guest Tools.

The Windows Services console was used to verify that the service was running.

Service:

`QEMU Guest Agent`

Configuration:

- Status: Running
- Startup Type: Automatic

The QEMU Guest Agent improves communication between the Proxmox host and the Windows guest operating system.

It allows Proxmox to retrieve information such as the IP addresses of the VM and enables improved VM management.

---

## Network Configuration

The VM initially received an IPv4 address via DHCP.

Initial DHCP address:

`192.168.178.66/24`

A static IPv4 configuration was configured afterwards.

### Current IPv4 Configuration

| Setting | Value |
|---|---|
| IPv4 Address | 192.168.178.21 |
| Prefix Length | /24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.178.1 |
| DNS Server | 192.168.178.1 |

The static address is outside the DHCP pool of the router.

The DHCP pool currently uses:

`192.168.178.50 - 192.168.178.200`

This prevents the DHCP server from dynamically assigning `192.168.178.21` to another device.

---

## Network Tests

After configuring the static IP address, network connectivity was verified using `ping`.

The following connections were tested successfully:

- Windows VM → Default Gateway
- Windows VM → Proxmox Host
- Windows VM → Debian Server
- Windows VM → Internet IPv4
- DNS name resolution
- IPv6 connectivity

The following commands were used:

```cmd
ping 192.168.178.1
ping 192.168.178.10
ping 192.168.178.20
ping 8.8.8.8
ping google.de
```

All tests completed successfully without packet loss.

### What the Tests Verify

`ping 192.168.178.1`

Tests connectivity between the Windows VM and the default gateway.

`ping 192.168.178.10`

Tests connectivity between the Windows VM and the Proxmox VE host.

`ping 192.168.178.20`

Tests connectivity between the Windows VM and the Debian server.

`ping 8.8.8.8`

Tests IPv4 connectivity to the Internet without relying on DNS name resolution.

`ping google.de`

Tests DNS name resolution and external network connectivity.

In this test, Windows resolved the hostname to an IPv6 address, confirming that IPv6 connectivity is also working.

---

## Homelab Network

The current relevant network structure is:

```text
192.168.178.0/24
│
├── 192.168.178.1    FRITZ!Box / Default Gateway
│
├── 192.168.178.10   Proxmox VE Host
│
├── 192.168.178.20   Debian Server
│
└── 192.168.178.21   Windows 11 VM
```

All systems are currently located inside the `192.168.178.0/24` network.

Devices inside the same `/24` network can communicate directly without sending their traffic through the default gateway.

For IPv4 communication inside the local network, ARP (Address Resolution Protocol) is used to resolve an IPv4 address to the corresponding MAC address.

Traffic destined for networks outside `192.168.178.0/24` is sent to the default gateway at:

`192.168.178.1`

---

## Windows Updates

Windows Update was executed after installation.

The system reported that it was up to date before the baseline snapshot was created.

Keeping the operating system updated is important for:

- Security patches
- Bug fixes
- Driver updates
- System stability

---

## Windows Activation

Windows 11 Pro is currently not activated.

The activation status does not prevent the VM from being used for the current homelab and administration exercises.

---

## Baseline Snapshot

After completing the installation and verifying the configuration, a Proxmox snapshot was created.

Snapshot name:

`windows11-baseline`

The snapshot represents a known working state of the VM with:

- Windows 11 installed
- Windows updates installed
- VirtIO Guest Tools installed
- QEMU Guest Agent installed and running
- Static IPv4 configuration
- Working local network connectivity
- Working Internet connectivity
- Working DNS resolution
- Working IPv6 connectivity

The snapshot can be used as a rollback point before future configuration changes or administration exercises.

---

## Snapshot vs. Backup

A snapshot is useful for quickly returning a virtual machine to a previous state.

However, a snapshot is not a replacement for a proper backup.

If the Proxmox storage containing the VM is lost or damaged, snapshots stored on the same storage can also be lost.

A proper backup should therefore be stored separately from the original VM whenever possible.

---

## Current Status

The Windows 11 VM has reached its first stable baseline.

Current status:

- [x] Windows 11 Pro installed
- [x] VirtIO Guest Tools installed
- [x] QEMU Guest Agent installed
- [x] QEMU Guest Agent running automatically
- [x] Windows updates installed
- [x] Static IPv4 address configured
- [x] Default gateway configured
- [x] DNS configured
- [x] Proxmox connectivity verified
- [x] Debian server connectivity verified
- [x] Internet connectivity verified
- [x] DNS resolution verified
- [x] IPv6 connectivity verified
- [x] Baseline snapshot created

---

## Next Steps

Planned exercises for this VM include:

- Remote Desktop (RDP)
- Windows Firewall configuration
- Local user and group administration
- PowerShell administration
- SMB file sharing
- Windows security configuration
- Remote administration
- Integration into a future Active Directory lab environment

---

## Key Terms

**Hypervisor**  
Software used to create and manage virtual machines. Proxmox VE acts as the hypervisor in this homelab.

**Guest Operating System**  
The operating system running inside a virtual machine. Windows 11 is the guest operating system in this VM.

**Static IP Address**  
An IP address that is manually configured and remains unchanged unless an administrator modifies it.

**Default Gateway**  
The router used to reach networks outside the local subnet.

**DNS (Domain Name System)**  
Translates domain names such as `google.de` into IP addresses.

**ARP (Address Resolution Protocol)**  
Used in IPv4 networks to determine the MAC address associated with an IPv4 address on the local network.

**Baseline**  
A known and verified system state that can be used as a reference for future changes.

**Snapshot**  
A saved state of a virtual machine that can be used to roll the VM back to an earlier configuration.

**Rollback**  
Returning a system to a previously saved state.

**Remote Management**  
Administering a computer or server from another system over a network.