---
title: "Homelab Journey Part 1: The Road To Homelab"
description: A retrospective on the bumpy road from a simple file server to a true homelab, the mistakes made and lessons learned along the way
categories: [Homelab]
tags: [homelab, self-hosting, servers, raspberry-pi, truenas, jellyfin, home-assistant]
mermaid: false
---

## Introduction

For the past few years I have been going from one solution to another, trying to provide infrastructure for my growing media collection, cord-cutting DIY media streaming, home automation, and general development. At this point I have fallen solidly into the homelab rabbit hole and now that I have my footing it is a good time to reflect on what I've learned from all the frustration.

## What's a Homelab?

In case you haven't the jargon before, "homelab" refers to a personal computing environment, typically servers, networking equipment, and all the associated infrastructure, that enthusiasts run at home for learning, experimentation, and self-hosting services. When I first heard people talking about homelabs, I imagined something like my home electronics lab: oscilloscopes, soldering stations, breadboards full of half-finished projects. I was a bit disappointed to discover it was "just" servers and network switches. It'd be a few years before I circled around to seeing the complexity and value in taking it seriously. A homelab can be as simple as a Raspberry Pi running a few Docker containers or as elaborate as a rack full of enterprise servers with redundant networking. The gist it's yours to experiment with, break, and learn from.

## How Did We Get Here?

I suspect the journey I've taken is fairly typical, and you'll probably find yourself someone along it or else find me treading the same ground you've already passed. I'll divide the route into an evolving sequence of eras, with the pros and cons of each.

- **Just my PC:** In the beginning I just had my desktop PC and that was enough. I had Windows file sharing, and I even installed FileZilla FTP server. Any other services I wanted to run I'd just run on my desktop and I set up my router to forward ALL ports to it.
    - **Pros:** ...few. I suppose it was convenient to set things up, but frankly I just didn't know any better.
    - **Cons:** Where to start? Exposing my desktop to the Internet with who-knows-what running on it is a security nightmare, and eventually it WAS breached by someone who installed a Monero miner. And of course any service I wanted to host would die whenever my PC was turned off, and would choke when my PC was occupied with something. I would struggle to play Minecraft while also hosting a Minecraft server for my friends.

- **A Second PC:** To address some of the issues, I set up a basic HP mini desktop to serve things. It still ran Windows, but it was quieter and freed up my PC.
    - **Pros:** Not worrying about the services going down when my PC was heavily loaded or just plain crashed was liberating. I could leave the Minecraft server running 24/7.
    - **Cons:** It really was still a Windows desktop with ports forwarded to it. Worse, I even forwarded RDP to it so I can remotely access it from college when it was running at my parents' house.

- **A USB External Drive:** At some point, dealing with a second PC was just too much bother and the biggest drive I had was entombed in a USB enclosure. I copied all the media I had onto it and would just bring it around to plug into things as needed. Plug into my PC, my laptop, etc.

    - **Pros:** Convenience was a lot higher, in that I didn't have a live system to maintain or secure.
    - **Cons:** The inevitable happened when that HDD took too much rough handling: The drive hit a read error and had to be reformated, costing us our whole media collection.

