# Nginx Web Server

## Overview

Nginx was installed on `debian-server-01` as the first additional network service in the homelab.

The goal of this setup is to learn the basic operation and administration of a web server, including:

- Installing and managing a Linux service
- Understanding HTTP and TCP port 80
- Configuring firewall rules with UFW
- Understanding the web root directory
- Hosting a custom HTML page
- Testing access from another device on the local network

## Environment

- Server: `debian-server-01`
- Operating System: Debian 13
- Web Server: Nginx
- Server IPv4: `192.168.178.20`
- Protocol: HTTP
- TCP Port: `80`

## Installation

Nginx was installed using the Debian package manager:

```bash
sudo apt install nginx
```

After installation, the service status was verified:

```bash
systemctl is-active nginx
systemctl is-enabled nginx
```

The results were:

- `active` — Nginx is currently running
- `enabled` — Nginx will start automatically when the server boots

The listening network ports were also checked using:

```bash
sudo ss -tulpn
```

Nginx was confirmed to be listening on TCP port `80`.
## Firewall Configuration

Before allowing HTTP traffic, UFW was configured to deny incoming connections by default.

The available UFW application profiles were checked using:

```bash
sudo ufw app list
```

Nginx provides several firewall profiles. For the current HTTP-only configuration, the `Nginx HTTP` profile was selected.

HTTP traffic was allowed with:

```bash
sudo ufw allow 'Nginx HTTP'
```

The firewall configuration was then verified:

```bash
sudo ufw status
```

The firewall now allows:

- OpenSSH — TCP port `22`
- Nginx HTTP — TCP port `80`

## Web Root

The default Nginx web root on this Debian installation is:

```text
/var/www/html/
```

The contents of the directory were inspected using:

```bash
ls -la /var/www/html/
```

The default Nginx page was stored as:

```text
/var/www/html/index.nginx-debian.html
```

## Custom Web Page

A custom HTML page was created:

```text
/var/www/html/index.html
```

The file was edited using Nano:

```bash
sudo nano /var/www/html/index.html
```

The page contains basic information about the homelab server, including its hostname and IPv4 address.

The saved file was verified using:

```bash
cat /var/www/html/index.html
```

## Testing

The web server was tested from another computer on the local network by opening the server IPv4 address in a web browser:

```text
http://192.168.178.20
```

The custom homelab page was successfully returned by Nginx.

This confirms that:

1. The Debian server is reachable over the local network.
2. UFW allows incoming HTTP traffic on TCP port `80`.
3. Nginx is running and listening for HTTP requests.
4. Nginx can serve files from `/var/www/html/`.
5. A client on the network can access the hosted website.

## Current Status

The Nginx web server is operational and accessible on the local network over HTTP.

HTTPS/TLS has not yet been configured. Therefore, browser connections currently use unencrypted HTTP on TCP port `80`.

Further Nginx configuration and HTTPS will be explored later.

# Nginx Troubleshooting

## Configuration Validation

Before applying changes to the Nginx configuration, the configuration should always be tested.

    sudo nginx -t

A successful test returns:

    syntax is ok
    test is successful

During troubleshooting, a missing semicolon in `nginx.conf` caused the configuration test to fail.

Although Nginx was still running and the website was reachable, the configuration file on disk was invalid.

This demonstrates an important principle:

> A running service does not necessarily mean that its current configuration file is valid.

## Troubleshooting Workflow

A safe workflow for Nginx configuration changes is:

1. **Edit** the configuration.
2. **Validate** the configuration with `nginx -t`.
3. **Apply** the configuration with a reload.
4. **Verify** that the service is working correctly.

Commands:

    sudo nginx -t
    sudo systemctl reload nginx
    systemctl status nginx

The listening ports can also be verified with:

    sudo ss -tulpn

Nginx should be listening on TCP port 80 for HTTP.

Service logs can be inspected with:

    sudo journalctl -u nginx

## Key Takeaway

Always validate configuration changes before reloading a service.

**Workflow:**

`Edit → Validate → Apply → Verify`