---
layout: default
title: Installation
nav_order: 4
permalink: /docs/installation
---

# Launch Karnak

Karnak has been tested with [docker](https://docs.docker.com/install/) **19.03** and [docker-compose](https://docs.docker.com/compose/install/) **1.22**.

1. Download [karnak](https://github.com/OsiriX-Foundation/karnak-docker) repository
2. Execute `generateSecrets.sh` to generate the secrets required by Karnak
3. Adapt all the *.env files if necessary
4. Start docker-compose with [Docker commands](#docker-commands) (or [create docker-compose service](installation/service)) 

## Docker commands

Commands from the root of this repository.

* Update docker images ([version](https://hub.docker.com/r/osirixfoundation/karnak/tags) defined into .env): `docker-compose pull`
* Start a docker-compose: `docker-compose up -d`
* Stop a docker-compose: `docker-compose down`
* Stop and remove volume of a docker-compose (reset all the data): `docker-compose down -v`
* docker-compose logs: `docker-compose logs -f`
* Karnak's logs: `sudo docker exec -it CONTAINERID bash`     
`cd logs`

## Secrets

You can generate the secrets with the `generateSecrets.sh` script available at the root of the [karnak-docker repository](https://github.com/OsiriX-Foundation/karnak-docker) (adapt the script to your system if necessary).

Note: *These following secrets are stored in files and use the environment variables ending with _FILE (see 'Environment variables' below)*

Before starting docker-compose make sure that the `secrets` folder and the secrets defined in the [karnak-docker repository](https://github.com/OsiriX-Foundation/karnak-docker#secrets) exist

