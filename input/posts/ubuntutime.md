Title: Ubuntu time
Published: 06/11/2019
Tags:
    - Ubuntu
    - UbuntuTime
    - Introduction
    - Basics
---

Time Synchronization on Ubuntu 
=================================

Basic Time Commands
********************

```shell

    date
```

```shell

    timedatectl list-timezones
```

```shell

    sudo timedatectl set-timezone Asia/Dhaka

```

```shell 

    timedatectl
```

```shell

    sudo dpkg-reconfigure tzdata
```

Configure NTP Servers
***********************

```shell

    apt-get install ntp
```

```shell

     sudo nano /etc/ntp.conf
```

```shell

     ntpq -p

```

```shell

    service ntp restart

```