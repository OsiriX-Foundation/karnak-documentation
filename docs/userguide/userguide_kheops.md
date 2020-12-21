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

When you create a destination that points to a KHEOPS album, you can propagate your data to underlying albums.

This is useful when you want to send a cohort of studies to a research group for example, without sharing all of the album studies.

**Beware**, the study sharing between album, must be done only within the same KHEOPS. Studies cannot be shared between different KHEOPS instances, you should create one destination per KHEOPS instance.

The purpose of this functionality is to allow sending your data to a single destination and to use the KHEOPS API to propagate your data to different places without having to create a new destination.

The following illustration show a scenario of this functionality. The illustrated scenario allows you to send a DICOM data to KARNAK. KARNAK has a destination defined to send the data to a KHEOPS album (Album main). This means that this album will regroup all the data sent by KANRAK. To prevent researchers or end users from having access to all the data, the data will be shared in other albums according to defined conditions.

1. The DICOM data is send to KARNAK
2. KARNAK send the data to the album main in KHEOPS
3. The data will be shared in the album X and in the album Y

![Switching KHEOPS example](resources/switching_kheops_scenario.png)

### Create a switching KHEOPS album

To share your DICOM in different KHEOPS album, you must complete the following fields.

The destination is the album where the studies will be shared.

The source is the album main, where all studies are sent.

![Switching](resources/kheops_switching.png)

| Fields                     | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| Url API                    | The url of the KHEOPS API                                    |
| Valid token of destination | The token to write to the album destination. Need **WRITE** permission |
| Valid token of source      | The token to shared from the album source. Need **READ, SEND** permission |

The condition field will allow you to assign a condition to enable sharing to the destination.

