# Debian Network Configuration

## Objective

The Debian server network configuration was adjusted to provide a predictable and reliable IP address for remote administration and future homelab services.

## Network Interface

The primary network interface of the Debian virtual machine is:

```text
ens18
```

The network interface connects the virtual machine to the network through the virtual networking provided by Proxmox VE.

## Initial Configuration

Initially, the Debian server obtained its IPv4 address automatically using DHCP.

The network configuration contained:

```text
allow-hotplug ens18
iface ens18 inet dhcp
```

With DHCP enabled, the IP address is assigned automatically by the DHCP server on the local network.

## Static IP Address

For a server, a predictable IP address is useful because services and administration tools need to reliably reach the same system.

The Debian server was therefore configured to use the following IPv4 address:

```text
192.168.178.20
```

After applying the network configuration and rebooting the server, the new address was verified from another device on the local network.

## Connectivity Test

Network connectivity was tested from the Windows client using:

```powershell
ping 192.168.178.20
```

The server responded successfully with no packet loss.

SSH connectivity was then tested using:

```powershell
ssh jonas@192.168.178.20
```

The connection was successful, confirming that the server was reachable at its new IP address.

## Result

The Debian server is reachable on the local network using a predictable IPv4 address.

This provides a stable foundation for remote administration and for services that will be added to the homelab later.