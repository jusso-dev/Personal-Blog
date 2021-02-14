---
template: BlogPost
path: '/why-homelabs-are-invaluable '
date: 2021-02-13T23:03:55.724Z
title: Why having your Homelab can be invaluable to your career progression
metaDescription: Homelab for learning and career progression
thumbnail: /assets/IMG_3128.JPG
---
# How it started

Last year, my wife and I purchased our first home. Prior to this we were renting for many years, and despite having the burning desire to setup a Homelab, I never got around to it. The idea of setting up server racks, caballing, configuring the server etc, every 12 months, in a new house, quickly put me off the idea all together.

Once we had settled into our new home I knew this was the chance I hadn't had prior to owning a house, to have a Homelab setup. I knew this had to consist of the following a minimum starting point:

* Small server that was power efficient, but also afforded me to the freedom to configure and play with most enterprise workloads and tooling
* Quite
* Flexibility to run enterprise hosting virtualisation such as VMWare ESXi (more on that later)

I ultimately decided based on the above requirements, that a Intel NUC was the perfect fit, which brings me to hardware

# Hardware

For hardware, I decided to go with Intel BXNUC10I7FNH NUC Barebone Kit - i7 10th Gen, sporting 32 GB of DDR4 2666MHz Crucial memory, with a Samsung 970 EVO 1TB NVMe drive as my primary storage.

Having enough memory in a Homelab quickly became apparent, and also a bottleneck in most cases, as most of my workloads usually only require 1-2 vCPUs, but generally require 4+ GB of memory. 

By using a Intel BXNUC10I7FNH NUC I am able to run VMWare ESXi 7.0 in a 'bare-metal' configuration, which allows me to have greater control and viability over my entire fleet. Plus let's be honest, if ESXi is good enough for most medium to large enterprise, it's good enough for my Homelab.

As for my internet, I have a gigabit fiber link directly to my home, which is completed by the [Unifi Dream Machine](https://store.ui.com/collections/unifi-network-routing-switching/products/unifi-dream-machine). The UDM provides my home wireless routing of internet, Network Intrusion Protection (NIPS), host-based discovery and scanning, ISP speed checks, and much more.

Having shiny hardware is nice and all, but you probably came here to see what I am running on this kit, so let's discuss what workloads I am currently running.

# Workloads

## Containers

For my containerisation capability, I decided to go with [Portainer](https://www.portainer.io/). Portainer is a light-weight GUI, that let's you deploy docker and docker-stack based images, and control all of this using a nice looking, easy to navigate web UI. I initially opted to use Rancher within my environment, but due to running Rancher in a single node cluster, I quickly found it to be unstable and unusable, which is my own fault because if I had done my research prior to standing Rancher up I would have known that Rancher is better suited to multi-node clusters.

## VM based workloads

My entire Homelab fleet consist of Ubuntu 20.04 LTS machines. Initially I planned on using Fedora or CentOS but Ubuntu seemed like a good fit, was secure (for the most part) out of the box and was feature rich.

As for specific VM based workloads, I am running the following:

* VMWare Harbor as a local private container repository:

![harbor-image](/assets/Harbor.jpg "VMware Harbor private container image repository ")

* Nessus Essentials

  Whether you're running a Homelab or an enterprise fleet of servers, you should not overlook security. Nessus provides me the ability to automatically scan my entire fleet of unix based hosts, and report on any vulnerabilities it finds, as well as any hidden malware.

![Nessus](/assets/Nessus Essentials.jpg "Nessus Essentials Vulnerability Scanning")



* Nextcloud 

  If you haven't checked out [Nextcloud ](https://nextcloud.com/)before, you should. It provides features such as a locally hosted cloud file storage system, which offers apps for IOS and Android, and provides other features such as auto-sync of files from devices, multiple user accounts, calendar etc.

![](/assets/Nextcloud.jpg)

## Vulnerability scanning

## Automation

## DNS and ad-blocking

# Where to next?
