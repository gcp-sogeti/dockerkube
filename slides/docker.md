## Docker

----

### Le challenge

<img src="img/the-challenge.png" style="background:none; border:none; box-shadow:none;"/>

----

### La matrice de l'enfer

<img src="img/the-matrix-from-hell.png" style="background:none; border:none; box-shadow:none;"/>

----

### Le transport par cargo avant 1960

<img src="img/cargo-transport-pre-1960.png" style="background:none; border:none; box-shadow:none;"/>

----

### Aussi une matrice de l'enfer

<img src="img/also-a-matrix-from-hell.png" style="background:none; border:none; box-shadow:none;"/>

----

### La solution : le conteneur intermodal

<img src="img/intermodal-shipping-container.png" style="background:none; border:none; box-shadow:none;"/>

----

### Docker est un conteneur pour le code

<img src="img/shipping-container-for-code.png" style="background:none; border:none; box-shadow:none;"/>

----

### Docker résoud la matrice de l'enfer

<img src="img/eliminates-matrix-from-hell.png" style="background:none; border:none; box-shadow:none;" width="80%"/>

----

### Un peu d'histoire

* [IBM VM/370 (1972)](https://en.wikipedia.org/wiki/VM_%28operating_system%29)

* [Linux VServers (2001)](http://www.solucorp.qc.ca/changes.hc?projet=vserver)

* [Solaris Containers (2004)](https://en.wikipedia.org/wiki/Solaris_Containers)

* [FreeBSD jails (1999-2000)](https://www.freebsd.org/cgi/man.cgi?query=jail&sektion=8&manpath=FreeBSD+4.0-RELEASE)

Les containers sont la depuis *très longtemps*

----

### Containers vs. VMs

<img src="img/containers-vs-vms.png" style="background:none; border:none; box-shadow:none;"/>

----

### Pourquoi les containers sont légers ?

<img src="img/why-are-containers-lightweight.png" style="background:none; border:none; box-shadow:none;"/>

----

### Docker - Worflow

<img src="img/basics-of-docker-system.png" style="background:none; border:none; box-shadow:none;"/>

----

### Cgroups (control groups)

cgroups (control groups) est une fonctionnalité du noyau Linux pour limiter, compter et isoler l'utilisation des ressources (processeur, mémoire, utilisation disque, etc.)

----

### Cgroups (control groups)

<img src="img/cgroups.png" style="background:none; border:none; box-shadow:none;"/>

[more on cgroups](https://mairin.wordpress.com/2011/05/13/ideas-for-a-cgroups-ui/)

----

### Cgroups - multiple cgroups

<img src="img/cgroups-usage.png" style="background:none; border:none; box-shadow:none;"/>

[more on cgroups](https://mairin.wordpress.com/2011/05/13/ideas-for-a-cgroups-ui/)

----

### Namespaces

- PID
  - Fournit l'isolation pour l'allocation des identifiants de processus (PIDs), la liste des processus et de leurs détails
- Network
  - Isole le contrôleur de l'interface réseau (physique ou virtuel), les règles de pare-feu iptables, les tables de routage, etc.
- IPC
  - Isole le System V de communication inter-processus entre les espaces de nommage

----

### Namespaces

- Mount
  - Permet de créer différents modèles de systèmes de fichiers, ou de créer certains points de montage en lecture-seule
- UTS
  - Permet le changement de nom d'hôte
- User
  - Isole les privilèges et sépare l'identification des utilisateurs dans plusieurs ensembles de processus

----

### Namespaces

<img src="img/namespaces.png" style="background:none; border:none; box-shadow:none;" width="60%" />

----

### Layer

<img src="img/container-layers.jpg" style="background:none; border:none; box-shadow:none;"/>

----

### Image

Une image est un système de fichier en lecture seule

```bash
$ docker images

REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
fedora          latest    ddd5c9c1d0f2   3 days ago     204.7 MB
centos          latest    d0e7f81ca65c   3 days ago     196.6 MB
ubuntu          latest    07c86167cdc4   4 days ago     188 MB
redis           latest    4f5f397d4b7c   5 days ago     177.6 MB
postgres        latest    afe2b5e1859b   5 days ago     264.5 MB
alpine          latest    70c557e50ed6   5 days ago     4.798 MB
debian          latest    f50f9524513f   6 days ago     125.1 MB
busybox         latest    3240943c9ea3   2 weeks ago    1.114 MB
```

----

### Container

Un container est un ensemble de processus isolés qui s'exécute sur ce système de fichier

```bash
$ docker ps

CONTAINER ID        IMAGE                        COMMAND                CREATED              STATUS              PORTS               NAMES
4c01db0b339c        ubuntu:12.04                 bash                   17 seconds ago       Up 16 seconds       3300-3310/tcp       webapp
d7886598dbe2        crosbymichael/redis:latest   /redis-server --dir    33 minutes ago       Up 33 minutes       6379/tcp            redis,webapp/db
```

----

### Création d'une image

Un Dockerfile est un fichier qui contient toutes les inscriptions nécessaires pour créer une image

```dockerfile
FROM busybox
RUN ls -lh /
CMD echo Hello world
```

----

### Docker build

Le fichier Dockerfile est interprêté par la commande docker build

```bash
$ docker build .

Uploading context 10240 bytes
Step 1/3 : FROM busybox
Pulling repository busybox
 ---> e9aa60c60128MB/2.284 MB (100%) endpoint: https://cdn-registry-1.docker.io/v1/
Step 2/3 : RUN ls -lh /
 ---> Running in 9c9e81692ae9
total 24
drwxr-xr-x    2 root     root        4.0K Mar 12  2013 bin
drwxr-xr-x    5 root     root        4.0K Oct 19 00:19 dev
drwxr-xr-x    2 root     root        4.0K Oct 19 00:19 etc
drwxr-xr-x    2 root     root        4.0K Nov 15 23:34 lib
lrwxrwxrwx    1 root     root           3 Mar 12  2013 lib64 -> lib
dr-xr-xr-x  116 root     root           0 Nov 15 23:34 proc
lrwxrwxrwx    1 root     root           3 Mar 12  2013 sbin -> bin
dr-xr-xr-x   13 root     root           0 Nov 15 23:34 sys
drwxr-xr-x    2 root     root        4.0K Mar 12  2013 tmp
drwxr-xr-x    2 root     root        4.0K Nov 15 23:34 usr
 ---> b35f4035db3f
Step 3/3 : CMD echo Hello world
 ---> Running in 02071fceb21b
 ---> f52f38b7823e
Successfully built f52f38b7823e
Removing intermediate container 9c9e81692ae9
Removing intermediate container 02071fceb21b
```

----

### Démarrer un container

```bash
$ docker run --name test -it debian bash

Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
05d1a5232b46: Pull complete 
Digest: sha256:07fe888a6090482fc6e930c1282d1edf67998a39a09a0b339242fbfa2b602fff
Status: Downloaded newer image for debian:latest
root@d6c0fe130dba:/# exit 13
```

```bash
$ echo $?
13
```

```bash
$ docker ps -a | grep test
d6c0fe130dba        debian:7            "/bin/bash"         26 seconds ago      Exited (13) 17 seconds ago                         test
```

----

### Détacher un container

```bash
$ docker run --name test -d debian

77b89ea9ce3537dff386149cb2cb51dc482413a19f8106b11814a66143d8360b
```

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
77b89ea9ce35        debian              "bash"              54 seconds ago      Exited (0) 53 seconds ago                       test
```

----

### Les variables d'environnement

Les variables d'environnements servent à paramétrer un container, par exemple pour mettre en place une chaine de connexion, ou un paramétrage particulier

```bash
$ docker run -d \
    -e POSTGRES_ENV_POSTGRES_USER='bar' \
    -e POSTGRES_ENV_POSTGRES_PASSWORD='foo' \
    postgres
```

----

### Le réseau

Pour accéder au service tournant dans le container, il faut passer mapper le port de la machine avec le service exposé

```bash
$ docker run --name nginx -p 8080:80 -d nginx
```

----

### La persistance

Pour pouvoir persister des données provenant des containers, il est nécessaire de passer par des volumes

```bash
$ docker run -d --name devtest -v myvol2:/app nginx:latest
```

```bash
"Mounts": [
    {
        "Type": "volume",
        "Name": "myvol2",
        "Source": "/var/lib/docker/volumes/myvol2/_data",
        "Destination": "/app",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
]
```

----

### Les fichiers de log

Il est souvent nécessaire de voir ce qui se passe dans le container, notamment à des fins de monitoring et de debug

Par défaut, Docker écrit sur la sortie standard STDOUT

```bash
$ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
```

```bash
$ date
Tue 14 Nov 2017 16:40:00 CET
```

```bash
$ docker logs -f --until=2s
Tue 14 Nov 2017 16:40:00 CET
Tue 14 Nov 2017 16:40:01 CET
Tue 14 Nov 2017 16:40:02 CET
```

----

### Docker compose

Pour simplifier toutes les notions vues précédemment, Docker utilise un fichier YAML, ainsi qu'un outil afin de décrire une stack logicielle complète

```yaml
version: '3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
```

----

### Quelques cas d'usage
#### CI/CD
<img src="http://codethebuild.github.io/slides/images/build-env-jenkins-containers.png" style="background:none; border:none; box-shadow:none;"/>

----

### Quelques cas d'usage
#### Microservices
<img src="https://github.com/dockersamples/example-voting-app/blob/main/architecture.excalidraw.png" style="background:none; border:none; box-shadow:none;" width="70%" />
