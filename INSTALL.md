# CTMM Installation docs 

## Release package 

### Git repo 
#### Root 
https://github.com/keosys-lygature

#### Docker-compose configuration 
https://github.com/keosys-lygature/ctmm-ims-docker-compose

#### Launcher javascript
https://github.com/keosys-lygature/ctmm-ims-rma-web

### Binaries
#### linux 
* Docker containers 
* ims-apps-repository 
* ims-viewer-connector

## Installation guide  

### Launcher tools

The new Keosys launcher needs several services to be deployed in docker containers. This require docker and docker-compose installed on the host machine.

Once docker and docker-compose the following steps are required 

Import docker containers 

    $ docker load -i ims-apps-repository.tar
    $ docker load -i ims-viewer-connector.tar

Retrieve docker-compose script from git

    $ git clone https://github.com/keosys-lygature/ctmm-ims-docker-compose

Run docker containers 

    $ docker-compose up -d

The docker containers expose ports 42101 and 42102. These ports shall be mapped with the reverse proxy configuration of the Apache. 

### Web application server  

Apache configuration

The updated Keosys launcher comes with new set of javascript files. These files can be retrieved from the git repository 
https://github.com/keosys-lygature/ctmm-ims-rma-web

The repository shall be cloned and stored in the local file system available to the apache server.

    $ mkdir -p /data/keosys
    $ cd /data/keosys
    $ git clone https://github.com/keosys-lygature/ctmm-ims-rma-web

Once the sources are cloned, the apache configuration shall be updated in order to specify rewrite rules: 

    $ vi /etc/httpd/conf.d/nbia-proxy.conf

For both *.80 (http) and https (.443) protocols after `RewriteEngine On` specify

    RewriteRule /ncia/js/keosys/keoscript.js /data/keosys/ctmm-ims-rma-web/keoscript.js [L]
    RewriteRule /ncia/js/keosys/rma/KAgent.js /data/keosys/ctmm-ims-rma-web/rma/KAgent.js [L]
    RewriteRule /ncia/js/keosys/rma/KAgent.config.js /data/keosys/ctmm-ims-rma-web/rma/KAgent.config.js [L]

Then save and quit.

### CTMM Viewer update 

* Install KSWMIDB

* Install KSWMIAS
