---
layout: default
title: KARNAK service
parent: Installation
nav_order: 1
permalink: /docs/installation/service
baselevel: 2

---

## Create docker-compose service

Example of systemd service configuration with a docker-compose.yml file in the folder /opt/karnak (If it's another directory you have to adapt the script).

Instructions:

* Go to /etc/systemd/system
* Create the file ( eg: $ sudo touch karnak.service )
* Copy and paste the config below (eg: $ sudo nano karnak.service):

~~~
# /etc/systemd/system/karnak.service 

#########################
#    KARNAK             #
#    SERVICE            #	
##########################

[Unit]
Description=Docker Compose KARNAK Service
Requires=docker.service
After=docker.service network.target

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/karnak
ExecStart=/usr/local/bin/docker-compose up -d
ExecStop=/usr/local/bin/docker-compose down
TimeoutStartSec=0

[Install]
WantedBy=multi-user.target
~~~

Test the service:

* $ systemctl start karnak.service
* $ systemctl status karnak.service
* $ systemctl enable karnak.service