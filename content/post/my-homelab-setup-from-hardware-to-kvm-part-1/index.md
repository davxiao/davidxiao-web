---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "My Homelab Setup From Hardware to KVM - Part 1"
subtitle: "Hardware"
summary: "I plan on writing a few posts about my Homelab project. This is the first one focusing on the hardware spec and networking"
profile: false
authors:
  - david-xiao
categories:
  - Homelab
tags:
  - kvm
  - homelab
  - amd ryzen
  - kvm
  - proxmox
  - pve
date: 2020-05-28
lastmod: 2020-05-28
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/-flQuOWF18Y)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

***[TL;DR](https://en.wikipedia.org/wiki/Wikipedia:Too_long;_didn%27t_read)***

*I plan on writing a few posts about my Homelab project. This is the first one focusing on the hardware spec and networking.*

---

Last year I decided to set up a homelab to learn technologies, to run some services for my own use and more importantly, to have fun.

There is a budget and a few high level requirements on the infrastructure side I worked on:

- Be able to run a hypervisor that supports both Linux and Windows virtual machine

- One single host. No clustering
- Use open source as long as the it's mature enough
- Easy to manage and operate
- Cost effective
- No dedicated networking hardware

I plan on writing a few posts with respect to the HW spec I chose, the architecture decisions I made along the way and how I built the whole lab. This is the first post.

## Hardware

<style>
table th:nth-of-type(1){
    width: 20%;
}
table th:nth-of-type(2) {
    width: 30%;
}
table th:nth-of-type(3) {
    width: 50%;
}
</style>

| Item   |     Spec      |  Notes |
| --- | --- | --- |
| CPU         | AMD Ryzen 7 3700x     | 65W TDP. 8-core, 16-thread, 7nm        |
| Motherboard | ASUS PRIME X470-PRO   | See my [notes](#motherboard) |
| Memory | 16gb x2 ECC UDIMM | Unbuffered ECC |
| System Storage | 250GB SATA SSD | See my [notes](#storage) |
| Data Storage | 4TB HDD | As temporary data storage |
| Networking | On-board Gigabit Ethernet | See my [notes](#networking) |
| Graphics Card #1 | Asus GeForce GTX 1660 Super | model: TUF-GTX1660S-O6G-GAMING See my [notes](#gpu-passthrough) |
| Graphics Card #2 | MSI GeForce GT 710 1GB | Fanless design |

Accessories such as case and power supply are not listed but included in the total cost.

## Total Cost

The hardware cost of my homelab is around CA $1,400 when I bought them back in December 2019.

## Motherboard

If you need to use ECC memory, you have to pick a motherboard that support ECC. In consumer grade motherboard market, look no further than ASUS and ASROCK. Those two are know for building consumer grade ECC-enabled motherboards.

If you need to manage your homelab hypervisor from a remote location, and be able to access it when troubleshooting something low level, e.g. when hypervisor is crashed or configuring BIOS, you have two choices: Phyical Access or IPMI.

### Phyical Access

It means hook up a display, a keyboard and a mouse to your server and sit in front of it when troubleshooting.

### IPMI

With IPMI, tasks such as power on/off the machine, configure BIOS settings and monitor console output all can be done from remote wia a web portal. Motherboards that support IPMI typically provide a dedicated Ethernet port for IPMI management. Learn more about IPMI [here](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface).

ASROCK has some pretty good IPMI-enabled workstartion motherboards. For AMD cpu, take a look at ASROCK X470D4U or x470d4u2-2t with 10G networking. There are also x570d4i-2t to be released in 2020.

## Storage

To have flexability and speed, I need two kinds of storage. [Direct Attached Storage](https://en.wikipedia.org/wiki/Direct-attached_storage) (DAS) for hypervisor and guest OS images and [Network Attached Storage](https://en.wikipedia.org/wiki/Network-attached_storage) (NAS) for data such as video and photos.

### SSD as DAS

M2.SSD is my primary choice for DAS since it's much faster than SATA SSD both on paper and in reality. So I bought a [XPG SX8800 PRO](https://www.xpg.com/us/feature/637/) 1TB and installed it on my server.

Unfortunately my server started freezing up randomly every a few hours to a few days. Troubleshooting proved to be one hell of an effort when my server is in the basement without a dedicated display. I'd have to take an external display from my home office down to the basement for troubleshooting. IPMI could have saved my day.

{{< figure src="m2ssderror.png" title="Error messages showing I/O error on nvme0n1" >}}

It took some time to reproduce the problem and troubleshoot the issues. Finally I was able to conclude that the controller chip that XPG SX8800 PRO uses may have compatibility issue with PRIME X470-PRO. After I replaced the M2.SSD with an old SATA SSD that I have, the server runs smoothly without any issues.

### Synology NAS

I have a Synology DS416play. It's a little home NAS with 4 bays and 1GB memory. I've since upgraded the memory to 8G and have it fitted with 4x10G HDD. In terms of usable space it has around 27T with one disk fault tolerance which is more than enough for me.

The downside of this NAS is it only has Gigabit Ethernet. If you think 10G is appealing, I'm totally with you. QNAP is offering a number of 10G ready NAS. Check it out [here](https://www.qnap.com/solution/10gbe-ready/en-us/) for detail.

## Networking

My home network topology is rather simple. Both homlab server and NAS are connected to the internet router. The router provides basic functionalities such as IP management and port mapping.

{{< figure src="network.png" title="Home Network Diagram" >}}

Given my home network cables are all Cat5 supporting up to 1 Gigabit, I have to stay with 1G network and the on-board Gigabit Ethernet works well in my setup.

If you are considering 10G, you would need to make sure all critial devices on the network (e.g. router, NAS, Wifi etc.) are capable of dealing with the throughput, since any performance bottleneck can be the low watermark that undermines the overall network performance.

## GPU Passthrough

The KVM hypervisor supports passing graphics card(s) onto dedicated guest OS.

On my setup, GTX1660 is the primary card for tasks such as streaming encoding/decoding whereas the GT710 is pass-through to a Win10 guest. This setup allows me to play a few not-so-demanding games on the Win10 through remote desktop.

{{< figure src="playsan13.png" title="Playing [三國志XIII](https://store.steampowered.com/app/363150/ROMANCE_OF_THE_THREE_KINGDOMS_XIII/) on Win10 guest through Remote Desktop" >}}

I will discuss more on GPU passthrough in the next post.
