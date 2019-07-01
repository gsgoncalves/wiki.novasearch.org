---
title: Ubuntu 14.04 LTS
permalink: wiki/Ubuntu_14.04_LTS/
layout: wiki
---

### Install Dropbox

Don't install from the website. Install like this instead:

### Fix ugly Windows fonts

Disable embedded bitmaps for Calibri and others.

### Better Power Management

Add the TLP-PPA to your package sources with the commands:

`$ sudo add-apt-repository ppa:linrunner/tlp`  
`$ sudo apt-get update`

The packages may be installed via package management tools:

`$ sudo apt-get install tlp tlp-rdw`

ThinkPads require an additional:

`$ sudo apt-get install tp-smapi-dkms acpi-call-tools`

Remove default Ubuntu CPU frequency configuration

`$ sudo update-rc.d -f ondemand remove`
