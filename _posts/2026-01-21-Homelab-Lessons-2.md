---
title: Homelab Lessons 2 - My Little Kingdom
description: Reviewing my current homelab's hardware and software, and the hard-earned experience that drove those decisions
categories: [Homelab]
tags: [homelab, self-hosting, servers, networking, docker, truenas]
mermaid: true
---

## Introduction

In the previous post, I walked through the evolutionary journey from a simple file server to a full homelab. Now let's take a look at what that homelab actually looks like today and why.

## The Architecture

Here's a high-level view of my current homelab architecture:

```mermaid
architecture-beta
    group wan(cloud)[WAN]
    service cloudflare(cloud)[Cloudflare] in wan
    service vpn(cloud)[VPN] in wan
    service internet(internet)[Internet] in wan
    service modem(server)[Fiber Modem] in wan

    group network(cloud)[Network]
    service ucg(internet)[UCG Gateway] in network
    service poe_switch(server)[PoE Switch] in network
    service tengig_switch(server)[10G Switch] in network
    service ap1(server)[WiFi AP 1] in network
    service ap2(server)[WiFi AP 2] in network

    group highspeed(server)[10G Devices]
    service monolith(server)[Monolith] in highspeed
    service compute(server)[Compute] in highspeed
    service workstation(server)[Workstation] in highspeed

    group clients(server)[Clients]
    service gaming_pc(server)[Gaming PC] in clients
    service family_pc(server)[Family PC] in clients
    service shield(server)[Shield TV] in clients
    service kodi_pi(server)[Kodi Pi] in clients
    service ha_pi(server)[HA Pi] in clients

    group monolith_svc(cloud)[Monolith Services]
    service storage(disk)[60TB Storage] in monolith_svc
    service jellyfin(database)[Jellyfin] in monolith_svc
    service navidrome(database)[Navidrome] in monolith_svc
    service nextcloud(cloud)[NextCloud] in monolith_svc
    service minecraft(server)[Minecraft] in monolith_svc
    service bluemap(server)[BlueMap] in monolith_svc

    group compute_svc(cloud)[Compute Services]
    service gpus(server)[RTX 8000 GPUs] in compute_svc
    service ollama(server)[Ollama] in compute_svc
    service comfyui(server)[ComfyUI] in compute_svc

    group ha_svc(cloud)[Home Automation]
    service mqtt(database)[MQTT] in ha_svc
    service matter(database)[Matter] in ha_svc
    service ha(database)[Home Assistant] in ha_svc

    vpn:R -- L:ucg
    cloudflare:R -- L:internet
    internet:R -- L:modem
    modem:R -- L:ucg
    ucg:R -- L:poe_switch
    poe_switch:R -- L:tengig_switch
    poe_switch:B -- T:ap1
    poe_switch:B -- T:ap2
    poe_switch:B -- T:gaming_pc
    poe_switch:B -- T:family_pc
    poe_switch:B -- T:shield
    poe_switch:B -- T:kodi_pi
    poe_switch:B -- T:ha_pi
    tengig_switch:B -- T:monolith
    tengig_switch:B -- T:compute
    tengig_switch:B -- T:workstation
    monolith:R -- L:storage
    monolith:R -- L:jellyfin
    monolith:R -- L:navidrome
    monolith:R -- L:nextcloud
    monolith:R -- L:minecraft
    monolith:R -- L:bluemap
    compute:R -- L:gpus
    compute:R -- L:ollama
    compute:R -- L:comfyui
    ha_pi:R -- L:mqtt
    ha_pi:R -- L:matter
    ha_pi:R -- L:ha
```

## The Monolith

## The Compute Servers

## The Workstation

## The Automation Pi

## Networking Infrastructure
