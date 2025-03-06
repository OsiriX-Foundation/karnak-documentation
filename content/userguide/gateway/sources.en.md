---
title: Sources
weight: 20
description: Sources management in Karnak
---


## Sources

The Sources tab contains a list of configured sources for the forward node.

![Sources view](/userguide/source_view.png)

A source is used to perform control over the sending entity to the associated forward node. It checks if the received DICOM instance is provided from a known AET.

If no sources are defined, the forward node will receive DICOM instances from any AET.

![Creation source](/userguide/source_main.png)

The field AETitle is mandatory. 

The fields Description and Hostname are optional. If "Check the hostname" option is selected, the Hostname's field value will be also be used to filter DICOM instances based on the sending AET. 
