---
archetype: "home"
title: "Karnak"
description: "Karnak is a DICOM gateway for data de-identification and DICOM attribute normalization."
keywords: [ "dicom gateway", "de-identification", "tag morphing", "dicom proxy" ]
---

Karnak is a DICOM gateway for data de-identification and DICOM attribute normalization. It is distributed as a containerized application.

![Karnak gateway](/images/karnak-gateway.png)

Karnak manages a continuous DICOM flow with a DICOM listener as input and a DICOM and/or DICOMWeb as output.

## Key Features

*   **DICOM Gateway**: Acts as a proxy between DICOM modalities/workstations and PACS/Archives.
*   **Flexible Connectivity**: Supports standard DICOM protocols (C-STORE) and DICOMWeb (STOW-RS).
*   **De-identification**: Robust de-identification based on DICOM standards (Basic Profile) with configurable actions.
*   **Clean pixels**: Remove burned-in annotations from images to ensure patient privacy.
*   **Pseudonymization**: Apply pseudonyms from DICOM fields, by importing CSV, or with external web services.
*   **Tag Morphing**: Normalizes DICOM attributes to ensure consistency across your workflow.
*   **Extensible Profiles**: Customizable de-identification and tag morphing profiles using YAML files.
*   **Conditional Processing**: Apply de-identification and tag morphing rules based on customizable conditions.
*   **Notification System**: Email notifications for successful transfers and errors.
*   **Project Management**: Organize de-identification rules and secrets by project.
*   **Web Interface**: User-friendly web portal for configuration and monitoring.
*   **Portable Distribution**: Run Karnak as a portable application without installation.

[Source Code on GitHub](https://github.com/OsiriX-Foundation/karnak)

