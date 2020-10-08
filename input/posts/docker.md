Title: Docker for Windows
Published: 09/04/2020
Tags:
    - docker
    - windows
---

**Docker Basic Command**

all the process for docker
```
    docker ps
```

```
    docker ps -a
```
pulling an image from docker-hub if not available locally and install it

```
    docker run hello-world
```

```
    docker image ls
```
Docker run in port 80 and mapping it to host port 80
hostport:containerport
```
    docker run -p 80:80 nginx
```
stop the software 
```
    docker stop parts_of_container_id_which_is_unique
```
```
    docker stop ee3
```

```
    docker ps -a
```

```
    docker start container_name/part_of_container_id
```
uninstalling software

```
    docker rm ee3
```
```
    docker rm ee3 b9 etc_rasa
```
Removing images 
```
    docker rmi nginx
```

Docker command help

```
    docker help search
```
Search from command line
```
    docker search docs
```

Run docker documents locally

```
    docker run -p 4000:4000 docs/docker.github.io
```

Run docker server with interactive command line with name
```
    docker run -p 4000:4000 -it --name docs docs/docker.github.io
```

Running server in background in detached mode

```
    docker run -p 81:80 --name iis -d microsoft/iis:nanoserver
```

So see the details about runnig image

```
    docker inspect iis
```
Get the process associated with dockeer in powershell

```
    get-process *docker*
```


**Installing Docker Engine on Windows Server 2016**

[Set up Environment](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-Server)

```
    Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
```
```
    Install-Package -Name docker -ProviderName DockerMsftProvider
    Restart-Computer -Force
```

```
    docker run microsoft/dotnet:nanoserver
```

```
    docker run -it microsoft/dotnet:nanoserver dotnet --version
```

**Running Command Line Apps in Containers**

```shell
    docker save nanoserver/iis -o iis.tar
```

***Using linux container to extract tar***

```shell
    docker run -it alpine
```
mounting c:\Users to container /data folder and show all files and folder inside container /data folder

```shell
    docker run --rm -v c:/Users:/data alpine ls /data
    docker run --rm -v c:/Users/ashraful.alam/iistartest:/data alpine ls /data 
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine sh
    bash:\# tar -tf /data/iis.tar
    \# mkdir /data/extract
    \# tar -xf /data/iis.tar -C /data/extract
    \# cd /data/extract/ 
    \# ls
    \# cat manifest.json
    \# jq
    \# apk add --no-cache jq
    \# cat manifest.json | jq
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine tar -xf /data/extract/e7306db32ff89edf4f4b1da671bb3923cdd116eb90df9882a78f5dbe3e92f019/layer.tar -C /data/extract/e7306db32ff89edf4f4b1da671bb3923cdd116eb90df9882a78f5dbe3e92f019/layer
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine sh 
    \#ls
    \#cd data
    \#cd extract
    \#cd 7a27115c84f3a27e5658c2af681b313ddec35152658fc89270380c0495d0434b
    \#mkdir layer
    \#tar -xf layer.tar -C layer  
```
```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data ubuntu sh
```

nmap port scanning

```shell
    docker run --rm `
    weshigbee/nmap -v 192.168.0.0/24
```

ffmpeg converting mp4 to gif

```shell
    docker run --rm `
    --volume ${pwd}:/output `
    jrottenberg/ffmpeg -i http://www.weshigbee.com/wp-content/uploads/2014/12/Turkey-Short.mp4 /output/Turkey.gif
```

