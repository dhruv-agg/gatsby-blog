---
title: Reviving an Old PC!
date: "2023-01-22T23:46:37.121Z"
---

# Reviving an Old PC 🖥 : A Step-by-Step Guide to Installing Linux Mint XFCE with Windows 7 and Zorin OS Lite in Dual Boot

<img src="https://www.linuxmint.com/pictures/screenshots/vera/xfce.png" style="width: 500px; height: 300px;">

---

**⚠️Caution:** Please follow this post cautiously. I will not be responsible for any issue caused to your system.

You can google and find various blogs & videos that will tell you all the standard steps to follow.
But this post is specific to my situation. The constraints I had, the errors I faced & the solutions that worked for me.

### Requirements

0. Stable internet & electricity connection
1. Advanced knowledge about Windows & Linux
2. 1 Old PC
3. 1 Pen Drive

### Preparation

**⚠️Caution:** Take a back up of your data before proceeding.

If Windows 7 is your permanent OS & you don't have any other in dual boot, follow [this](https://linuxmint-installation-guide.readthedocs.io/en/latest/index.html) wonderful guide on how to proceed.

If you already have any other OS in dual boot with Windows 7, you might need to uninstall it, like I did.

1. **Prepare the bootable USB**

    <img src="https://linuxmint-installation-guide.readthedocs.io/en/latest/_images/etcher.png" style="width: 500px; height: 300px;">
    Follow [these](https://linuxmint-installation-guide.readthedocs.io/en/latest/burn.html) steps.

2. **Uninstall Zorin OS**

   <img src="https://assets.zorincdn.com/images/releases/15/15-Lite.png" style="width: 500px; height: 300px;">

   Following steps from this video

   <iframe width="560" height="315" src="https://www.youtube.com/embed/DxtIy2uj9vs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
               
   -  I didn't had a bootable CD for Windows to fix boot menu. So I jumped to uninstalling Zorin. 
   - `Delete Partition` was not enabled for me somehow. I tried to fix it but ended up extending the volume. 
   - EasyBCD or any similar tool is not mandatory as you can fix your BCD using Windows Command Prompt
   - I rebooted my system to begin installing Linux Mint

   <img src="https://neosmart.net/wiki/wp-content/uploads/sites/5/2013/09/Advanced-Boot-Options.jpg" style="width: 500px; height: 300px;">

   - You can try to enter Windows recovery by following [these](https://neosmart.net/wiki/system-recovery-options/) steps if feasible.

3. **Installing Linux Mint**
   <img src="https://linuxmint-installation-guide.readthedocs.io/en/latest/_images/grub-efi.png" style="width: 500px; height: 300px;">

   I got a similar GRUB menu & followed the steps described [here](https://linuxmint-installation-guide.readthedocs.io/en/latest/boot.html)
   After installation is completed. We have to fix boot menu before restarting.

4. **Repair Boot Menu**
   <img src="https://linuxhint.com/wp-content/uploads/2018/09/10-6.png" style="width: 500px; height: 300px;">
   It is possible to fix any boot issues using Linux. In fact, if you are stuck in a situation try to boot into another OS & fix the boot issues.

   Boot Repair is a tool to repair common boot issues on Ubuntu, Debian, Arch, Linux Mint, OpenSUSE, Fedora and other Linux distributions, Windows and Mac OS operating systems.
   I followed [this](https://linuxhint.com/ubuntu_boot_repair_tutorial/) guide.

   You have to

   - Install Boot Repair
   - Do Recommended Repair
   - Backup Partition Table
   - Repair File System

Try rebooting & check if both Linux Mint & Windows 7 are working as expected.

👋 You have successfully installed Linux Mint in dual boot with Windows 7 after uninstalling Zorin OS! 🥳

---
Thanks for reading 🙏
If you’ve made it this far, I really thank you for your time :) 
That’ll be all for me today. Until next time 👋