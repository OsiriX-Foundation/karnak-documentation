---
title: Gateway
weight: 10
description: Gateway management in Karnak
---

## Forward Node

This page lists all the forward nodes configured in Karnak and allows to edit, create and delete nodes. The forward node contains the configuration of the DICOM node's Application Entity.

![gateway_page](/userguide/gateway_forwardnode.png)

##### 1. Creation of a forward node

After clicking on the New Forward Node button, a form will appear as illustrated below.

![New Forward Node](/userguide/gateway_new_forwardnode.gif)

To create a new forward node, the "Forward AETitle" input must be filled, then click on the "Add" button. Once added, it will appear in the list of forward nodes and will be selected. The Forward AETitle must be unique.

##### 2. Forward node list

This list displays all the forward nodes created in the Karnak instance. A forward node can be selected to view and manage its details on the right panel.

##### 3. Forward node parameters

The value of the forward AETitle and its description can be modified after creation on the detailed view of the forward node. To save your changes, click on the "Save" button.

##### 4. Forward node sources or destinations

In the forward node's details, source control can be configured, checking if the received DICOM is provided from a known source. Destinations can also be created and configured to distribute received DICOM instances. These destinations can communicate with the DICOM or DICOM WEB protocol.

![gateway destinations](/userguide/gateway_destinationspage.png)

###### 4.1 Navigation

Depending on the selected tab, the list of destinations or sources associated to this forward node will be displayed.

###### 4.2 Filtering

The destination's list filter is applied to the destination description.

The source's list filter is applied to the AE title, hostname and description source.

###### 4.3 List

All the destinations or sources associated to the forward node are displayed here.

Click on an element of the list will open its detailed view, also allowing its edition.

###### 4.4 Actions

The action buttons depend on the current tab selected.

In the Destinations view, two actions are displayed, corresponding to the protocol associated to the destination being created. The available protocols are either DICOM or DICOM WEB. The Destination creation is detailed in the section [Destinations](destinations).

In the Sources view, a button for creating a new Source is displayed. The Source creation is detailed in the [Sources](sources) page.

![Sources button](/userguide/gateway_sourcesbutton.png)

##### 5. Forward node actions

Three actions are available:

* Save: saves the changes made on the forward node parameters
* Delete: deletes the selected forward node
* Cancel: reverts the changes made on the forward node parameters
