---
layout: default
title: KHEOPS
parent: User guide
nav_order: 5
permalink: /docs/userguide/kheops
---

# KHEOPS

## Create a destination album

To create a KHEOPS album as a destination you must use:

* Protocol: STOW
* DICOM endpoint: /api/studies

First create the album destination, please refer to the official documentation of KHEOPS to [create a new album](https://docs.kheops.online/docs/albums/new_album).

![New token](resources/kheops_newtoken.png)

![New token](resources/kheops_newtoken_1.png)

![New token](resources/kheops_newtoken_2.png)

1 Create a new token

2 Give **WRITE** permission to the token

3 Set a token expiration date

4 Copy the token value to be used in the header of your KARNAK destination

Below is an example of creating headers with the token value:

```
<key>Authorization</key>
<value>Bearer y8KlxLhhq8yeEPpOHkhbHg</value>
```

## Switching in different KHEOPS albums

KARNAK offre la possibilité de splitter des dicom dans différents album KHEOPS. Le principe est le suivant, toutes les données seront envoyées dans un album commun principale. Ensuite selon vos conditions, ces études seront partagées à des albums.

Ce partage peut avoir lieu uniquement au seins d'une instance KHEOPS. Le partage entre différent KHEOPS n'est pas possible 