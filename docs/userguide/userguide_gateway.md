---
layout: default
title: Gateway
parent: User guide
nav_order: 1
permalink: /docs/userguide/gateway

---

# Gateway
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Forward Node

This page allows the creation of a forward node and its edition. All the forward nodes present in your KARNAK are listed here. The forward node will configure the Application Entity of your DICOM node.

![gateway_page](resources/gateway_forwardnode.png)

### 1. Creation of a forward node

Once clicked on the button "New forward node", a form will appear, as below, instead of the "New forward node" button (It will replace it).

![New Forward Node](resources/gateway_new_forwardnode.gif)

To create a new forward node, you have to complete the "Forward AETitle" input and click on the "Add" button. Once added, it will appear in the list of forward nodes and will be selected. A Forward AETitle must be unique.

### 2. Forward node list

This list displays all the forward nodes created in your KARNAK instance. You can select a forward node to view and manage its details on the right panel.

### 3. Forward node inputs

You can directly modify the value of the forward AETitle and its description. To save your changes, you have to click on the "Save" action button.

### 4. Forward node sources or destinations

In an Application Entity, you can add source control, which checks if the received DICOM is provided from a known source. You can also create one or more destinations to distribute received DICOM instance. These destinations can communicate with the DICOM or DICOM WEB protocol.

![gateway destinations](resources/gateway_destinationspage.png)

#### 4.1 Sources or destinations navigation

Depending on the navigation selected, between sources and destinations, the list and the action buttons will change.

#### 4.2 Filter in destinations or sources list

The destination filter will use the destination description.

The source filter will use the AE title, hostname and description source.

#### 4.3 Destination or sources list

All the destinations or sources, depending on the navigation selected, associated to the forward node is listed here.

You can click on a destination or source present in this list to view details or to modify it.

#### 4.4 Action buttons

The action buttons will change depending the navigation selected.

For a destination, these buttons defined the protocol you can use to create a destination. The protocol can be DICOM or DICOM WEB. The details for the destination creation will be explained below in the section [Destinations](#destinations).

If you click on the DICOM button, the DICOM destination creation page will appear.

If you click on the STOW button, the DICOM WEB destination creation page will appear.

For a source, the illustration below show the button. The details for the source creation will be explained below in the section [Sources](#sources).

![Sources button](resources/gateway_sourcesbutton.png)

 If you click on the the Sources button, the source creation page will appear.

### 5. Action buttons

You have three action buttons available.

* "Save" will save your change apply on the forward node inputs.
* "Delete" will delete your forward node
* "Cancel" will cancel the changes applied on the forward node inputs.

## Destinations

### DICOM destination



## Sources

A source is used to perform control over the sending entity to the associated forward node. It will check if the received DICOM is provided from an AET known.

In the case that no source is defined, the verification does not take place and the forward node can receive data from any AET.

![Creation source](resources/source_main.png)

#### 1. Form input

All the fields are optional, excepted the AETitle, which is mandatory.

If the "Check the hostname" box is checked, KARNAK also checks the hostname.

#### 2. Action buttons

You have three action buttons available.

* "Save" will create a new source or update your modifications on the selected source.
* "Delete" will delete the selected source.
* "Cancel" will cancel the changes applied on the selected source. You will be redirect to the forward node.