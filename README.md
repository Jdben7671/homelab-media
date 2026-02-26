# Homelab Project: Virtualized Media Infrastructure

## 1. Project Overview

This project documents the design and implementation of a virtualized Docker-based media stack running on a Proxmox hypervisor.

The goal of this project was to:
- Learn VM infrastructure design
- Practice Linux server administration
- Implement disk passthrough and filesystem merging
- Deploy and manage containerized services
- Secure remote access using Tailscale
- Build a reliable self-hosted service architecture

---

## 2. Infrastructure Overview

### Hardware

- HP ProDesk 600 G4 Mini
  - Intel i3 8th Gen
  - 16GB DDR4 RAM
  - 1TB Internal HDD
  - 1TB External USB HDD (passed through to VM)

### Hypervisor

- Proxmox VE

### Virtual Machine

- Ubuntu Server (LTS)
- 4 vCPU
- 8 GB RAM
- Disk passthrough enabled for external HDD

---

## 3. Storage Architecture

### Disk Layout

- Internal virtual disk (Proxmox storage)
- External 1TB physical disk passed directly to VM

### Filesystem Strategy

Used `mergerfs` to merge:
- Internal disk
- External disk

Into a unified mount point for:
- Media
- Downloads

Benefits:
- Single mount point
- Simplified container volume mapping
- Storage flexibility

---

## 4. Containerized Services

Docker installed on Ubuntu Server VM.

Services deployed via Portainer:

- Gluetun
- Jellyfin
- Sonarr
- Radarr
- Prowlarr
- qBittorrent
- Jellyseer

All services are:
- Running on a single docker-compose file
- Run through Gluetun for additional security
- Volume mapped to merged storage
- Isolated within Docker
- Accessible via Tailscale (no port forwarding)

---

## 5. Networking

- Static IP assigned to VM
- No public port forwarding
- Secure remote access via Tailscale
- DNS handled separately via Raspberry Pi (AdGuard)

---

## 6. Security Considerations

- No exposed WAN ports
- Access controlled through Tailscale ACL
- Services bound internally
- Minimal attack surface

---

## 7. Challenges & Solutions

### 1. USB Disk Passthrough in Proxmox
Solution:
- Direct hardware passthrough to VM

### 2. Merging Physical + Virtual Storage
Solution:
- mergerfs configuration with proper permissions

### 3. Container Volume Permissions
Solution:
- UID/GID alignment across services

---

## 8. Future Improvements

- Implement VLAN segmentation
- Add reverse proxy with HTTPS
- Centralized logging (Grafana/Prometheus)
- Automated backups
- Infrastructure-as-Code approach

