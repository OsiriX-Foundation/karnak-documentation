---
title: Profiles
weight: 20
description: Manage the profiles
---

This page lists all profiles configured in Karnak and allows you to create, edit, and delete them. A profile defines the actions to apply to a DICOM instance before it is sent

The `Dicom Basic Profile` is available by default and cannot be deleted. It is described in the section [How does de-identification work?](../../profiles/rules). This profile cannot be modified except for the default `defaultIssuerOfPatientID` [value](../../profiles/profilestructure/#profile-metadata).

![profile page](/userguide/profile_main.png)

##### 1. Profile drag and drop area

Profiles can only be created by importing a YAML file using the top\-right component of the `Profiles` view. You can select a file by clicking the `Upload file...` button or by dragging and dropping it into the area. The file is then loaded and analyzed. If no errors are found, a new profile is automatically created from the file and added to the profile list.

##### 2. Profile list

All available profiles are listed here. A profile can evolve over time and appear multiple times in the list under the same name but with different versions.

When you select a profile in the list, its details are displayed on the right.

##### 3. Profile details

This area shows the details of the selected profile.

If a profile is selected in the list, its details appear in the right panel.

Some fields, such as the name, version, and minimum required Karnak version, can be edited by clicking the pen icon.

![profile edit](/userguide/profile_editbtn.png) 

Edit the field value, then click the checkmark button to save the changes or the cross button to discard them.

![profile edit](/userguide/profile_editconfirm.png)

The rest of the profile cannot be modified.

You can download the entire profile as a YAML file by clicking the button on the right, or delete it by clicking the button on the left.

![Profile actions](/userguide/profile_actions.png)

When a YAML file is imported successfully, this view shows the details of the newly created profile.

If errors occur during profile creation, the profile is not added to the list. The errors are shown in the right panel with details about their cause, as illustrated below.

![profile page](/userguide/profile_error.png)
