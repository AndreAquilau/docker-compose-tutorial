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

#### Docker-Compose CLI and Docker-Compose File
```docker-compose.yml
$ sudo docker-compose up
```
#### Services Cloud9 with Docker-Compose
> docker-compose.yml
```.yml
version: '3'

services: 
  cloud9:
    image: sapk/cloud9
    volumes: 
      - .:/workspace
    ports: 
      - "8181:8181"
    command: --auth username:password
```
##### Up Container 
> docker-compose.yml
```bash
$ sudo docker-compose up 
```

##### Up Container in Backgroud -d or --detach
> docker-compose <command> <sub-command> 
```bash
$ sudo docker-compose up -d 
```

##### Up Container Reference File  --file or -f yml
<p>
Usar o --file ou -f quando o nome do arquivo for diferente de docker-compose.yml
</p>

```.yml
$ sudo docker-compose -f *****.yml up -d 
```
#### Down Container 
<p>
O Docker Compose Down e igual ao docker container prune, que ele para os <br/>
container e exclui os containers parados.
</p>

```bash
$ sudo docker-compose down 
```

#### Docker Compose Stop
<p>
O Docker Compose Stop apenas para os container assim como o docker container stop. 
</p>

```bash
$ sudo docker-compose stop
```

#### Docker Compose Ports Rounds
<p>
Quando definimos portas e não mapeamos elas na nossa máquina local <br>
quando rodar o comando up elas seram mapeadas de forma aleatórias.
<p>

```.yml
version: '3'
services: 
  nodejs:
    image: andreaquilau/nodejs
    volumes: 
      - .:/workspace
    ports: 
      - "22"
      - "5000"
    command: /usr/sbin/sshd -D
```
#### Scale - Varias instância de um único serviço
<p>
Scale escala varios container a parti de uma único serviço.
</p>

> docker-compose up -d --scale <name:services>=<quantidade:instância>
```bash
$ suSdo docker-compose up -d --scale nodejs=5
```

#### Docker Compose Run
<p>
O comando RUN é executado uma única vez no momento que o container está <br/>
sendo levantado.
</p>
<p>
Pode ser utilizado para substituir instruções, como -p, -v, -d, --name, ...
</p>

>$ sudo docker-compose run <command> <name:service>  
```bash
$ sudo docker-compose run -d -p 2222:22 -p 8181:80 nodejs
```
#### Trabalhado Com Imagem no Docker Compose
<p>
    Para trabalhamos com imagem atravez do docker compose devemos utilizar <br/>
    a propriedade "build" e passa o diretorio do Dockerfile.
</br>

> build: <diretory:Dockerfile>
```.yml
    version: '3'
    services:
        webapp:
            build: ./dir
```
#### Docker Compose Build força recriar imagem
<p>
Rodando o comando docker-compose build ele irá acessar o arquivo Dockerfile <br/>
e recriar a imagem para o serviço. 
<p>

##### New Image
```bash
$ sudo docker-compose build
```
or
##### New Image And Up Service => used
```bash 
$ sudo docker-compose up --build
```
#### Variável de Ambiente:Environment
Para definir variáveis de ambiente dentro do container usa-se environment
```.yml 
version: '3' 
  services: 
    webapp: 
      build: ./dir 
      environment: 
      POSTGRES_PASSWORD: root 
```

#### Depends Services depends_on 
<p>
Usar depends_on quando tiver um service que precise de um outro service.
</p>

```.yml 
version: '3' 
  services: 
    nodejs:
      image: andreaquilau/nodejs
      volumes: 
        - .:/workspace
      ports: 
        - "22"
        - "5000"
      depends_on:
        - db
      command: /usr/sbin/sshd -D
    
    db:
      image: postgres
      restart: always
      environment:
        POSTEGRES_PASSWORD: mysecretpassword
      ports:
        - "5432"
      
```
#### Docker Compose NetWork
> networks: 

<br>

>  - servidor
```.yml 
version: '3' 
services: 
    nodejs:
      image: andreaquilau/nodejs
      volumes: 
        - .:/workspace
      ports: 
        - "22"
        - "5000"
      depends_on:
        - db
      ## set network in container
      networks:
        - servidor
      command: /usr/sbin/sshd -D
    
    db:
      image: postgres
      restart: always
      environment:
        POSTEGRES_PASSWORD: mysecretpassword
      ports:
        - "5432"     
      ## set network in container
      networks:
        - servidor
## declared network
networks:
  servidor
```
##### Alias For NetWork
```.yml 
version: '3' 
services: 
    nodejs:
      image: andreaquilau/nodejs
      volumes: 
        - .:/workspace
      ports: 
        - "22"
        - "5000"
      depends_on:
        - db
      ## set network in container
      networks:
        servidor:
          aliases:
            - server


      command: /usr/sbin/sshd -D
    
    db:
      image: postgres
      restart: always
      environment:
        POSTEGRES_PASSWORD: mysecretpassword
      ports:
        - "5432"     
      ## set network in container
      networks:
        servidor:
          aliases:
            - server
## declared network
networks:
  servidor
```