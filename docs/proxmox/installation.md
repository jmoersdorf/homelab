# Proxmox VE Installation

## Overview

This document describes the installation and basic setup of Proxmox VE on my homelab server.

Proxmox VE is used as the virtualization platform for running and managing virtual machines and containers.

## Hardware

The Proxmox host is a Lenovo ThinkCentre M920q Tiny with the following specifications:

- Intel Core i5-8500T
- 16 GB RAM
- 512 GB SSD
- Gigabit Ethernet

## Installation

Proxmox VE was installed directly on the physical server.

The installation included:

- Installing Proxmox VE on the internal SSD
- Configuring the management network
- Setting a hostname for the Proxmox host
- Accessing the Proxmox web interface
- Performing the initial system configuration

## Management

The Proxmox server is managed through the Proxmox VE web interface.

For security reasons, passwords, credentials and sensitive network information are not included in this repository.

## Virtual Machines

The first virtual machine created in the homelab is a Debian Linux server.

Further virtual machines and services will be documented separately as the homelab grows.

## What I Learned

During the initial Proxmox setup, I gained practical experience with:

- Type 1 hypervisors
- Virtual machines
- Basic Linux administration
- Network configuration
- Web-based server management