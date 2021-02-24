## Overview of Docker Compose
[Docker-Composer](https://docs.docker.com/compose/)
<p>
O Compose é uma ferramenta para definir e rodar aplicações multe containers.
</p>

___ Primeiro Definir o Dockerile ___
<p>
Primeiro definir os serviços executados pelo docker.
</p>

```.yml
version: '1.0.0'
services: 
    web:
        build: .
        ports:
        - "5000:5000"
        valoumes:
        - $(pwd)/docker/app:/workspace
        links:
        - redis:
    redis:
        image: redis
logvolume01: {}
```
#### Install Docker Compose
[link install docker composer](https://docs.docker.com/compose/install/)
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```bash
$ sudo chmod +x /usr/local/bin/docker-compose

$ sudo docker-compose --version
```

