## URLS
- [MEAN Blueprint Quick Start](https://docs.bitnami.com/aws/infrastructure/mean/get-started/get-started/)
- [Apache Virtual Hosts](https://httpd.apache.org/docs/2.4/vhosts/examples.html)

## Commands

`Forever` command for making our Node server into a daemon
```shell
$ forever start index.js
$ forever restart 0
$ forever restartall
```

Reconfigure Apache server
```shell
$ # List virtual hosts
$ apachectl -S 
$ # Check configuration has correct syntax
$ apachectl configtest 
$ # Restart Apache for reloading the new configuration
$ sudo /opt/bitnami/ctlscript.sh restart apache
```

Steps to follow
```shell
$ create MEAN project in light sail with ssh keys
$ Install and configure Filezilla
$ create new site in filezilla and configure filezilla
$ create a folder inside /opt/bitmani/<folder name>
$ copy paste that all files inside the created folder | or | clone project directly inside the /opt/bitnami/
$ cd <folder name>
$ npm install

$ # Demonize application to forever running
$ npm install -g forever
$ forever start index.js
$ forever restart 0
$ forever restartall

$ # what if we havemultiple folder structure with build files
$ npm install 
$ npm run build
$ forever start -c "npm start" .     //npm start depends what we have configured in package.json file

$ # to check the forever running
$ forever list

$ # to see forever logs
$ sudo nano /home/bitnami/.forever/<log UID>.log

$ # to see port is running or not 
$ curl http://localhost:8080

$ # to remove all the forever list
$ sudo rm -rf /home/bitnami/.forever

$ # Create vhost.conf to configure (before generating SSL certificate to domain) // only for backend

<VirtualHost *:80>
    ServerName <domain name backend>
    ProxyPass / http://localhost:8000/
    ProxyPassReverse / http://localhost:8000/
    # Other directives here
</VirtualHost>

$ # copy vhost.conf to apache server
$ cp vhost.conf /opt/bitnami/apache/conf/vhosts/

$ # move to this directory and make config test
$ cd /opt/bitnami/apache/conf
$ apachectl configtest

$ # httpd.conf is the main file for apache server
$ tail httpd.conf

$ # restart the apache server after configuration
$ sudo /opt/bitnami/ctlscript.sh restart apache

$ # second virtual host without ssl
<VirtualHost *:80>
    ServerName <domain name frontend>
    # Other directives here
</VirtualHost>

$ # Add IP to DNS Domain or sub domain (EX:- Route 53)
  -> create record
  -> configure lightsail instance static IP address
  -> # record type must be A-routes trafficto an IP4 address

$ # create SSl Certificate (It will generate free certificate)
$ sudo /opt/bitmani/bncert-tool
$ # it will as for domain name so configure domain names
$ Ex:- Domain list []: shivraj.com shivraipatil2.com
$ # it will ask do you want to add them mention no

$ # List virtual hosts
$ apachectl -S 

$ #Reconfigure vhost.conf
<VirtualHost *:443>
    ServerName shivraj.com
    SSLEngine on
    SSLCertificateFile "/opt/bitnami/apache/conf/shivraj.com.crt"
    SSLCertificateKeyFile "/opt/bitnami/apache/conf/shivraj.com.key"
    # Other directives here
</VirtualHost>

<VirtualHost *:443>
    ServerName shivraipatil2.com
    ProxyPass / http://localhost:8000/
    ProxyPassReverse / http://localhost:8000/
    SSLEngine on
    SSLCertificateFile "/opt/bitnami/apache/conf/shivraipatil2.com.crt"
    SSLCertificateKeyFile "/opt/bitnami/apache/conf/shivraipatil2.com.key"
    # Other directives here
</VirtualHost>

$ #after ading this file copy it into the apache server
$ cp vhost.conf /opt/bitnami/apache/conf/vhosts/

$ # make one config test
$ # restart apache server again
$ # apachectl -S  //to recahec apache sercver is configured according to port for SSl

$ # configure Frontend Ui
$ npm install
$ npm run build

$ # we have to copy our build file inside the htdocs
$ cp -r build/* ~/htdocs/


To Remove all PIDS Running related to node
pgrep node | xargs kill -9
pgrep node
ps aux | grep node
lsof -i :8080

```
