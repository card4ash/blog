Title: Linux 7 Basics
Published: 10/12/2020
Tags:
    - Introduction
    - Linux
---

# Linux command to find out hardware info?
Here is a list of commands to check hardware on Linux. Note that not all commands are available on all distributions. It is better to launch then as root (or via sudo) to get all the information.

```
    lscpu
```
List available cpus and their caracteristics
Not available on older distribution
```
    lshal
```
Require HAL (Hardware Abstraction Layer) to be installed
List all hardware visible by HAL
```
Command: lshw
```
Available on Ubuntu based distributions by default, and Debian in the main repo
Available in the Fedora repositories
Uses many inputs to detect all hardware: Kernel, HAL, DMI, etc.
As a neat ‘-html’ switch that generates hardware reports
Check more on this page
```
Command: lspci
```
Standard command
List all hardware connected to the PCI bus as detected by the kernel
Command: lsusb

Standard command
List all hardware connected to the USB buses as detected by the kernel
```
    dmidecode
```
Standard command
Get the source information from the DMI (a kind of BIOS interface)
List all hardware as reported by the DMI interface