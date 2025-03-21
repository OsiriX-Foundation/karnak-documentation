---
title: Installation
weight: 5
description: Installation of the Karnak DICOM gateway
---

Minimum docker version required: 20.10

1. Download [karnak](https://github.com/OsiriX-Foundation/karnak-docker) repository
2. Execute `generateSecrets.sh` to generate the secrets required by Karnak
3. Adapt all the *.env files if necessary
4. Start docker-compose with [Docker commands](#docker-commands) (or [create docker-compose service](installation/service)) 

## Docker commands

Commands from the root of this repository.

* Update docker images ([version](https://hub.docker.com/r/osirixfoundation/karnak/tags) defined into .env): `docker compose pull`
* Start: `docker compose up -d`
* Stop: `docker compose down`
* Stop and remove volume (reset all the data): `docker compose down -v`
* docker-compose logs: `docker compose logs -f`

## Secrets

You can generate the secrets with the `generateSecrets.sh` script available at the root of the [karnak-docker repository](https://github.com/OsiriX-Foundation/karnak-docker) (adapt the script to your system if necessary).

Note: *These following secrets are stored in files and use the environment variables ending with _FILE (see 'Environment variables' below)*.

Before starting docker-compose make sure that the secrets folder and the following secrets exist:
* `karnak_login_password`
* `karnak_postgres_password`

## Other installation pages

{{% children description="true" %}}