- **A Raspberry Pi:** Once the USB HDD was reformatted and healthy again and I started rebuilding my media collection, I wanted to avoid moving the HDD around anymore. Setting up a whole second PC wasn't a great option at the time, so leaving a spare Windows box running wouldn't suffice. Raspberry Pi was all the rage and I had a couple to play with so I decided to install [OpenMediaVault](https://www.openmediavault.org/) on one and plug the HDD into it to function as a file server. You might say this was my first step into the homelab territory.
    - **Pros:** Compared to a USB drive, it was more convenient because multiple systems could connect to it at once. I port forwarded to the Pi so I could access files on the go too, and in retrospect that was safer than my previous approaches because I only forwarded the port for FTP. The OpenMediaVault ecosystem was my first exposure to containers which would remain forbidden magic to me for a long time, but the convenience was indisputable.
    - **Cons:** A first-generation Raspberry Pi wasn't exactly a compute powerhouse, and a HDD through USB 2.0 isn't the fastest thing. The Pi was often overloaded and overheated, and SD card corruption resulted in me rebuilding the system multiple times.

- **Multiple Raspberry Pis:** While I COULD install multiple services on a single Pi, it wasn't very practical given the performance issues I was already facing. Instead, I got a couple more Pis. One went next to the fileserver Pi and served [Mosquitto MQTT](https://mosquitto.org/) and [Home Assistant](https://www.home-assistant.io/) to support my DIY home automation projects, as well as a few microservices I wrote for hobby projects. Two more Pis were configured with [LibreELEC](https://libreelec.tv/) for [Kodi](https://kodi.tv/) and set up behind TVs to stream media from the fileserver Pi.
    - **Pros:** Dividing up the responsibility between devices helped reduce the impact of those performance issues. The Pi serving MQTT and Home Assistant never had any stability issues because of the lighter workloads it ran, which was great since those services needed high uptime.
    - **Cons:** The fileserver Pi struggled to move files fast enough, the wifi wasn't really fast enough for high-bitrate videos and the Pis running Kodi weren't really fast enough to decode them so I took to transcoding videos down to 720p. It was a step in the right direction but I clearly needed something beefier.

- **An Off-The-Shelf NAS:** A big shift happened when I spotted a neighbor selling a barely-used Synology Diskstation. Two HDDs, a web interface, and a bespoke system that ran like butter. Better, the little SoC inside it had headroom to spare for additional applications, all easily installed as containers.
    - **Pros:** This sucker ran smooth as butter. None of the performance or stability issues I'd faced with the Pi running OMV. Interacting with the system via a web GUI instead of just SSH was much more convenient, and being able to install applications as curated containers that _just worked_ was wonderful.
    - **Cons:** Synology. I won't lie, the web UI is one of the best experiences I've had with software in a long time. Unfortunately it wasn't the most open platform ever and newer Synology products are even more restricted. I ran into the limitations of the NAS' performance and had no way to upgrade that, so for me it was a dead end.

- **A Second PC Again, But Better:** Frustrated with the performance demands of HD video, plus the issues my family had with trying to stream HD video over wifi to their phones, I wanted a way to decrease the burden on the client devices and resolve the networking bottleneck. [Jellyfin](https://jellyfin.org/) offered one solution: transcode the videos server-side so it only sends as much data as needed or as much as the network connection will support. Of course, a Pi won't be able to provide much better performance this way and my Synology NAS wasn't compatible with it, but if I set up a PC as a single big server then I should be able to store and run everything on a single box. I ended up running [TrueNAS](https://www.truenas.com/) and went nuts installing everything I ever wanted from curated images.
    - **Pros:** TrueNAS definitely makes things easy. Not as smooth as Synology's Diskstation UI, but still a lot more convenient than raw SSH. The dashboard automates a lot of things I wasn't very familiar with yet, like ZFS pools, datasets, ACL management, and containers. Running on a real PC with a Xeon CPU and 128GB of RAM, plus whole stacks of HDDs and SSDs, meant serving files was no sweat and I could even run a few other services without issues. In theory.
    - **Cons:** In practice, a single server running everything wasn't an ideal solution. Even a 24-core Xeon has limits and my excited plunge into containerized services meant the CPU was under steady load by the time I set up the 10th service or so. Worse, while containers aren't supposed to conflict with each other there were situations where they'd compete for resources. The Minecraft server would start to lag badly when a video was being transcoded by Jellyfin, for example. Running heavy local AI applications or other compute workloads were out of the question unless I made sure the Minecraft, Jellyfin, and other services weren't in use first, which is less than ideal. Lastly, I needed to install a specific extension for Home Assistant that was not compatible with the containerized version, and I wasn't keen on trying to install Home Assistant directly in the server or running a full-fat VM just for that.

- **Multiple Servers:** The solution was simple enough: Divide the workloads across multiple servers built up to suit different cases. At the time of this writing, I have a fileserver crammed with disks, a compute server crammed with GPUs, and a Raspberry Pi running just Home Assistant and Mosquitto MQTT. Of course, spreading things around meant needing to upgrade the LAN between them so there's a 10Gb/s Ethernet switch for the file server and compute server.
    - **Pros:**  With each system targeting a different set of needs, I don't need to compromise as much. The file server has a slower CPU with tons of cores and the compute server has a faster CPU with fewer cores. Compute-heavy workloads go on the compute server where there's less base load and fewer high-uptime applications to risk fouling up.
    - **Cons:** Enough that when I finally got it under control I decided it was worth writing multiple blog posts about.

This is, of course, a very simplified retrospective. In between all of these eras I experimented with different OSes, different distros, PXE booting, reinventing the wheel with bespoke solutions, many, many hardware swaps and reinstallations and reboots. I learned the hard way that the security principles I learned in college mattered in my home network as much as anywhere else, and that there are desktop vs. server versions of OSes for a reason.

Next up, we'll take a look at what I've built so far and why.
