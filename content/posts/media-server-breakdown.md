+++
date = '2025-04-08T23:51:11+09:00'
title = 'Media Server Breakdown'
description = 'an overview into how i run a media server'
+++

## The Basics

Starting from the point-of-view of a Plex user, I'm going to go through how each step of the process of requesting media to streaming and how it works.

When you watchlist something on Plex, a program called Overseerr picks the request up. Media (movies and TV shows) all have IMDB IDs, which are unique identifiers for each piece of media assigned by IMDB. Overseerr uses these IDs to find the media on other services I'm running called Radarr (for movies) and Sonarr (for TV shows). These services are responsible for downloading the media using either torrents or through a protocol called Usenet.

In order to actually find the media, it has to search indexers. Indexers connect to torrent websites and usenet servers to find the media, and rate each file based on scoring rules. This prevents a file that's 480p being downloaded over a full HD file.

Two programs handle downloading media -- qBittorrent, and SABnzbd. When files finish downloading in qBittorrent, they're seeded to a ratio of 2.0, which means I've uploaded it twice for every time I've downloaded it. When they're downloaded through SABnzbd, it arrives to my server as a series of zip files, and SABnzbd will unpack and combine them to produce a single video file.

Radarr and Sonarr handle taking the downloaded file from the downloads folder and placing it into the media folders that Plex recognizes. They also do additional metadata tagging (such as episode title, release date, etc.).

Once the whole download process is completed, Sonarr and Radarr tell Plex to scan the media folder for new content.

## Requesting specific/partial media

Overseerr has a feature that allows you to request specific media. This is useful if you want to watch a specific episode of a show, or a specific version of a movie (like the extended cut). When you go to the Overseerr request website [overseerr.vrrdnt.co](https://overseerr.vrrdnt.co) ( <— tap this link to go to Overseerr) and search for a show, you can request a specific season or episode -- this is useful for avoiding downloading the entirety of a show when you only want the latest season or something like that.

Make sure you don't have the show you're partially requesting on your Plex watchlist -- Overseerr will interpret this as a request for the entire show.

## Reporting problems with media

Both Plex and Overseer have ways you can report issues with downloaded media.

In Plex, you can look for a button that looks like three dots next to the media you're watching. Click on it, and you should see a button that says "Report Issues". This will take you to a page where you can describe any problems with the media, such as missing subtitles or bad audio.

In Overseerr, it's a similar process. Go to the media, and you'll see a yellow hazard button on the screen. Click it, and a small window will open that will let you describe the issue in the same way as Plex.

#### Notes

```
I think this is everything -- I'd like to write a more technical description of how the server works and include images to things I talk about here, which I'll do soon.
identifiers you think anything should be added or explained, please let me know!
```
