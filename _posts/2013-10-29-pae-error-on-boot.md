---
layout: post
title: PAE Error on boot
---
***NB - If you found this page through Google, do NOT follow these instructions***

{% highlight bash %}
This kernel requires the following features not present on the CPU:
pae
Unable to boot - please use a kernel appropriate for your CPU.
{% endhighlight %}

Step 1: Chroot
-------------
Boot up the computer through a rescue disk/usb
{% highlight bash %}
mount /dev/sda2 /mnt
for i in dev proc sys; do mount --bind /$i /mnt/$i; done
chroot /mnt
{% endhighlight %}

Step 2: Make initrd
-------------------
{% highlight bash %}
cd /boot
mkinitrd -c -k 3.2.45 -m ext2 -f ext2 -r /dev/sda2
{% endhighlight %}

Step 3: Add to lilo
-------------------
{% highlight bash %}
echo >>/etc/lilo.conf <<EOF
# Linux bootable partition config begins
image = /boot/vmlinuz-generic-3.2.45
initrd = /boot/initrd.gz
root = /dev/sda2
label = Slacky-3.2.45-initrd
read-only
# Linux bootable partition config ends
EOF
{% endhighlight %}

Step 4: Update lilo and reboot
-----------------------------
{% highlight bash %}
lilo
exit
umount /mnt/{dev,proc,sys,}
reboot
{% endhighlight %}
