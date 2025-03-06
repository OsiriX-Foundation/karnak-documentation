---
title: Profiles
weight: 20
description: Manage the profiles
---

This page lists all the profiles configured in Karnak and allows to edit, create and delete them. A profile contains a definition of actions that should be applied to a DICOM instance before being sent.

The "Dicom Basic Profile" is present by default and cannot be deleted. This profile is detailed in the section [How does de-identification work?](../../profiles/rules). This profile cannot be deleted or edited, except for the value of default issuer of patient ID. 

![profile page](/userguide/profile_main.png)

##### 1. Profile drag and drop area

A profile can only be created by importing a YAML file using the top right component of the Profiles view. The file can be selected by clicking the "Upload File" button or drag and dropped there. It will be loaded and analyzed. If there are no errors, a new profile will automatically be created with the content of the file, and added to the list of profiles.

##### 2. Profile list

All the profiles available are listed here. A profile can evolve and be present in the list under the same name but with a different version.

By selecting a profile in the list, its details are displayed on the right.

##### 3. Profile details

This area contains the details of a profile.

If the profile is selected in the list, its details will appear on the right panel. 

Some information can be edited such as the name, version and minimum required version of Karnak, by clicking on the pen icon. 

![profile edit](/userguide/profile_editbtn.png) 

The field's value can be edited, the changes are saved by clicking on the check mark button and reverted by clicking on the cross button.

![profile edit](/userguide/profile_editconfirm.png)

The rest of the profile cannot be modified.

The entire profile can be downloaded as a YAML file by clicking on the button on the right or deleted by clicking on the button on the left.

![Profile actions](/userguide/profile_actions.png)

If a YAML file was imported, this view will display the details of the newly created profile, if no errors occurred during the import and the creation was successful. 

If some errors occurred during the profile creation, the profile won't be created and added to the profiles' list. The errors will be displayed in the right panel with details about the source of the error(s). An example is shown below.

![profile page](/userguide/profile_error.png)
