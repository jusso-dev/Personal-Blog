---
template: BlogPost
path: /homelabs
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

By using a Intel BXNUC10I7FNH NUC I am able to run VMWare ESXi 7.0 in a 'bare-metal' configuration, which allows me to have greater control and viability over my entire fleet. Plus let's be honest, if ESXi is good enough for most medium to large enterprise, it's good enough for my Homelab.

As for my internet, I have a gigabit fiber link directly to my home, which is complemented by the [Unifi Dream Machine](https://store.ui.com/collections/unifi-network-routing-switching/products/unifi-dream-machine). The UDM provides my home wireless routing of internet, Network Intrusion Protection (NIPS), host-based discovery and scanning, ISP speed checks, and much more.

Having enough memory in a Homelab quickly became apparent, and also a bottleneck in most cases, as most of my workloads usually only require 1-2 vCPUs, but generally require 4+ GB of memory. I will probably either add another Intel NUC later on down the track, or simply add another 32 GB of memory.

Shiny hardware is nice and all, but you probably came here to see what I am running on this kit, so let's discuss what workloads I am currently running.

# Workloads

## Containers

For my containerisation capability, I decided to go with [Portainer](https://www.portainer.io/). Portainer is a light-weight GUI, that let's you deploy docker and docker-stack based images, and control all of this using a nice looking, easy to navigate web UI. I initially opted to use Rancher within my environment, but due to running Rancher in a single node cluster, I quickly found it to be unstable and unusable, which is my own fault because if I had done my research prior to standing Rancher up I would have known that Rancher is better suited to multi-node clusters.

## VM based workloads

My entire Homelab fleet consist of Ubuntu 20.04 LTS machines. Initially I planned on using Fedora or CentOS but Ubuntu seemed like a good fit, was secure (for the most part) out of the box and was feature rich.

As for specific VM based workloads, I am running the following:

* VMWare Harbor as a local private container repository:

![harbor-image](/assets/Harbor.jpg "VMware Harbor private container image repository ")

* Nessus Essentials

  Whether you're running a Homelab or an enterprise fleet of servers, you should not overlook security. Nessus provides me the ability to automatically scan my entire fleet of Unix based hosts, and report on any vulnerabilities it finds, as well as any hidden malware. More on this below.

![Nessus](/assets/Nessus Essentials.jpg "Nessus Essentials Vulnerability Scanning")

* Nextcloud 

  If you haven't checked out [Nextcloud ](https://nextcloud.com/)before, you should. It provides features such as a locally hosted cloud file storage system, which offers apps for IOS and Android, and provides other features such as auto-sync of files from devices, multiple user accounts, calendar etc.

![](/assets/Nextcloud.jpg)

* Portainer

  Portainer provides a nice Web UI to manage all of my containerised workloads, from Homebridge, to MeiliSearch, to bespoke applications I hosted locally for various automation needs.

![Portainer-image](/assets/Portainer.jpg "Portainer docker image GUI/Web UI")

## Vulnerability scanning

As touched on above, security should not be overlooked even in the context of a Homelab environment. Nessus Essentials is free to use, and provides the ability to perform host discovery, scan for malware on various guest host operating systems, scan web applications in the environment, and much much more.

## Automation

For automation in my Homelab environment, from patching various applications, to installing software on newly created hosts, I have opted to use Red Hat [Ansible](https://www.ansible.com/). Ansible provides me the capability to perform desired state configuration, ensuring my fleet remains up-to date, patched, secure and also have all the required packages and software that I require when provisioning new hosts. In the future I am looking into further automating this and enabling scheduling of Ansible scripts, I am to achieve this initially with [Rundeck](https://www.rundeck.com/open-source).

## DNS and ad-blocking

If you have aimed to block ads at the DNS level before, you probably have already guessed what I am using, but for the rest of you, I am using [Pihole ](https://pi-hole.net/)to block ads for my entire home at the DNS level, both LAN and WAN. Pihole is fantastic at swallowing up traffic that matches a preconfigured wordlist that contains well known sites that serve ads. Not only can ads sometimes bother us when we are attempting to browse to our favorite sites, they can sometimes be malicious and serve unwanted content such as JavaScript payloads that can perform all sorts of nasty actions. Using Pihole is extremely helpful in a Homelab environment, or really in general as it has an in-built DNS feature, that allows Pihole to act as your DNS server. I use Pihole as my primary DNS, failing over to Cloudflare if required. When you start to provision more than 10 hosts in your environment, there comes a point where remembering 10 sets of IP addresses for all workloads becomes impossible, Pihole lets you configure a DNS hostname, so you can have human-readable URL's to remember instead.

# Why has running a Homelab taught me so much

Ever since I started writing code around 5 years ago, I have always been frustrated when I could not understand how a certain library, framework or piece of code worked. This frustration had always led me to going home after work and provisioning these solutions locally on my laptop and using them until I fully understood the solution.

Having a Homelab environment now has taught me so much just from general exposure to the latest and greatest tooling, frameworks, security tools and much more. The freedom and ability to keep up-to date with the latest and greatest has allowed me to progress in my career and be on the front foot when new solutions are proposed at work.

# Where to next?

As for what's next, I plan to fully automate my day-to-day operations, by using Ansible as discussed above, but configuring Ansible playbooks to run on a pre-configured schedule. I also intend to run more Nessus scans in an automated fashion, which will require some bespoke code to invoke Nessus API's on a schedule (as I have exhausted my limit of 1 on the Essentials free plan). I also intend on adding more logging, for this I intend to use the ELK stack and the recent [security features of Elastic](https://www.elastic.co/security).
