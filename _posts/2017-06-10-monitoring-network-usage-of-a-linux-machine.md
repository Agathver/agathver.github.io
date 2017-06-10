---
layout: post
title:  "Monitoring Network Usage of a Linux Machine"
date:   2017-06-10 23:06:31 +0530
categories: stories
tags: [cloud, linux, networks]
comments: true
---

If you are on a Linux Workstation and are wondering what process is eating away your bandwith, or need to keep track of your VPSs monthly bandwidth usage, I am going to show you two effective tools for this purpose.

## Monitor your daily/weekly/monthly/yearly bandwidth usage with vnStat
Introducing `vnStat`. Might sound lot familiar to `vmstat`, one that shows memory statistics, while this one shows network statistics. It shows you realtime bandwidth usage and aggregrated usage over a time period. It is composed of two parts `vnstatd`, the daemon responsible for polling `/proc` file-system in Linux to fetch network statistics and save them in a database. And a client program `vnstat`, which configures and shows the statistics collected by the daemon.

`vnstat` is a simple easy to use program. You can install it using your distributions package manager - it is available in Debian, Ubuntu, Fedora and Arch repositories. CentOS and RHEL users need to enable the EPEL repos for installing `vnstat`. I am a Fedora user, so my instructions and outputs are based on Fedora. However, it doesn't require any weird kernel-foo so it can be easily adopted to your distro.

### Installing vnstat

```bash
sudo dnf install vnstat
```
This fetches vnstat from the repositories and installs it. Proceed to find the list of network interfaces on your machine and pick the one (or many) that you want to watch for.

```bash
ifconfig
```
This command neatly lists all devices and their types.

You need to configure vnstat to collect data for a device by creating a database for them. Here is how to do it for a device named eth0.

```bash
sudo vnstat --create -i eth0
```

`vnstat` comes with a systemd service, which you need to enable for data collection. This might not be necessary on Debian/Ubuntu. For systems without systemd, you need to consult the manual for enabling a daemon.

```bash
systemctl enable vnstat.service
systemctl start vnstat.service
```

If `vnstat` was already enabled and running before configuring your interface, you will need to restart the service.

```bash
systemctl restart vnstat.service
```

Stats will be available in a while and you can check them using `vnstat` command

```bash
vnstat
```

                        rx      /      tx      /     total    /   estimated
    wlo1:
        Jun '17       590 KiB  /      13 KiB  /     603 KiB  /       0 KiB
            today       590 KiB  /      13 KiB  /     603 KiB  /      --    

    eno1: Not enough data available yet.
    enp0s18f2u1:
        Jun '17      1.29 GiB  /   97.09 MiB  /    1.39 GiB  /    4.29 GiB
            today      1.29 GiB  /   97.09 MiB  /    1.39 GiB  /    1.48 GiB

This is what `vnstat` shows on my laptop, (eno1 is my PCI ethernet, and I was not using it today)

```bash
vnstat -l -i enp0s18f2u1
```
This will show you live transfer rates on the selected interface.

    Monitoring enp0s18f2u1...    (press CTRL-C to stop)

    rx:       30 kbit/s     9 p/s          tx:       24 kbit/s    10 p/s

And on pressing <kbd>Ctrl + C</kbd>, you will get the full traffic statistics for the duration.

    Monitoring enp0s18f2u1...    (press CTRL-C to stop)

    rx:       30 kbit/s     9 p/s          tx:       24 kbit/s    10 p/s^C


    enp0s18f2u1  /  traffic statistics

                            rx         |       tx
    --------------------------------------+------------------
    bytes                      504 KiB  |         132 KiB
    --------------------------------------+------------------
            max             833 kbit/s  |       90 kbit/s
        average           36.24 kbit/s  |     9.50 kbit/s
            min               0 kbit/s  |        0 kbit/s
    --------------------------------------+------------------
    packets                        677  |             689
    --------------------------------------+------------------
            max                100 p/s  |          87 p/s
        average                  5 p/s  |           6 p/s
            min                  0 p/s  |           0 p/s
    --------------------------------------+------------------
    time                  1.90 minutes

For small scale uses, this is quite adequate to keep a watch on the bandwidth usage. Bu if you manage several servers, looking into a monitoring solution such as ELK stack or Nagios will be helpful. You may also try [Datadog](https://www.datadoghq.com/). They are quite nice!

## Hey, I think some process is hogging my bandwidth!
Meet nethogs, to find those sneaky hogs that eat away your network bandwidth! It's really simple to install and use. And it does one task, listing bandwith usage by program and connection, updated in realtime.

```bash
sudo dnf install nethogs
```

And to see the live stats:

```bash
nethogs
```

## Background
I discovered `nethogs` when I was really angry of some random process eating away 4GB (my entire daily data allowance) in 2 hours, while I was busy on a phone call. Turned out to be a system update and an unexpected pest - a feed reader which I newly installed, trying to download all unread posts for offline viewing in my entire Feedly library. And I have used `vnstat` to monitor bandwidth usage for a VPS, before starting to use ELK stack and then Datadog.
