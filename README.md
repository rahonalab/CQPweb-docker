Docker-compose yaml file for [CQPweb](http://cwb.sourceforge.net/cqpweb.php), starting from the dockerfile by [egon w. stemle](https://gitlab.inf.unibz.it/commul/docker/teitok) from UniBozen (Sud Tirol, Europe).

From CQPweb's site: 
>CQPweb is a web-based graphical user interface (GUI) for some elements of the CWB - and in particular, the CQP query processor. Thus the name.

>CQPweb is designed to replicate the user-interface of the popular BNCweb tool, which also uses CQP as a back-end. Like BNCweb, CQPweb uses a database alongside the CWB to provide extra functions beyond those built into CWB/CQP. However, unlike BNCweb, CQPweb can be used with any corpus.

>CQPweb is especially suitable for students, non-linguists, and others for whom a Unix-like command-line is a terrifying prospect.

## Features
- separate containers for MariaDB (sql) and CQPweb (cqpweb) sub-systems
- automatic building of CWB tools (latest version)
- (quasi)automatic installation of CQPweb GUI (latest version)
- basic installation of R (based on R Docker image)
- MariaDB's latest version (straight from the original Docker image)
- persistent volumes

## Installation
1. Edit .env.edit and secrets.env.edit to fit your needs (see below for details). Rename the former to .env and the latter to secrets.env
2. `docker-compose build`
3. `docker-compose up`
4. When the system pops up "Ok, now you should run /var/www/html/CQPweb/bin/autosetup.php in order to finish the autosetup", run `docker-compose exec cqpweb bash` to enter CQPweb container terminal
5. Run autosetup.php from its directory
6. Visit http://youraddress/CQPweb
7. Drop your VRT files to ./${FIRSTNAME}-container/cqpweb/upload
8. Happy corpus linguistics!

Next time(s) you will just have to run `docker-compose up -d`

## Configuration
Before starting, you have to modify two files: .env and secrets.env

### .env
This file is read by docker-compose: you first need to set up your domain (${DOMAINNAME}) and host name (${FIRSTNAME}). The latter is particurarly important, since persistent volumes on the host filesystem are created using this name (see persistent volumes). The ${CORPORA} variable defaults to `/var/corpora`, while the ${REGISTRY} variable to `/usr/local/share/cwb/registry`. Please make sure that these directories exist!

### secrets.env
This file is read by containers. You can safely ignore the Apache environment variables; just modify MySQL variables and LOCALTIME.

## Persistent volumes
In order to mantain corpora data and registry, CQPweb files and SQL database through further code upgrades, the following persistent volumes are provided:

| Host | Container:directory | Description & Usage |
| ------ | ----- | ------------------- |
| ${CORPORA} | cqpweb:/var/corpora | corpora data |
| ${REGISTRY} | cqpweb:/usr/local/share/cwb/registry | corpora registry files |
| ./${FIRSTNAME}-container/cqpweb/data | cqpweb:/var/www/html/CQPweb | CQPweb data files |
| ./${FIRSTNAME}-container/cqpweb/upload | cqpweb:/var/cqpweb/upload | CQPweb upload directory |
| ./${FIRSTNAME}-container/cqpweb/tmp | cqpweb:/var/cqpweb/tmp| CQPweb temporary directory |
| ./${FIRSTNAME}-container/sql/data | sql:/var/lib/mysql| MariaDB data |




