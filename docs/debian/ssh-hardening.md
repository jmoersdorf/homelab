# SSH Hardening – Debian Server

## Objective

The SSH access to the Debian server was hardened to improve security. Password-based authentication was disabled and replaced with public-key authentication.

## Environment

- Operating system: Debian
- Hostname: `debian-server-01`
- Administration from a Windows client
- SSH access via the local network

## UFW Firewall

UFW (Uncomplicated Firewall) was installed and enabled to control incoming network connections.

```bash
sudo apt install ufw
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status verbose
```

The resulting firewall policy:

- Incoming connections are denied by default.
- Outgoing connections are allowed by default.
- OpenSSH connections are explicitly allowed.

## SSH Key Generation

An ED25519 SSH key pair was generated on the Windows client:

```powershell
ssh-keygen -t ed25519
```

The private key remains exclusively on the client. Only the public key is transferred to the server.

## Public Key Configuration

The public key was added to the following file on the Debian server:

```text
~/.ssh/authorized_keys
```

The permissions of the SSH directory and the `authorized_keys` file were restricted:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

This ensures that only the account owner can access the SSH configuration and authorized keys.

## SSH Server Hardening

A separate configuration file was created instead of modifying the main `sshd_config` file:

```text
/etc/ssh/sshd_config.d/99-homelab-hardening.conf
```

The following settings were configured:

```text
PubkeyAuthentication yes
PasswordAuthentication no
PermitRootLogin no
```

This configuration ensures that:

- Public-key authentication is enabled.
- Password-based SSH authentication is disabled.
- Direct SSH login as `root` is disabled.

## Configuration Validation

Before reloading the SSH service, the configuration was checked for syntax errors:

```bash
sudo sshd -t
```

The effective SSH configuration was then verified:

```bash
sudo sshd -T | grep -E 'passwordauthentication|pubkeyauthentication|permitrootlogin'
```

The relevant settings were confirmed as:

```text
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
```

After successful validation, the SSH service was reloaded:

```bash
sudo systemctl reload ssh
```

## Connection Test

The existing SSH session was kept open while testing the new configuration to prevent an accidental lockout.

A second SSH connection was opened from the Windows client. Authentication using the ED25519 private key was successful.

Only after confirming that the new connection worked correctly was the original SSH session closed.

## Result

The Debian server now accepts SSH authentication using public keys while password-based authentication and direct root login are disabled.

UFW is active and explicitly allows OpenSSH connections.

> **Security Note:** Private SSH keys, passphrases, and passwords must never be stored in this repository.