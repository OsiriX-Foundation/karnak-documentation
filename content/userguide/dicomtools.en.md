---
title: DICOM tools
weight: 70
description: DICOM tools for checking the connection to a DICOM server
---

This page offers utility tools related to DICOM:
* test the availability of a DICOM node through DICOM Echo
* visualize worklists
* test standard and proprietary PACS services

The different modules are accessed through the tabs at the top of the page.

![DICOM Tools main page](/userguide/dicomtools_mainpage.png)

Those functionalities will be presented in details below.

### DICOM Echo tab

This tool tests the DICOM communication between 2 AETs.

The field "Calling AETitle" defines the identity of the calling DICOM entity. The field "Called AETitle" defines the identity of the DICOM entity to test. The field "Called Hostname" and "Called Port" define the hostname and port of the DICOM node to test.

All the fields are mandatory to execute the ECHO test. 

Clicking on the button "Echo" will launch the connectivity test and DICOM Echo command. Below is an example of a successful test.

![DICOM Tools Echo success](/userguide/dicomtools_successecho.png)

In this example, the "Called AETitle" value is purposely wrong, we can see that the network connectivity test is successful but not the DICOM Echo, displaying the reason of the failure.

![DICOM Tools Echo error](/userguide/dicomtools_echoerror.png)

#### Select Node Utility

The Select Node popup can be displayed by clicking on the "Select Node" button. 

![DICOM Tools Select Node](/userguide/dicomtools_selectnode.png)

The type of DICOM Node is set in the "Dicom Nodes Type" field. The list of values in the "Dicom Node" field is then updated according to the selected node type. The values can be filtered in this field by typing directly a string that will be matched against the AET, hostname, port or description.

Clicking on the "Select" button will automatically fill the Called AETitle, Called Hostname and Called Port fields with the values of the selected DICOM node.

![DICOM Tools Select Node](/userguide/dicomtools_selectnode_result.png)

### DICOM Worklist tab

This tool retrieves the content of a DICOM Worklist and tests their behavior and connectivity.

![DICOM Tools Worklist main page](/userguide/dicomtools_worklist.png)

###### Worklist Configuration
The field "Calling AETitle" defines the identity of the calling DICOM entity. The field "Worklist AET" defines the identity of the worklist to test. The field "Worklist Hostname" and "Worklist Port" define the hostname and port of the worklist to test.

All the fields are mandatory to retrieve the worklist content.

###### Worklist Query

Additionally, some fields are defined to filter the results returned by the worklist. These fields are optional. If none are filled, no filters are applied and all the elements of the worklist are retrieved.

###### Query Results

Clicking on the button "Select Worklist" will launch the test and data retrieval. The retrieved worklist data is displayed in a table, that can be sorted by column, by clicking on its header.

![Worklist success](/userguide/dicomtools_worklistsuccess.png)

#### Select Worklist Utility

The Select Worklist popup can be displayed by clicking on the "Select Worklist" button. 

![DICOM Tools Select worklist](/userguide/dicomtools_worklistpopup.png)

A list of available worklist will be displayed and a node can be selected. The worklists can be filtered in this field by typing directly a string that will be matched against the AET, hostname, port or description.

Clicking on the "Select" button will automatically fill the Worklist AET, Worklist Hostname and Worklist Port fields with the values of the selected DICOM worklist.

![DICOM Tools Select worklist result](/userguide/dicomtools_worklist_result.png)

### Monitor tab

This tool checks the status of DICOM nodes using both DICOM and WADO protocols.

![DICOM Tools Monitor](/userguide/dicomtools_monitor.png)

#### DICOM ECHO

A DICOM node must be selected in the list, and the button "Check" can be clicked to launch the DICOM Echo test for that node. Below is an example of a successful DICOM Echo test.

![Monitor DICOM Echo](/userguide/dicomtools_monitorecho.png)

#### WADO

A DICOM node must be selected in the list, and the button "Check" can be clicked to launch the WADO test for that node. Below is an example of a successful WADO test.

![Monitor DICOM WADO](/userguide/dicomtools_monitorwado.png)