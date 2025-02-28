---
title: Kheops
weight: 80
description: Send DICOM data to a Kheops album
---

## Create a destination album

To create a Kheops album as a destination you must use:

* Protocol: STOW
* DICOM endpoint: /api/studies

First create the album destination, please refer to the official documentation of Kheops to [create a new album](https://docs.kheops.online/docs/albums/new_album).

![New token](/userguide/kheops_newtoken.png)

![New token](/userguide/kheops_newtoken_1.png)

![New token](/userguide/kheops_newtoken_2.png)

1 Create a new token

2 Give **WRITE** permission to the token

3 Set a token expiration date

4 Copy the token value to be used in the header of your Karnak destination

Below is an example of creating headers with the token value:

```
<key>Authorization</key>
<value>Bearer y8KlxLhhq8yeEPpOHkhbHg</value>
```

## Switching in different Kheops albums

When you create a destination that points to a Kheops album, you can propagate your data to underlying albums.

This is useful when you want to send a cohort of studies to a research group for example, without sharing all of the album studies.

**Beware**, the study sharing between album, must be done only within the same Kheops. Studies cannot be shared between different Kheops instances, you should create one destination per Kheops instance.

The purpose of this functionality is to allow sending your data to a single destination and to use the Kheops API to propagate your data to different places without having to create a new destination.

The following illustration show a scenario of this functionality. The illustrated scenario allows you to send a DICOM data to Karnak. Karnak has a destination defined to send the data to a Kheops album (Album main). This means that this album will regroup all the data sent by KANRAK. To prevent researchers or end users from having access to all the data, the data will be shared in other albums according to defined conditions.

1. The DICOM data is send to Karnak
2. Karnak send the data to the album main in Kheops
3. The data will be shared in the album X and in the album Y

![Switching Kheops example](/userguide/switching_kheops_scenario.png)

### Create a switching Kheops album

To share your DICOM in different Kheops album, you must complete the following fields and **validate them by clicking on Add button**.

The destination is the album where the studies will be shared.

The source is the album main, where all studies are sent.

![Switching](/userguide/kheops_switching.png)

| Fields                       | Description                                                                                          |
|------------------------------|------------------------------------------------------------------------------------------------------|
| Url API                      | The url of the Kheops API                                                                            |
| Valid token of destination   | The token to write to the album destination. Need **WRITE** permission                               |
| Valid token of source        | The token to shared from the album source. Need **READ, SEND** (Sharing in the Kheops UI) permission |

The condition field will allow you to assign a condition to enable sharing to the destination.

The conditions syntax and usage is detailed in the [Conditions](../../profiles/conditions) page.
