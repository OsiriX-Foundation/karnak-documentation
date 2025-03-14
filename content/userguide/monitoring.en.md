---
title: Monitoring
weight: 60
description: Log of the completed or ongoing transfers
---

![monitoring view](/userguide/monitoring_main.png)

Monitoring view is used to follow transfers in progress or recent transfers.

Currently limited to 150000 transfers, it is possible to browse the different pages via the navigation bar at the bottom center of the view

An automatic cleaning occurs on the monitoring tables, if the limit is exceeded.

The view displays the most recent transfers and is ordered with most recent first.

Filters are available to easily browse or find a transfer.

It is possible to export the list of transfers via the export functionality in csv format depending on the filters selected.

By clicking on a transfer, its details are displayed containing the de-identified and original information.

### 1. Filters

![monitoring filters](/userguide/monitoring_filters.png)

Filters allow to easily search and browse transfers.
It is possible to filter by:
* Date/Time of transfers
* Study UID (original or de-identified) 
* Serie UID (original or de-identified)
* Sop Instance UID (original or de-identified)
* Status of the transfer (not sent, sent, all, excluded and error) 

The status of a transfer is defined as follows :
* if the instance was successfully sent, it appears with the label "Sent" in green
* if the instance was not sent because of the SOP filter, the destination condition or the use of ExcludeInstance() in the profile, it appears in white with a label corresponding to the reason why it was excluded
* if the instance was not sent because an unexpected error occurred, it will appear in red with a label corresponding to the error that occurred

![Transfer statuses](/userguide/monitoring_statuses.png)

### 2. Browsing

![monitoring browsing](/userguide/monitoring_browsing.png)

It is possible to go through the different pages of the list of transfers by using the navigation bar.

### 3. Refresh

![monitoring refresh](/userguide/monitoring_refresh.png)

By clicking on the refresh button, the list of transfers is updated with last transfers.

### 4. Export

![monitoring export settings](/userguide/monitoring_export_settings.png)

Export settings allow to customize the csv before exporting it. It is possible to change the csv delimiter and the quote character of the file.

![monitoring export button](/userguide/monitoring_export_button.png)

Once customized, the export is launched by clicking on the export button.

![monitoring export csv](/userguide/monitoring_export_csv.png)

Export will use the filters selected in the monitoring view and export only transfers matching the filters' criteria. 