# Remote Desktop Access

## Overview

Remote Desktop Protocol (RDP) was configured to provide remote access to the Windows 11 Pro virtual machine.

RDP allows the VM to be used from another Windows computer without relying on the Proxmox web console.

## Configuration

Remote Desktop was enabled in:

Settings → System → Remote Desktop

A dedicated local user account was created for remote access.

The account was added to the following local Windows group:

Remote Desktop Users

This follows the principle of least privilege because the RDP account does not require administrator permissions for normal remote access.

## Connection

The Windows Remote Desktop Connection client (`mstsc`) is used to connect to the VM.

The connection is established using the VM's static IP address and the credentials of the dedicated RDP user.

## Audio Redirection

Remote audio playback was enabled in the RDP client:

Local Resources → Remote audio → Settings

The following option was selected:

Remote audio playback:
- Play on this computer

Windows then provides a virtual audio device called:

Remote Audio

Audio generated inside the Windows 11 VM is redirected through the RDP session and played on the client computer.

## Result

Remote Desktop access was successfully established.

Verified functionality:

- Remote login with a dedicated local user
- Remote Desktop access over the local network
- Keyboard and mouse input
- Clipboard redirection
- Remote audio playback

The Windows 11 VM can now be used as a remote workstation without requiring direct access through the Proxmox console.