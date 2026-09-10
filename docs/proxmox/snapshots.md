# Proxmox VM Snapshots

## Overview

Snapshots provide a way to save the current state of a virtual machine before making significant configuration changes.

They allow the virtual machine to be rolled back to a known working state if a later configuration change causes problems.

## Baseline Snapshot

After completing the initial Debian server configuration and security hardening, a baseline snapshot was created for the virtual machine `debian-server-01`.

Snapshot name:

`baseline-after-ssh-hardening`

The system state at this point includes:

- Debian base installation
- Static IPv4 configuration
- UFW firewall enabled
- SSH public key authentication configured
- SSH password authentication disabled
- SSH root login disabled

The snapshot was created without including the VM's RAM state.

## Why This Snapshot Was Created

This snapshot represents a known working and secured baseline of the Debian server.

Future services and configuration changes can now be tested while keeping the option to return to this stable state if necessary.

## Snapshot vs. Backup

A snapshot is not a replacement for a backup.

A snapshot is primarily intended as a short-term recovery point before configuration changes. It depends on the underlying Proxmox storage and therefore does not protect against storage or host failure.

A backup creates a separate copy of the virtual machine and can be stored independently from the VM.

For this homelab, snapshots will be used before significant configuration changes, while a separate backup strategy will be implemented later.
