---
layout: post
title: PAE Error on boot
---

{% include panel_start.html header="PAE Error on Boot" %}

<b>If you found this page through Google, it would be stupid to follow these
instructions to the letter</b>

{% highlight bash %}
This kernel requires the following features not present on the CPU:
pae
Unable to boot - please use a kernel appropriate for your CPU.
{% endhighlight %}

<b>Step 1: Chroot</b>
{% highlight bash %}
mount /dev/sda2 /mnt
for i in dev proc sys; do mount --bind /$i /mnt/$i; done
chroot /mnt
{% endhighlight %}

<b>Step 2: Make initrd</b>
{% highlight bash %}
cd /boot
mkinitrd -c -k 3.2.45 -m ext2 -f ext2 -r /dev/sda2
{% endhighlight %}

<b>Step 3: Add to lilo</b>
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

<b>Step 4: Update lilo and reboot</b>
{% highlight bash %}
lilo
exit
umount /mnt/{dev,proc,sys,}
reboot
{% endhighlight %}

{% include panel_end.html %}
