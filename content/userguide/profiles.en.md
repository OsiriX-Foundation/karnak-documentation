---
title: Profiles
weight: 20
description: Manage the profiles
---

This page lists all profiles configured in Karnak and allows you to create, edit, and delete them. A profile defines the actions to apply to a DICOM instance before it is sent.

The `Dicom Basic Profile` is available by default and cannot be deleted. It is the reference de-identification profile based on the [DICOM standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html). For more details, see [How does de-identification work?](../../profiles/rules)

![profile page](/userguide/profile_main.png)

##### 1. Profile drag and drop area

Profiles can only be created by importing a YAML file using the top-right component of the `Profiles` view. You can select a file by clicking the `Upload file...` button or by dragging and dropping it into the area. The file is then loaded and analyzed. If no errors are found, a new profile is automatically created from the file and added to the profile list.

> [!INFO]
> For details on the profile structure and how to create a valid YAML file, see the [Profile Structure](../../profiles/profilestructure) documentation.

##### 2. Profile list

All available profiles are listed here. A profile can evolve over time and appear multiple times in the list under the same name but with different versions.

When you select a profile in the list, its details are displayed on the right.

##### 3. Profile details

This area shows the details of the selected profile.

Some fields, such as the name, version, and minimum required Karnak version, can be edited by clicking the pen icon.

![profile edit](/userguide/profile_editbtn.png) 

Edit the field value, then click the checkmark button to save the changes or the cross button to discard them.

![profile edit](/userguide/profile_editconfirm.png)

> [!INFO]
> The rest of the profile cannot be modified. To make changes to the profile rules or structure, you must delete the profile and upload a new version.

##### 4. Profile actions

You can download the entire profile as a YAML file by clicking the button on the right or delete it by clicking the trash icon.

![Profile actions](/userguide/profile_actions.png)

**Download**: Export the profile configuration as a YAML file for backup or sharing with other Karnak instances.

**Delete**: Remove the profile from Karnak. If the profile is in use by one or more projects, a warning message appears to prevent accidental deletion.

##### 5. Profile upload feedback

When a YAML file is imported successfully, this view shows the details of the newly created profile.

If errors occur during profile creation, the profile is not added to the list. The errors are shown in the right panel with details about their cause, as illustrated below.

![profile page](/userguide/profile_error.png)
