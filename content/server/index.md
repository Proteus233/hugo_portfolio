---
title: "Homelab Infrastructure"
date: 2026-04-03
description: "A full self-hosted homelab running Proxmox, TrueNAS, Docker, and more on repurposed hardware."
tags: ["proxmox", "truenas", "docker", "linux", "self-hosting"]
---

## Overview

My homelab is built around a secondhand **HP EliteDesk SFF G3** (i5-7500) running **Proxmox VE** as the hypervisor. This server has tyaught me the basics of virtualitzation.

---

## Architecture

```
HP EliteDesk (Proxmox)
├── TrueNAS VM
│   ├── WD HDD × 2 (passthrough)
│   └── SMB shares → LAN + Tailscale
├── Home Assitant VM
│   └── Zigbee2MQTT
├── Dockge (Docker)
│   ├── Immich
│   ├── Jellyfin
│   └── OpenSpeedServer
├── Debian LXC (Docker)
│   └── Nginx running this website
└── Adguard Home LXC (dns server)

```

---

## Key Components

## Remote connection
I use **tailscale exit node** on a debian lxc so I can remote into my home LAN.  
### 🔐 Tailscale (Exit Node Setup)

I use **Tailscale** to securely access my homelab from anywhere.

- Runs inside a **Debian LXC container**
- Configured as an **exit node**, allowing me to:
  - Route all my traffic through my home network
  - Access local services as if I were physically at home
- Avoids the need for:
  - Port forwarding
  - Exposing services directly to the internet

This setup gives me a **VPN-like experience**, but simpler and more reliable.

---