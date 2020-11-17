---
layout: default
title: Installation
nav_order: 3
has_children: true
permalink: /docs/installation
---

# Launch Karnak

Karnak has been tested with [docker](https://docs.docker.com/install/) **19.03** and [docker-compose](https://docs.docker.com/compose/install/) **1.22**.

1. Execute `generateSecrets.sh` to generate the secrets required by Karnak
2. Adapt all the *.env files if necessary
3. Start docker-compose with commands (or [create docker-compose service](#create-docker-compose-service)) 

## Docker commands

Commands from the root of this repository.

* Update docker images ([version](https://hub.docker.com/r/osirixfoundation/karnak/tags) defined into .env): `docker-compose pull`
* Start a docker-compose: `docker-compose up -d`
* Stop a docker-compose: `docker-compose down`
* Stop and remove volume of a docker-compose (reset all the data): `docker-compose down -v`
* docker-compose logs: `docker-compose logs -f`
* Karnak's logs: `sudo docker exec -it CONTAINERID bash`     
`cd logs`

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

## Secrets

You can generate the secrets with the `generateSecrets.sh` script available at the root of this repository (adapt the script to your system if necessary).

Note: *These following secrets are stored in files and use the environment variables ending with _FILE (see 'Environment variables' below)*

Before starting docker-compose make sure that the secrets folder and the following secrets exist:
* `karnak_hmac_key`
* `karnak_login_password`
* `karnak_postgres_password`
* `mainzelliste_api_key`
* `mainzelliste_postgres_password`
* `mainzelliste_pid_k1`
* `mainzelliste_pid_k2`
* `mainzelliste_pid_k3`