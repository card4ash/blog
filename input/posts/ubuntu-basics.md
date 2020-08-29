Title: Ubuntu basics
Published: 06/11/2019
Tags:
    - Ubuntu
    - UbuntuUser
    - Introduction
    - Basics
---

Ubuntu Basics
===================


How To Create a Sudo User on Ubuntu 


Check Ubuntu Version

```shell

        lsb_release -a
```

```shell

       adduser username
```

Set and confirm the new user's password at the prompt. A strong password is highly recommended!

```shell

        Set password prompts:
        Enter new UNIX password:
        Retype new UNIX password:
        passwd: password updated successfully
```

Follow the prompts to set the new user's information. It is fine to accept the defaults to leave all of this information blank.

```shell

        User information prompts:
        Changing the user information for username
        Enter the new value, or press ENTER for the default
            Full Name []:
            Room Number []:
            Work Phone []:
            Home Phone []:
            Other []:
        Is the information correct? [Y/n]
```

Use the usermod command to add the user to the sudo group.

```shell

        usermod -aG sudo username
```

By default, on Ubuntu, members of the sudo group have sudo privileges.
Use the su command to switch to the new user account.

```shell

        su - username
```

```shell

        sudo ls -la /root
```

********************************
Set static IP Ubuntu 16.04
********************************

``` shell

        sudo nano /etc/network/interfaces

```

and paste this under ``# The primary network interface:``

```shell

        auto enp0s25
        iface enp0s25 inet static
        address 192.168.0.16
        netmask 255.255.255.0
        gateway 192.168.0.1
        dns-nameservers 8.8.4.4 8.8.8.8
```

```shell

        sudo /etc/init.d/networking restart
```

``` shell

        # The loopback network interface  
        auto lo  
        iface lo inet loopback  


        # The primary network interface  
        auto enp8s0 
        iface enp8s0 inet static  
        address 192.168.11.95
        netmask 255.255.255.0
        gateway 192.168.11.1
        dns-nameservers 8.8.8.8 8.8.4.4 
```


``sudo ifdown enp8s0 && sudo ifup enp8s0``