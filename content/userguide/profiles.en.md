---
title: Profiles
weight: 20
description: Manage the profiles
---

The profile page allows you to manage your profiles. On this page.

Karnak has the "Dicom Basic Profile" profile by default, the details about this profile is explained in the section [How Karnak does ?](../profiles/rules.en). This profile cannot be deleted or edited, except for the value of default issuer of patient ID. 

![profile page](/userguide/profile_main.png)

### 1. Profile drag and drop area

As presented below, you can drag and drop your yaml profile configuration.

<img src="/userguide/profile_upload.gif" alt="upload profile" style="zoom:75%;" />

### 2. Profile list

All the profiles available in your Karnak instance are listed here. To view [Profile details](#profile-details), you can click on a profile.

### 3. Profile details

This panel will change depending on your action.

Once you have clicked on a profile, this panel will display the details about the chosen profile.

If you have drag and drop your profile, the details about the profile uploaded will be presented. In case your profile contains any errors, it will show where and what the error are, as the illustration below.

<img src="/userguide/profile_error.png" alt="error profile" style="zoom:50%;" />

The following table shows you the different action of icons present in the profile details.

| Icons                                                | Action associated to the icon                                                                     |
|------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| ![download profile](/userguide/profile_download.png) | Download the profile                                                                              |
| ![download profile](/userguide/profile_delete.png)   | Remove the profile. However, if a profile is used by a project, you will not be able to delete it |
| ![download profile](/userguide/profile_edit.png)     | Edit the name, version, minimum version Karnak required and default issuer of patient ID profile. |