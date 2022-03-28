Title: Docker for Windows
Published: 09/04/2020
Tags:
    - docker
    - windows
---

# Docker Basic Command

All the process for docker
```
    docker ps
```

```
    docker ps -a
```
Pulling an image from docker-hub if not available locally and install it

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
Stop the software 
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
Uninstalling/Removing software/docker container

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
Docker IP Address

```shell
    docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

Get the process associated with dockeer in powershell

```
    get-process *docker*
```


# Installing Docker Engine on Windows Server 2016

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

# Running Command Line Apps in Containers

```shell
    docker save nanoserver/iis -o iis.tar
```

**Using linux container to extract tar**

```shell
    docker run -it alpine
```
Mounting c:\Users to container /data folder and show all files and folder inside container /data folder

```shell
    docker run --rm -v c:/Users:/data alpine ls /data
    docker run --rm -v c:/Users/ashraful.alam/iistartest:/data alpine ls /data 
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine sh
    bash:\# tar -tf /data/iis.tar
    # mkdir /data/extract
    # tar -xf /data/iis.tar -C /data/extract
    # cd /data/extract/ 
    # ls
    # cat manifest.json
    # jq
    # apk add --no-cache jq
    # cat manifest.json | jq
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine tar -xf /data/extract/e7306db32ff89edf4f4b1da671bb3923cdd116eb90df9882a78f5dbe3e92f019/layer.tar -C /data/extract/e7306db32ff89edf4f4b1da671bb3923cdd116eb90df9882a78f5dbe3e92f019/layer
```

```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data alpine sh 
    # ls
    # cd data
    # cd extract
    # cd 7a27115c84f3a27e5658c2af681b313ddec35152658fc89270380c0495d0434b
    # mkdir layer
    # tar -xf layer.tar -C layer  
```
```shell
    docker run --rm -it -v c:/Users/ashraful.alam/iistartest:/data ubuntu sh
```

Nmap port scanning

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

# Building Images to Host Web Sites

```shell
    docker run --rm -it -p 8080:80 nginx
    docker run --rm -it -p 8080:80 -v C:\Users\ashraful.alam\solitaire\app:/usr/share/nginx/html nginx


    docker run -d -p 8080:80 --name ngi nginx
    docker cp .\app\. ngi:/usr/share/nginx/html 
    docker exec -it ngi bash
    cd /usr/share/nginx/html

    docker exec nginx ls /usr/share/nginx/html 
```

**Create Image**

```shell
    docker commit ngi solitaire:nginx
    docker run -d -p 8090:80 solitaire:nginx
    docker exec 4f9e1 ls /usr/share/nginx/html
```

**Image layer**

```shell
    docker history nginx
```

**Docker File**

```shell
    From nginx
    COPY app /usr/share/nginx/html
```

```shell
mv  .\Dockerfile.txt .\Dockerfile
```

```shell
    docker build -f Dockerfile -t solitaire:nginx-df .
    or
    docker build -t solitaire:nginx-df .
```

```shell
    docker images | sls nginx
```

**Publishing to Docker HUb**

```shell
    docker tag solitaire:nginx-df ashcoder/solitaire:nginx
    docker push  ashcoder/solitaire:nginx
```

# Running Database in Container
MS SQL Server on linux Docker

```shell
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=sqladmin@123" -p 1450:1433 -d mcr.microsoft.com/mssql/server
     docker logs container_id_partial
```

```shell
    docker run --name some-mysql -p 3305:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
    docker exec -it some-mysql mysql --user=root --password=my-secret-pw 
```


# Clean Up
stop all container
```shell
    docker stop $(docker ps -q)
```
remove all stoped container
```shell
    docker container prune
```
remove all container
```shell
docker rm -f $(docker ps -aq)
```

Remove unused volumes
```shell
    docker volume prune
```
remove all volume
```shell
docker volume rm $(docker volume ls -q)
```
remove container with volume
```shell
docker rm -fv container_id
```

# Docker Compose
```shell
    docker run --name db `
		-d `
		-p 3306:3306 `
		-e MYSQL_ROOT_PASSWORD=my-secret-pw `
		-v db:/var/lib/mysql `
		mysql
		
docker inspect db #extract ip address

docker run --name web `
		-d `
		-p 8080:80 `
		-e MY_DB_PORT=3306 `
		-e MY_DB_HOST=? `
		-v /my/php/app:/usr/share/nginx/html `
		nginx
```
docker-compose.yml file
```shell
version: '2'

services:
    teamcity:
        image: sjoerdmulder/teamcity
        ports:
            - 8111:8111
    teamcity-agent:
        image: sjoerdmulder/teamcity-agent
        environment:
            - TEAMCITY_SERVER=http://teamcity:8111
    postgres:
        image: postgres
        environment:
            - POSTGRES_DB=teamcity
            - POSTGRES_PASSWORD=teamcity
```

```shell
    docker-compose up
    docker network ls 
    docker network inspect eb1_container_id
    docker network inspect bridge
    docker exec -it teamcity_postgres_1 bash
    docker-compose exec postgres bash 
    docker run --name alpine --rm --net teamcity_default -it alpine sh
    docker-compose exec teamcity bash
    docker-compose start teamcity-agent
    docker-compose ps
    docker-compose exec postgres psql -U postgres 
    docker-compose stop
    docker-compose rm
    docker-compose down
```

```shell
version: ‘2’
services:
    db:
        image: mysql
        ports:
        -3306:3306
        environment:
        -MYSQL_ROOT_PASSWORD=my-secret-pw
        volumes:
        -db:/var/lib/mysql
    web:
        image: nginx
        ports:
        -8080:80
        environment:
        -MY_DB_PORT=3306
        -MY_DB_HOST=db
        volumes:
        -/my/php/app:/usr/share/nginx/html

```
[github://friism/MusicStore](https://github.com/friism/MusicStore)


```shell
version: '3'
services:
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433" # REMARK: This is currently required, needs investigation
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 30

  web:
    build:
      context: .
      dockerfile: Dockerfile.windows
    environment:
      - "Data:DefaultConnection:ConnectionString=Server=db,1433;Database=MusicStore;User Id=sa;Password=Password1;MultipleActiveResultSets=True"
    depends_on:
      - db
    ports:
      - "5000:5000"

```

# installing sql server on docker desktop

links: https://docs.microsoft.com/en-us/sql/linux/tutorial-restore-backup-in-sql-server-container?view=sql-server-ver15

**Restore Database from bak file on host**

```shell
docker cp E:\Tutorial\AdventureWorksLT.bak sql1:/var/opt/mssql/backup
```

```shell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost  -U SA -P "<YourStrong!Passw0rd>"  -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/AdventureWorksLT.bak'"
```


