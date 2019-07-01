Docker-compose yaml file for [CQPweb](http://cwb.sourceforge.net/cqpweb.php), starting from the dockerfile by [egon w. stemle](https://gitlab.inf.unibz.it/commul/docker/teitok) from UniBozen (Sud Tirol, Europe).

From CQPweb's site: 
>CQPweb is a web-based graphical user interface (GUI) for some elements of the CWB - and in particular, the CQP query processor. Thus the name.

>CQPweb is designed to replicate the user-interface of the popular BNCweb tool, which also uses CQP as a back-end. Like BNCweb, CQPweb uses a database alongside the CWB to provide extra functions beyond those built into CWB/CQP. However, unlike BNCweb, CQPweb can be used with any corpus.

>CQPweb is especially suitable for students, non-linguists, and others for whom a Unix-like command-line is a terrifying prospect.

## Features
- separate containers for SQL and CQPweb sub-systems
- automatic building of CWB tools (latest version)
- (quasi)automatic installation of CQPweb GUI (latest version)
- basic installation of R
- MariaDB's latest version (straight from the original Docker image)
- persistent volumes

## Installation
1. Edit .env and secrets.env to fit your needs (see below for details).
2. docker-compose build
3. docker-compose up
4. When the system pops up "Ok, now you should run /var/www/html/CQPweb/bin/autosetup.php in order to finish the autosetup", run docker-compose exec cqpweb bash to enter CQPweb container terminal
5. Run autosetup.php from its directory
6. Visit http://youraddress/CQPweb
7. Drop your VRT files to ./${FIRSTNAME}-container/cqpweb/upload
8. Happy corpus linguistics!

Next time(s) you will just have to run docker-compose up -d 

## Persistent volumes
In order to mantain corpora data and registry, CQPweb files and SQL database through further code upgrades, the following persistent volume are provided:

| Host | Container:directory | Description & Usage |
| ------ | ----- | ------------------- |
| ./${FIRSTNAME}-container/cwb/corpora | CQPweb:/var/corpora | corpora data |
| ./${FIRSTNAME}-container/cwb/registry | CQPweb:/usr/local/share/cwb/registry | corpora registry files |
| ./${FIRSTNAME}-container/cqpweb/data | CQPweb:/var/www/html/CQPweb | CQPweb data files |
| ./${FIRSTNAME}-container/cqpweb/upload | CQPweb:/var/cqpweb/upload | CQPweb upload directory |
| ./${FIRSTNAME}-container/cqpweb/tmp | CQPweb:/var/cqpweb/tmp| CQPweb temporary directory |




