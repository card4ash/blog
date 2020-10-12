Title: Oracle linux 7 Basics
Published: 10/11/2020
Tags:
    - Introduction
    - Linux
    - Oracle
---

#Install docker on oracle linux

Take a backup of existing public-yum-ol7.repo :
```shell
    sudo mv /etc/yum.repos.d/public-yum-ol7.repo /etc/yum.repos.d/public-yum-ol7.repo_org1
```

Download the latest public-yum-ol7.repo from Oracle YUM repository:

```
    sudo wget http://yum.oracle.com/public-yum-ol7.repo
```

Make the following changes in your public-yum-ol7.repo file:
```shell
cd /etc/yum.repos.d/
sudo nano public-yum-ol7.repo
```

```
[ol7_latest]
name=Oracle Linux $releasever Latest ($basearch)
baseurl=https://yum.oracle.com/repo/OracleLinux/OL7/latest/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_UEKR4]
name=Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux $releasever ($basearch)
baseurl=https://yum.oracle.com/repo/OracleLinux/OL7/UEKR4/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_optional_latest]
name=Oracle Linux $releasever Optional Latest ($basearch)
baseurl=https://yum.oracle.com/repo/OracleLinux/OL7/optional/latest/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=https://yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```

Reboot your machine:
```
    sudo systemctl reboot
```
Install docker using Oracle yum repository:

```
    yum install docker-engine
```
Enable docker service:

```
    sudo systemctl enable docker
```
Start docker service:

```
    systemctl start docker
```

Check the status:

```
    systemctl status docker.service
```
#links
[https://blogs.oracle.com/blogbypuneeth/a-simple-guide-to-docker-installation-on-oracle-linux-75](https://blogs.oracle.com/blogbypuneeth/a-simple-guide-to-docker-installation-on-oracle-linux-75)