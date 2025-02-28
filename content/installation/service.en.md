---
title: Karnak as service
weight: 15
description: Create a systemd service for the Karnak DICOM gateway
---

## Create docker-compose service

Example of systemd service configuration with a docker-compose.yml file in the folder /opt/karnak (If it's another directory you have to adapt the script).

By default, Docker needs **root privileges** to be executed.

**Manage Docker as a non-root user**

For more details, the following commands are inspired by the [official Docker documentation](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user).

1. Create the `docker` group.

   ```bash
    sudo groupadd docker
   ```

2. Add your user to the `docker`group.

   ```bash
   sudo usermod -aG docker $USER
   ```

3. Activate the changes to groups.

   ```bash
   newgrp docker
   ```

4. Verify that you can run `docker` commands without `sudo`.

   ```bash
   docker run hello-world
   ```

**Specify User in the service**

In the `[Service]` section of the karnak.service (see below), it's possible to specify the user who will run the service.

```bash
User=root
```

### Create the service

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