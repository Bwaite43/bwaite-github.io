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

While Vmware was really good for my homelab it was getting to be an effort. I used Vmware's Vmug program to test out differnt vmware products like NSX, vRA, and ESXI products. We are a big VMware shop at work and I wanted to test out building and breaking things without the vmware admins getting mad at me. I set out to automate and deploy Windows and linux servers with a front end website to request builds following up with something that could enforce configuration changes like puppet . This worked great for my homelab for a good 5 years but it was getting old paying the subscription fee for vmug and having to find specific hardware or modify the boot config to deploy ESXI on hardware I had around the house. We also had the solution deployed at work so the need to figure out how it all works together was done plus I was over being restricted around server hardware. Time for a switch....

## In comes Proxmox

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/proxmox.png" alt="proxmox"/>
</div>

So were off on a mission to migrate over to proxmox, I cant use the already running Vmware environment as I have servers running on both ESXI hosts and the memory is maxed out. Time to test out building out a new server with hardware I wanted.

<a href="https://pcpartpicker.com/list/7rK7wc">PCPartPicker Part List</a>
<table class="pcpp-part-list">
  <thead>
    <tr>
      <th>Type</th>
      <th>Item</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="pcpp-part-list-type">CPU</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/KwLwrH/amd-ryzen-9-5900x-37-ghz-12-core-processor-100-100000061wof">AMD Ryzen 9 5900X 3.7 GHz 12-Core Processor</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/KwLwrH/amd-ryzen-9-5900x-37-ghz-12-core-processor-100-100000061wof">$395.98 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">CPU Cooler</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/PVfFf7/nzxt-kraken-x53-7311-cfm-liquid-cpu-cooler-rl-krx53-01">NZXT Kraken X53 73.11 CFM Liquid CPU Cooler</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/PVfFf7/nzxt-kraken-x53-7311-cfm-liquid-cpu-cooler-rl-krx53-01">$134.99 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Motherboard</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/dmGnTW/asus-tuf-gaming-x570-plus-wi-fi-atx-am4-motherboard-tuf-gaming-x570-plus-wi-fi">Asus TUF GAMING X570-PLUS (WI-FI) ATX AM4 Motherboard</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/dmGnTW/asus-tuf-gaming-x570-plus-wi-fi-atx-am4-motherboard-tuf-gaming-x570-plus-wi-fi">$199.00 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Memory</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/pdCFf7/corsair-vengeance-lpx-128-gb-4-x-32-gb-ddr4-2400-memory-cmk128gx4m4a2400c16">Corsair Vengeance LPX 128 GB (4 x 32 GB) DDR4-2400 CL16 Memory</a></td>
      <td class="pcpp-part-list-price">
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/4mkj4D/crucial-mx500-250gb-25-solid-state-drive-ct250mx500ssd1">Crucial MX500 250 GB 2.5" Solid State Drive</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/4mkj4D/crucial-mx500-250gb-25-solid-state-drive-ct250mx500ssd1">$43.99 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">Samsung 870 Evo 1 TB 2.5" Solid State Drive</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">$103.57 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">Samsung 870 Evo 1 TB 2.5" Solid State Drive</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/7nsnTW/samsung-870-evo-1-tb-25-solid-state-drive-mz-77e1t0bam">$103.57 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Storage</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/mwrYcf/seagate-barracuda-computer-2-tb-35-7200rpm-internal-hard-drive-st2000dm008">Seagate Barracuda Compute 2 TB 3.5" 7200RPM Internal Hard Drive</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/mwrYcf/seagate-barracuda-computer-2-tb-35-7200rpm-internal-hard-drive-st2000dm008">$49.99 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Case</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/bCYQzy/corsair-4000d-airflow-atx-mid-tower-case-cc-9011200-ww">Corsair 4000D Airflow ATX Mid Tower Case</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/bCYQzy/corsair-4000d-airflow-atx-mid-tower-case-cc-9011200-ww">$94.99 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td class="pcpp-part-list-type">Power Supply</td>
      <td class="pcpp-part-list-item"><a href="https://pcpartpicker.com/product/26rRsY/corsair-rmx-2021-850-w-80-gold-certified-fully-modular-atx-power-supply-cp-9020200-na">Corsair RMx (2021) 850 W 80+ Gold Certified Fully Modular ATX Power Supply</a></td>
      <td class="pcpp-part-list-price">
        <a href="https://pcpartpicker.com/product/26rRsY/corsair-rmx-2021-850-w-80-gold-certified-fully-modular-atx-power-supply-cp-9020200-na">$139.99 @ Amazon</a>
      </td>
    </tr>
    <tr>
      <td></td>
      <td class="pcpp-part-list-price-note">Prices include shipping, taxes, rebates, and discounts</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td class="pcpp-part-list-total">Total</td>
      <td class="pcpp-part-list-total-price">$1266.07</td>
    </tr>
    <tr>
      <td></td>
      <td class="pcpp-part-list-price-note">Generated by <a href="https://pcpartpicker.com">PCPartPicker</a> 2022-06-21 22:31 EDT-0400</td>
      <td></td>
    </tr>
  </tbody>
</table>

>Links
{: .prompt-info }

[Vmware](https://www.vmware.com/)

[Proxmox](https://www.proxmox.com//)


