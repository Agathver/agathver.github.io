---
layout: post
title:  'Almost Zero Configuration NFS Network Shares using AutoFS'
date:   '2017-06-26 11:55:12+05:30'
categories: stories
tags: [filesystems, linux, networks]
comments: true
redirect_to: 'https://medium.com/@agathver/almost-zero-configuration-nfs-network-shares-using-autofs-99cdbabceb69'
---

AutoFS is a great and easy way to mount filesystems automatically, on-demand, when available. It works great for both networked and non-networked drives.

I have a HP Laptop which I use as my development machine and a (highly-)upgraded Compaq CQ 3329-IX that doubles as my development as well as file-server. I want my files to be available automatically, whenever I am connected to my home-network. I cannot add a fstab entry as I am not always connected to the same network.

## AutoFS to the rescue

If you have a laptop and separate file-server where you store files, AutoFS is a great way to ensure automatic, on-demand mounting and unmounting of network shares. It even allows for auto-discovery and auto-mounting, so that file-sharing on the client is almost zero-configuration.

This assumes you already have a working NFS share setup, in a machine with host name - `theserver`, and let's say your client is named as - `theclient`.

_AutoFS needs domain names, thus you need a DNS stack such as Avahi/mDNS, a DHCP server or appropriate entries in `/etc/hosts`_

My home network uses mDNS. My host and my client are assigned `thehost.local` and `theclient.local` respectively. mDNS sets up domain names with zero configuration, and works in a variety of devices, including mobile.

## Installing and configuring AutoFS

The process is very simple. Use your package manager to install the `autofs` package.

```
user@theclient:~$ sudo dnf install autofs
user@theclient:~$ sudo systemctl enable autofs.service
user@theclient:~$ sudo systemctl start autofs.service
```

Now you will be automatically be able to access files at `theserver.local` at `/net/theserver.local/share`, where share is the path configured at `/etc/exports` in `theserver.local`

In Fedora and CentOS, the `/etc/autofs.net` file comes enabled out-of-the-box. For other distributions, you may need to add: `/net	-hosts` to `/etc/auto.master`.

In addition to NFS, you can also access SAMBA shares under (/smb) in a similar fashion, unless they require authentication.

[AutoFS in Ubuntu wiki][1] describes various setup options for AutoFS.

## Alternatives

There are other alternatives such as systemd-automount and gvfs-automount, however, they involve much more complicated configuration.

[1]: https://help.ubuntu.com/community/Autofs


