+++
date = '2025-04-08T23:51:11+09:00'
title = 'Media Server Breakdown'
description: "An architectural deep dive into the inner workings of a self-hosted, automated media server stack powered by Plex and the *arr family of tools."
+++

If you've ever wondered how an automated media server works behind the scenes, this post breaks down the full architecture of a remote **Plex server** powered by the *arr ecosystem.

## The Big Picture

At its core, a remote Plex server setup is a **pipeline** that begins with content discovery and ends with media streaming. Think of it like a digital supply chain:

1. **Discovery** – Find new content.
2. **Acquisition** – Download the content.
3. **Processing** – Organize and clean up the files.
4. **Delivery** – Serve it to users via Plex.

Each part of this chain is handled by a specialized service.

---

## Components & Their Roles

### **Plex Media Server**
Plex is the front-end. It’s what users interact with to stream their media. It maintains a database of metadata, watch history, posters, and handles transcoding when needed. It has no awareness of how media arrives — it just watches folders.

### ***arr Applications**
This is the automation brain. Each tool in the *arr stack is a scheduler, organizer, and metadata handler for a specific media type:

- **Sonarr** – Automates TV shows.
- **Radarr** – Automates movies.
- **Lidarr** – Handles music.
- **Bazarr** – Fetches subtitles.

These tools regularly check for missing or upcoming content, using your filters (e.g. 1080p only, English audio, etc.), and pass search requests to your indexers via **Jackett** or **Prowlarr**.

### **Jackett / Prowlarr**
These are indexer proxies. They translate *arr requests into something torrent and usenet indexers understand. Think of them like multi-lingual brokers that speak both Sonarr and BitTorrent fluently.

- **Jackett** is older and widely used for torrent indexers.
- **Prowlarr** is newer, designed as a unified indexer manager for all *arrs.

### **Download Clients**
Once a release is found, it's passed to your download client:

- **qBittorrent**, **Deluge**, or **Transmission** (for torrents)
- **NZBGet** or **SABnzbd** (for Usenet)

These tools download the media to a specified directory.

### **File Handling & Renaming**
Once downloaded, *arr tools move the files, rename them according to your naming convention, and place them in Plex’s monitored folders. Plex then scans the new content and adds it to the library.

No manual dragging, renaming, or tagging. The entire flow is event-driven.

---

## Remote Architecture Considerations

When hosted remotely (on a VPS, dedicated server, or cloud box), there are some architectural quirks:

- **Shared Storage** – All containers or apps must share access to the same volume (e.g. `/media/`).
- **Persistent Volumes** – Docker volumes or bind mounts are used so data persists across container updates.
- **Reverse Proxy** – Nginx or Traefik routes traffic to web UIs securely (e.g. Sonarr at `sonarr.yourdomain.com`).
- **VPN/Privacy** – Download clients are often routed through a VPN container (e.g. `gluetun`) to avoid exposing your IP.
- **Remote Plex Access** – Port forwarding or Plex Relay enables you to stream from anywhere.

---

## Diagram (Text-Based)
[Sonarr/Radarr/Lidarr]
|
v
[Jackett/Prowlarr]
|
v
[qBittorrent/NZBGet/SABnzbd]
|
v
[Media Files]
|
v
[Plex]
|
v
[End User Streaming]

Each arrow represents an API call, file operation, or polling loop. The key is automation: once configured, this entire loop runs without human input.

---

## Conclusion

A remote Plex server with the *arr stack is a beautiful example of composable, event-driven architecture. Each tool does one thing well, and when orchestrated together — usually via Docker — they form an intelligent, self-updating content delivery system.

It’s not just a media server. It’s your personal Netflix, powered by APIs and containers.

