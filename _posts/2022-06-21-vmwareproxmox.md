---
title: The BIG Migration from Vmware to Proxmox
author: Brandon Waite
date: 2022-06-21 00:00:00 +0800
categories: [Homelab, Tutorial]
tags: [Vmware, Proxmox]
---


# The Migration 

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/dino.jpg" alt="dino"/>
</div>


While Vmware was really good for my homelab it was getting to be an effort. I used Vmware's Vmug program to test out different vmware products like NSX, vRA, and ESXI products. We are a big VMware shop at work and I wanted to test out building and breaking things without the VMware Admins getting mad at me. I set out to automate and deploy Windows and linux servers with a front end website to request builds following up with something that could enforce configuration changes like puppet. This worked great for my homelab for a good 5 years but it was getting old paying the subscription fee for vmug and having to find specific hardware or modify the boot config to deploy ESXI on hardware I had around the house. We also had the solution deployed at work so the need to figure out how it all works together was done plus I was over being restricted around server hardware. Time for a switch....


## In comes Proxmox

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/proxmox.png" alt="proxmox"/>
</div>

So were off on a mission to migrate over to proxmox, I can't use the already running Vmware environment as I have servers running on both ESXI hosts and the memory is maxed out. Time to test out building a new server with hardware I wanted.

<a href="https://pcpartpicker.com/list/9bmtW4">PCPartPicker Part List</a>
<table class="pcpp-part-list">
  <thead>
    <tr>
      <th>Type</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="pcpp-part-list-type">CPU</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/KwLwrH/amd-ryzen-9-5900x-37-ghz-12-core-processor-100-100000061wof">AMD Ryzen 9 5900X 3.7 GHz 12-Core Processor</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">CPU Cooler</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/8mJtt6/corsair-h100x-572-cfm-liquid-cpu-cooler-cw-9060040-ww">Corsair H100x 57.2 CFM Liquid CPU Cooler</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Motherboard</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/dmGnTW/asus-tuf-gaming-x570-plus-wi-fi-atx-am4-motherboard-tuf-gaming-x570-plus-wi-fi">Asus TUF GAMING X570-PLUS (WI-FI) ATX AM4 Motherboard</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Memory</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/Vh2bt6/corsair-vengeance-lpx-128-gb-4-x-32-gb-ddr4-3600-memory-cmk128gx4m4d3600c18">Corsair Vengeance LPX 128 GB (4 x 32 GB) DDR4-3600 CL18 Memory</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/3kL7YJ/samsung-internal-hard-drive-mz75e250bam">Samsung 850 Evo 250 GB 2.5" Solid State Drive</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">Samsung 870 Evo 1 TB 2.5" Solid State Drive</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">Samsung 870 Evo 1 TB 2.5" Solid State Drive</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/mwrYcf/seagate-barracuda-computer-2-tb-35-7200rpm-internal-hard-drive-st2000dm008">Seagate Barracuda Compute 2 TB 3.5" 7200RPM Internal Hard Drive</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Case</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/bCYQzy/corsair-4000d-airflow-atx-mid-tower-case-cc-9011200-ww">Corsair 4000D Airflow ATX Mid Tower Case</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Power Supply</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/FPYmP6/corsair-rm-2021-850-w-80-gold-certified-fully-modular-atx-power-supply-cp-9020235-na">Corsair RM (2021) 850 W 80+ Gold Certified Fully Modular ATX Power Supply</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Wired Network Adapter</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/2mXfrH/intel-wired-network-card-e1g42etblk">Intel E1G42ETBLK 2 x Gigabit Ethernet PCIe x4 Network Adapter</a></td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Wired Network Adapter</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/2mXfrH/intel-wired-network-card-e1g42etblk">Intel E1G42ETBLK 2 x Gigabit Ethernet PCIe x4 Network Adapter</a></td>
    </tr>
  </tbody>
</table>

# Install of Proxmox

Proxmox was more or less a straight forward of a install. I did run into a few issues with the motherboard I bought. For some reason the first bootup it required a GPU even though it had built in HDMI. Good thing I had my daughter's old pc still around and I was able to pull the old GPU from it and start the boot process in to the bios to turn on SVM for virtualization and install proxmox.

I started off grabing the latest Proxmox VE ISO

[Proxmox ISO](https://www.proxmox.com/en/downloads/category/iso-images-pve)

Proxmox has great documentation, Im not even going to try go over all of it but if you follow this guide you should be just fine.

[Proxmox Install](https://pve.proxmox.com/wiki/Installation)

I downloaded the ISO file and use a great program to write the ISO to a USB drive. 

[Etcher](https://github.com/balena-io/etcher)

The hard drive setup would be as follow using ZFS. 

850 EVO 250gb - Proxmox install

850 EVO 1TB - Virtual machines 
850 EVO 1TB - Virtual machines

Seagate 2TB - VM Templates, Containers, Container Templates

I also have a DS920+ that I use for all my other backups. So it made sense to store any templates, ISO , and Image backups here. This was done by just creating a SMB/CIFS share in proxmox. 

So far so good. I created my first windows and Ubuntu Server templates. Setup backups jobs and recreated a few easy virtual machines I had running on the old ESXI hosts. I have a few that I need to move over but I wanted to try out the conversion process listed below. I need to save that for another weekend when I can migrate and move the old Vmware servers into the Proxmox Datacenter. 

[Migration of servers to Proxmox VE](https://pve.proxmox.com/wiki/Migration_of_servers_to_Proxmox_VE)

So far the setup is running great. IO delay is really low and overall performance has been most excellent!

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/bt.jpg" alt="bt"/>
</div>

>Links
{: .prompt-info }

[Vmware](https://www.vmware.com/)

[Proxmox](https://www.proxmox.com//)


