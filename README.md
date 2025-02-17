# Cloudflare Tunnel Setup Guide

## Overview
This guide provides instructions for setting up Cloudflare Tunnel, configuring DNS routes, and running services securely.

## Prerequisites
- A Cloudflare account
- A Linux-based server
- `sudo` privileges

---

## Installation
### 1. Install Cloudflared
```sh
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloudflare-main.gpg

echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflared.list

sudo apt-get update && sudo apt-get install -y cloudflared
```

### 2. Authenticate Cloudflared
```sh
cloudflared tunnel login
```
Follow the authentication steps in your browser.

### 3. Create a Tunnel
```sh
cloudflared tunnel create <TUNNEL_NAME>
```

### 4. List Available Tunnels
```sh
cloudflared tunnel list
```

---

## Configuring Cloudflare Tunnel
### 1. Configure Tunnel DNS Routing
```sh
cloudflared tunnel route dns <TUNNEL_NAME> <YOUR_DOMAIN>
```

### 2. Configure Ingress Rules
Edit the configuration file:
```sh
nano ~/.cloudflared/config.yml
```
Example configuration:
```yaml
tunnel: <TUNNEL_ID>
credentials-file: /root/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: example.com
    service: http://localhost:8080
  - service: http_status:404
```
Save and exit.

### 3. Run the Tunnel
```sh
cloudflared tunnel run <TUNNEL_NAME>
```

### 4. Enable Cloudflared Service
```sh
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

---

## Conclusion
This guide provides a streamlined process for setting up a secure Cloudflare Tunnel and configuring DNS routes. Customize configurations as needed for your specific use case.

For more details, refer to the [Cloudflare Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/).

