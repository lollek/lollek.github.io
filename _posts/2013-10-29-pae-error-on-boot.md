---
layout: post
title: PAE Error on boot
---

{% include panel_start.html header="PAE Error on Boot" %}
{% capture data %}
***If you found this page through Google, it would be stupid to follow these instructions to the letter***
{% endcapture %}{{ data | markdownify}}

{% capture data %}
```
This kernel requires the following features not present on the CPU:
pae
Unable to boot - please use a kernel appropriate for your CPU.
```

## Step 1: Chroot
```
mount /dev/sda2 /mnt
for i in dev proc sys; do mount --bind /$i /mnt/$i; done
chroot /mnt
```

## Step 2: Make initrd
```
cd /boot
mkinitrd -c -k 3.2.45 -m ext2 -f ext2 -r /dev/sda2
```

## Step 3: Add to lilo
```
echo >>/etc/lilo.conf <<EOF
# Linux bootable partition config begins
image = /boot/vmlinuz-generic-3.2.45
initrd = /boot/initrd.gz
root = /dev/sda2
label = Slacky-3.2.45-initrd
read-only
# Linux bootable partition config ends
EOF
```

## Step 4: Update lilo and reboot
```
lilo
exit
umount /mnt/{dev,proc,sys,}
reboot
```

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
