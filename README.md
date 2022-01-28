## `Learn how you can build and push a Docker image for Dockerhub`

### `Table of contents`
- [Installing Docker](#Installing-Docker)
- [Docker: Basic commands](#Docker-Basic-commands)
- [Test the NodeJS application to run locally](#Using-the-NodeJS-application-to-run-locally-on-my-machine)
- [Run NodeJS application from container](#Using-the-NodeJS-application-to-run-on-container)

### `Installing Docker`

1. Before you get started, please make sure you have *Docker* installed on your machine:

```
curl -fsSl https://get.docker.com
```

1.1. Make sure *Docker service* is running state:

```
docker-tutorialz]#  systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Qua 2022-01-26 22:49:42 -03; 7min ago
     Docs: https://docs.docker.com
 Main PID: 4627 (dockerd)
   CGroup: /system.slice/docker.service
           └─4627 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Jan 26 22:49:41 elliot01 dockerd[4627]: time="2022-01-26T22:49:41.146036873-03:00" level=info msg="Firewalld: interface docker_gwbridge already part of docke...returning"
Jan 26 22:49:42 elliot01 systemd[1]: Started Docker Application Container Engine.
Jan 26 22:49:42 elliot01 dockerd[4627]: time="2022-01-26T22:49:42.231763910-03:00" level=info msg="API listen on /var/run/docker.sock"
Hint: Some lines were ellipsized, use -l to show in full.
```

### `Docker: Basic commands`

1. Before we need test a container image to demonstrate a `docker container run` command:

```
# docker container run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:507ecde44b8eb741278274653120c2bf793b174c06ff4eaa672b713b3263477b
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

1.1. List all *Docker* containers:

```
# docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@elliot01 docker-tutorialz]# docker container ls -a
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS                     PORTS     NAMES
ea850619834e   hello-world     "/hello"                 2 minutes ago   Exited (0) 2 minutes ago             objective_haslett
ea3d6cba32ce   nginx:latest    "/docker-entrypoint.…"   2 months ago    Exited (255) 6 weeks ago   80/tcp    amaury.2.no71j9ujjjoj14u8bi62vqx5y
79151c794c62   nginx:latest    "/docker-entrypoint.…"   2 months ago    Exited (255) 6 weeks ago   80/tcp    amaury.3.bkbp2ugc5xoh1fagt02ha6nso
73a12ad1f474   nginx:latest    "/docker-entrypoint.…"   2 months ago    Exited (255) 6 weeks ago   80/tcp    amaury.1.tsmucyxcwftavjvy3vnzrcx40
2aaf276affba   alpine:latest   "ping 8.8.8.8"           2 months ago    Dead                                 eager_gates.2.pslfwckn3sc0gfl6mq5wrvy36
c00037dfdb53   nginx:latest    "/docker-entrypoint.…"   2 months ago    Exited (0) 2 months ago              amaury.1.vqdl6rxuu0x30o6jchlh66iqt
```

1.2. Now another example about how you can set a name for the *Docker* container: 

```
 # docker container run --name amaury hello-world
```

1.3. Listing the containers:

```
# docker container ls -a
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS                     PORTS     NAMES
cead5b46cf1e   hello-world     "/hello"                 5 seconds ago   Exited (0) 3 seconds ago             amaury
```

1.4. Other way to create a new *Docker* container using `-ti` option to interatic with container:

```
# docker container run -ti ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
ea362f368469: Pull complete
Digest: sha256:b5a61709a9a44284d88fb12e5c48db0409cfad5b69d4ff8224077c57302df9cf
Status: Downloaded newer image for ubuntu:latest
root@804841b67ea6:/#
```

1.5. Creating a *MongoDB* container:

- before we need check the Dockerhub about [MongoDB](https://hub.docker.com/_/mongo):

```
# docker container run -d -e MONGO_INITDB_ROOT_USERNAME=mongouser -e MONGO_INITDB_ROOT_PASSWORD=mongopwd mongo
2833a64a61b6637c77e4df70cdf2442ccd56b19dc1d63037307cb69497b2178a
```

1.6. Now we need access this container, you can do this below:

- port-bind (connection port with container, we need create a port-bind, get a local port of my machine and link with container port)
```
Options:
-d - execute container in background 
-p - port-bind
-e - environment variables 
```
- Running the command again using more options:

```
# docker container run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongouser -e MONGO_INITDB_ROOT_PASSWORD=mongopwd mongo
99d012a1ceb7ed598230fd4a11efb3c05d6404c61a1503c32d3a3ab7245bc8bb
```

- Listing the container:

```
# docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                           NAMES
99d012a1ceb7   mongo     "docker-entrypoint.s…"   26 seconds ago   Up 23 seconds   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   elated_hawking
```

### `Using the NodeJS application to run locally on my machine`

1.1. Before to start, you must clone the repository from [github.com/kubedev](https://github.com/KubeDev/api-produto)

```
 git clone https://github.com/amaurybsouza/api-produto.git
```

1.2. Now you need install some packages like, `nodejs` and `npm`:

```
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
```

1.3. Install `nodejs`:

```
# sudo yum install nodejs
Plugins carregados: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.ufscar.br
 * extras: mirror.ufscar.br
 * updates: mirror.ufscar.br
nodesource                                                                                                                                         | 2.5 kB  00:00:00
nodesource/x86_64/primary_db                                                                                                                       |  55 kB  00:00:01
Resolvendo dependências
--> Executando verificação da transação
---> O pacote nodejs.x86_64 2:10.24.1-1nodesource será instalado
--> Resolução de dependências finalizada

Dependências resolvidas

==========================================================================================================================================================================
 Package                             Arq.                                Versão                                             Repo                                     Tam.
==========================================================================================================================================================================
Instalando:
 nodejs                              x86_64                              2:10.24.1-1nodesource                              nodesource                               20 M

Resumo da transação
==========================================================================================================================================================================
Instalar  1 Package
```

1.4. Install some dependencies:

```
 /src $ npm install
npm WARN jornadakubernetes@1.0.0 No description
npm WARN jornadakubernetes@1.0.0 No repository field.

added 121 packages from 199 contributors and audited 121 packages in 4.13s

3 packages are looking for funding
  run `npm fund` for details

found 1 moderate severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
```

1.5. Run the application:

```
/src $ node app.js
(node:21496) DeprecationWarning: current URL string parser is deprecated, and will be removed in a future version. To use the new parser, pass option { useNewUrlParser: true } to MongoClient.connect.
(node:21496) [MONGODB DRIVER] Warning: Current Server Discovery and Monitoring engine is deprecated, and will be removed in a future version. To use the new Server Discover and Monitoring engine, pass option { useUnifiedTopology: true } to the MongoClient constructor.
Servidor rodando na porta 8080
```

- To access the application, make sure you have Docker IP and putin your browser `http://192.168.1.128:8080/api-docs/`

![nodejsapp](images/nodejs.png)

### `Using the NodeJS application to run on container`

1.1. Now we will work using `Docker images` befores some concepts to start:

- Container image: basically a template for our container
-  How to buid your Docker image? There are two ways to buid:
```
a. Docker commit (image created via container - wrong way to create - several disvantages)
b. Dockerfile (file to add a image following basic steps, seems a cake recipe)
```

1.2. Let's create our first `Dockerfile` using a `Node image` from Dockerhub:

- First of all, check on Dockerhub the [node image](https://hub.docker.com/_/node).

1.3. Let's create our `Dockerfile` as follow below:

```dockerfile
FROM node
WORKDIR /app
COPY ./package*.json ./
RUN npm install
COPY ..
CMD ["node", "app.js"]
```
1.4. Let's create our Docker image based on `Dockerfile`:

```
$ docker build -t
```
