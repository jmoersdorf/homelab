# SSH Client Configuration

## Overview

This document describes the configuration of SSH access from the Windows 11 virtual machine to the Debian server.

SSH is used for secure remote administration of the Linux server.

## SSH Key

An ED25519 SSH key pair was created on the Windows 11 VM.

The private key remains on the Windows system, while the public key was added to the Debian user's `authorized_keys` file.

A passphrase was configured to protect the private key.

## Connection Test

Before establishing the SSH connection, network connectivity and TCP port 22 were tested successfully.

The SSH connection was then established using public key authentication.

After connecting, the remote system and user were verified with:

`hostname`

`whoami`

The SSH session was closed with:

`exit`

## Security

Password-based SSH authentication on the Debian server had previously been disabled as part of the SSH hardening configuration.

Remote access therefore uses SSH public key authentication.

Sensitive information such as private keys, public keys, fingerprints, passwords, and IP addresses is not stored in this repository.

## What I Learned

During this configuration, I gained practical experience with:

- SSH remote administration
- ED25519 SSH key pairs
- Public and private keys
- Public key authentication
- The `authorized_keys` file
- Testing TCP port connectivity
- Connecting from Windows to a Linux server
- Securely terminating SSH sessions