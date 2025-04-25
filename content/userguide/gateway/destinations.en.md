---
title: Destinations
weight: 10
description: Destinations management in Karnak
---

Depending on the associated protocol, two types of destinations can be created: [DICOM Destinations](#dicom-destination) or [STOW Destinations](#stow-destination) protocol.

### DICOM Destination

The following fields are required for the DICOM Destination creation:

* AETitle
* Hostname
* Port (Should be between 1 and 65535)

![Creation source](/userguide/destination_DICOM.png)

##### 1. Destination parameters

These fields define the destination Karnak should send the instances to.

Condition is an optional field that can contain an expression. If the condition is met, the destination will be activated. See [Conditions](../../../profiles/conditions) for more details.

The hostname and the port will be used to define the host in case the "Use AETitle destination" is not checked.

##### 2. Transfer Syntax

This field defines the transfer syntax used.

##### 3. Use AETitle destination

If "Use AETitle destination" is checked, the AETitle will be used as host.

##### 4. Notifications

If the notifications are activated, automatic emails are generated and sent by Karnak, summarizing successfully forwarded instances as well as errors.

![Notifications](/userguide/destination_notifications.png)

* List of emails: comma separated list of emails that the email notification will be sent to
* Error subject prefix: prefix of the email object an issue occurred. Default value: ERROR
* Subject pattern: pattern of the email object in [Java String Format](https://dzone.com/articles/java-string-format-examples). Default value: [Karnak Notification] %s %.30s
* Subject values: attribute values that can be injected in the subject pattern. The attributes that can be injected are: PatientID, StudyDescription, StudyDate, StudyInstanceUID. Default value: PatientID,StudyDescription
* Interval: interval in seconds for the notification generation. Once an instance is received, it will wait for the number of seconds defined and aggregate the information so that only one email is sent in that period of time. Default value: 45

##### 5. Tag morphing

To activate tag morphing, it is required to [create a new project](../projects#1-create-a-project) first. The tag morphing needs a project to be selected. The applied profile is displayed below with its version.

![Tag Morphing](/userguide/destination_tagmorphing.png)

##### 6. De-identification

{{% notice note %}} **Activate de-identification** and **Activate tag morphing** are mutually exclusive options. They cannot be both selected since they are incompatible with each other.{{% /notice %}}

To activate de-identification, it is required to [create a new project](../projects#1-create-a-project) first.

If a project does not exist, a popup will be displayed.

![Popup deidentificatio](/userguide/popup_deidentification.png)

* **Create a project** redirects to the page project. If this option is chosen, all previously made changes in the destination form will be lost

* **Continue** will close the pop-up and the de-identification will not be activated

If a project exists, de-identification can be configured as detailed below.

![Configure de-identification](/userguide/deidentification_activate.png)

**Project**

De-identification requires a project to be selected. The applied profile is displayed below with its version.

**Pseudonym Type**

The pseudonym types are detailed below:

* **Pseudonym is already stored in KARNAK**: queries the pseudonym stored in the internal cache as explained in [External Pseudonym](../extpseudo). In case a pseudonym is not returned by the cache, the transfer of the current DICOM instance is aborted.
* **Pseudonym is in a DICOM tag**: retrieves the pseudonym in the specified DICOM tag. It requires the tag number. It is possible to use the delimiter and position fields to split the content of the tag and retrieve a part of it. Otherwise, the entire tag's value is used as the pseudonym.

![Pseudonym is in a DICOM tag](/userguide/pseudonym_dicomtag.png)

**Issuer of Patient ID by default**

The value contained in this field is used when retrieving the pseudonym using the [External Pseudonym](../extpseudo). The pseudonym is queried based on the Patient ID and the Issuer of the Patient ID value.

##### 7. Authorized SOPs

This field defines a list of SOPs that are used to filter the DICOM instances. If this option is activated, the list can be filled using [DICOM SOPs values](http://dicom.nema.org/medical/Dicom/current/output/chtml/part04/sect_B.5.html). If this option is not activated, any SOPs will be transferred without restrictions.

![SOP Filter](/userguide/destination_SOPfilter.png)

##### 8. Enable the destination

This field allows a destination to be disabled, meaning that no DICOM instances will be transferred to that destination.

##### 9. Action buttons

Three actions are available:

* Save: saves the changes made on the destination
* Delete: deletes the selected destination
* Cancel: reverts the changes made on the destination

### STOW Destination

The URL is required for the STOW Destination creation.

Only the parts that differ from the DICOM Destination configuration will be detailed here. For more information on the parts not highlighted, please refer to the [DICOM Destinations](#dicom-destination). 

![Creation source](/userguide/destination_stow.png)

##### 1. Destination parameters

These fields define the destination Karnak should send the instances to.

Condition is an optional field that can contain an expression. If the condition is met, the destination will be activated. See [Conditions](../../../profiles/conditions) for more details.

The Headers field contains the HTTP headers that are added to the HTTP request. The headers are defined with key and value tags, see below for examples.

The button **Generate Authorization Header** displays a popup that will generate the headers in the correct format based on the Authorization type selected and the relevant information provided. If the header's value already contains `<key>Authorization</key>`, an error message will be displayed. If other header values are present, the Authorization header will be appended.

The following Authorization types are supported:

**Basic Auth**
<img src="/userguide/destination_basicauth.png" alt="drawing" width="80%"/>

Generated headers:
```
<key>Authorization</key>
<value>Basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
```

**OAuth 2**
<img src="/userguide/destination_oauth.png" alt="drawing" width="80%"/>

Generated headers:
```
<key>Authorization</key>
<value>Bearer 1234567890</value>
```

##### 2. Switching in different Kheops albums

If the destination is a Kheops endpoint, it is possible to configure multiple albums by selecting the option "Switching in different KHEOPS albums".

Explanations for configuring Kheops-related parameters can be found in the [Kheops](../kheops#create-a-switching-kheops-album) chapter.

