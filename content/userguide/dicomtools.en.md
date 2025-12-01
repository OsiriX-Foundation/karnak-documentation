---
title: DICOM Tools
weight: 70
description: DICOM tools for testing connectivity and querying DICOM servers
---

This page provides an overview of utility tools available in the DICOM Tools module, designed to assist with connectivity testing, querying, and status monitoring of DICOM servers. These tools enable users to:

- Test DICOM node availability using **DICOM Echo**
- Query a **DICOM Worklist** and display results
- **Monitor** multiple DICOM nodes using DICOM Echo and WADO protocols

The different modules are accessed through tabs at the top of the page.

> [!INFO]
> Currently, the persistence of DICOM nodes and worklists is not supported. Configured nodes and worklists come from [CSV files](https://github.com/OsiriX-Foundation/karnak/tree/master/src/main/resources/config) that are loaded when the Karnak server starts.

## DICOM Echo

This tool tests DICOM communication between two Application Entity Titles (AETs).

![DICOM Tools main page](/userguide/dicomtools_mainpage.png)

### Configuration Fields

The following fields must be configured to execute an Echo test:

| Field | Description | Required |
|-------|-------------|----------|
| **Calling AETitle** | Identity of the calling DICOM entity | Yes |
| **Called AETitle** | Identity of the DICOM entity to test | Yes |
| **Called Hostname** | Hostname or IP address of the DICOM node | Yes |
| **Called Port** | Port number of the DICOM node | Yes |

All fields are mandatory to execute the Echo test.

### Running the Test

Click the **Echo** button to launch the connectivity test and DICOM Echo command.

#### Successful Test

Below is an example of a successful DICOM Echo test:

![DICOM Tools Echo success](/userguide/dicomtools_successecho.png)

#### Failed Test

In this example, the "Called AETitle" value is intentionally incorrect. The network connectivity test succeeds, but the DICOM Echo fails, displaying the reason for the failure:

![DICOM Tools Echo error](/userguide/dicomtools_echoerror.png)

### Select Node Utility

The **Select Node** popup simplifies configuration by allowing you to choose from pre-configured DICOM nodes.

Click the **Select Node** button to open the popup:

![DICOM Tools Select Node](/userguide/dicomtools_selectnode.png)

**Configuration:**

1. Choose the node type in the **DICOM Nodes Type** field
2. The **DICOM Node** dropdown updates automatically based on the selected type
3. Filter nodes by typing in the field (searches AET, hostname, port, or description)
4. Click **Select** to auto-fill the Called AETitle, Called Hostname, and Called Port fields

![DICOM Tools Select Node result](/userguide/dicomtools_selectnode_result.png)

## DICOM Worklist

This tool retrieves and displays the content of a DICOM Worklist to test connectivity and data retrieval.

![DICOM Tools Worklist main page](/userguide/dicomtools_worklist.png)

### Worklist Configuration

Configure the following fields to connect to a worklist:

| Field | Description | Required |
|-------|-------------|----------|
| **Calling AETitle** | Identity of the calling DICOM entity | Yes |
| **Worklist AET** | Identity of the worklist to query | Yes |
| **Worklist Hostname** | Hostname or IP address of the worklist | Yes |
| **Worklist Port** | Port number of the worklist | Yes |

All fields are mandatory to retrieve worklist content.

### Worklist Query Filters

Additional optional fields are available to filter the results returned by the worklist:

- If no filters are specified, all worklist entries are retrieved
- Multiple filters can be combined to narrow results

### Query Results

Click the **Query Worklist** button to execute the test and retrieve data.

Retrieved worklist data is displayed in a sortable table. Click any column header to sort by that column:

![Worklist success](/userguide/dicomtools_worklistsuccess.png)

### Select Worklist Utility

The **Select Worklist** popup allows you to choose from pre-configured worklists.

Click the **Select Worklist** button to open the popup:

![DICOM Tools Select worklist](/userguide/dicomtools_worklistpopup.png)

**Configuration:**

1. Browse the list of available worklists
2. Filter by typing in the search field (searches AET, hostname, port, or description)
3. Click **Select** to auto-fill the Worklist AET, Worklist Hostname, and Worklist Port fields

![DICOM Tools Select worklist result](/userguide/dicomtools_worklist_result.png)

## Monitor

This tool monitors the status of DICOM nodes using both DICOM and WADO protocols, allowing you to verify connectivity and service availability.

### DICOM Echo Monitoring

**To test a DICOM node:**

1. Select a DICOM node from the list
2. Click the **Check** button to launch the DICOM Echo test

Below is an example of a successful DICOM Echo monitoring test:

![Monitor DICOM Echo](/userguide/dicomtools_monitorecho.png)

### WADO Monitoring

**To test WADO connectivity:**

1. Select a DICOM node from the list
2. Navigate to the **WADO** tab
3. Click the **Check** button to launch the WADO test

Below is an example of a successful WADO test:

![Monitor DICOM WADO](/userguide/dicomtools_monitorwado.png)

> [!INFO]
> The Monitor tool is useful for regular health checks of your DICOM infrastructure and can help identify connectivity issues before they impact production workflows.