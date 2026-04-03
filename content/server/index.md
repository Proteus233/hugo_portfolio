---
title: "Homelab Infrastructure"
date: 2024-01-01
description: "A full self-hosted homelab running Proxmox, TrueNAS, Docker, and more on repurposed hardware."
tags: ["proxmox", "truenas", "docker", "linux", "self-hosting"]
---

## Overview

My homelab is built around a secondhand **HP EliteDesk SFF G3** (i5-7500) running **Proxmox VE** as the hypervisor. The goal is full data sovereignty — every service I use day-to-day runs on hardware I own and control.

---

## Architecture

```
HP EliteDesk (Proxmox)
├── TrueNAS VM
│   ├── WD HDD × 2 (passthrough)
│   └── SMB shares → LAN + Tailscale
├── Jellyfin LXC
├── Immich (Docker Compose)
│   ├── Postgres
│   ├── Redis
│   └── Machine Learning worker
└── AudioMuse-AI (Docker)
    ├── NVIDIA Container Toolkit
    └── GPU passthrough
```

---

## Key Components

### Proxmox VE
The backbone of everything. I manage LXC containers for lightweight services and full VMs for heavier workloads like TrueNAS. LVM thin pools handle storage, and `vzdump` handles regular backups.

**Highlights:**
- LXC containers for Jellyfin, custom scripts, and lightweight services
- TrueNAS running as a VM with raw disk passthrough (`/dev/disk/by-id/`)
- LVM thin pool management — extended data pool from unallocated VG space
- Backup/restore workflows with `vzdump` and Proxmox Backup Server

### TrueNAS
Running as a Proxmox guest with two WD HDDs passed through individually. Hosts all media and personal data via SMB shares, accessible locally and remotely over Tailscale.

**Highlights:**
- SMB shares with proper ACLs
- HDD standby timer via `hdparm -S` + udev persistence rule (APM not supported on these drives)
- Remote access via Tailscale for a direct peer-to-peer connection

### Immich
Self-hosted photo management — a full Google Photos replacement. Runs as a Docker Compose stack with Postgres, Redis, and a machine learning container.

**Highlights:**
- Resolved bind-mount path issues with `UPLOAD_LOCATION`
- Postgres container with runtime-generated config
- Machine learning container for facial recognition and smart search

### Jellyfin + AudioMuse-AI
Media server with an AI music analysis layer powered by AudioMuse-AI. AudioMuse runs with GPU passthrough via NVIDIA Container Toolkit on Fedora.

**Highlights:**
- Hardware-accelerated transcoding
- GPU passthrough with NVIDIA Container Toolkit
- Secondary remote worker with correct Redis/Postgres env wiring
- Gemini API integration for AI text features

### Game Streaming: Sunshine / Moonlight
Low-latency game streaming from my Linux desktop (NVIDIA RTX 3070) to a tablet. Configured to stream at the tablet's native resolution.

**Highlights:**
- Sub-30ms latency on local network
- NVIDIA RTX 3070 as the encode source
- Moonlight client on Android tablet

---

## Lessons Learned

- Always check for stale lock files before blaming hardware (`pct unlock`)
- `fsck -y` saves lives after unexpected shutdowns in LXC containers
- DNF5 and DNF4 are not the same — NVIDIA's docs haven't always caught up
- Blacklisting `nouveau` properly requires regenerating initramfs with `dracut --force`
- Secondhand hardware from Wallapop is great — just verify serial numbers and SMART data first

---

## What's Next

- Tailscale installed directly on TrueNAS for peer-to-peer remote NAS access
- Exploring Proxmox Backup Server for more robust scheduled backups
- Extending the homelab with additional compute for heavier workloads