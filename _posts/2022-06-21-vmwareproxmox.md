---
title: The BIG Migration from Vmware to Proxmox
author: Brandon Waite
date: 2022-06-21 00:00:00 +0800
categories: [Homelab, Tutorial]
tags: [Vmware, Proxmox]
---


# Vmware to Proxmox The Big Migration 

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/dino.jpg" alt="dino"/>
</div>

While Vmware is really good, for my homelab it was getting to be an effort. I used Vmwares Vmug program to test out differnt vmware products like NSX, vRA as we are a big VMware shop at work and I wanted to test out building and breaking things without the vmware admins getting mad at me. I set out to automate and deploy Windows and linux servers with a front end website to request builds. This worked great for my homelab for a good 5 years but it was getting old paying the subscription fee for vmug and having to find specific hardware or modify the boot config to deploy ESXI on hardware I had around the house.

## In comes Proxmox

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/proxmox.png" alt="proxmox"/>
</div>


>Links
{: .prompt-info }

[Vmware](https://www.vmware.com/)

[Proxmox](https://www.proxmox.com//)


