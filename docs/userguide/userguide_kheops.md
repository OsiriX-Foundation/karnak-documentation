---
layout: default
title: KHEOPS
parent: User guide
nav_order: 5
permalink: /docs/userguide/kheops
---

# KHEOPS

## Create a destination album

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