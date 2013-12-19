---
layout: post
title: PAE Error on boot
---
Today I had some issues reaching my server through SSH, so I decided to restart it. In the end, it turned out that the reason I couldn't connect was because of my ISP, but I received a nice little problem during the reboot.
# highlight bash
This kernel requires the following features not present on the CPU:
pae

Unable to boot - please use a kernel appropriate for your CPU.
# endhighlight

So what is pae you might ask? According to Wikipedia:
***Physical Address Extension, an x86 computer processor feature for accessing more than 4 gigabytes of memory***
Since my sad piece of server has no way near 4 GB of RAM, there wasn't any reason for me to use PAE (even if it would have been supported).
So I booted up the server on a rescue USB and found these kernels (with the bold one being the active one):
<ul>
  <li>generic-3.2.45</li>
  <li>generic-smp-3.2.45-smp</li>
  <li>huge-3.2.45</li>
  <li>*huge-smp-3.2.45-smp*</li>
</ul>
For some reason, the last Slackware update (14) decided that I wanted a huge smp kernel. Since it happened once it will probably happen again, so I'm writing a quick guide to the needful fix.

Step 1: Chroot 
-------------
Boot up the computer through a rescue disk/usb
# highlight bash
mount /dev/sda2 /mnt
for i in dev proc sys; do mount --bind /$i /mnt/$i; done
chroot /mnt
# endhighlight

Step 2: Make initrd
-------------------
# highlight bash
cd /boot
mkinitrd -c -k 3.2.45 -m ext2 -f ext2 -r /dev/sda2
# endhighlight

Step 3: Add to lilo
-------------------
# highlight bash 
echo <<EOF >>/etc/lilo.conf 
# Linux bootable partition config begins
image = /boot/vmlinuz-generic-3.2.45
initrd = /boot/initrd.gz
root = /dev/sda2
label = Slacky-3.2.45-initrd
read-only
# Linux bootable partition config ends
EOF
# endhighlight

Step 4: Update lilo and reboot
-----------------------------
# highlight bash
lilo
exit
umount /mnt{/dev,/proc,/sys,}
reboot
# endhighlight 
