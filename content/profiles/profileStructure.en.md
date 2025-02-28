---
title: Profile Structure
weight: 10
description: Presentation of the Profile Structure
---

In the Karnak user interface, the Profiles page can be accessed using the menu bar on the left. It displays the list of existing profiles and offers to import new profiles.

A profile file is one or a list of profile elements that are defined for a group of DICOM attributes and with a particular action. During de-identification or tag morphing, Karnak will apply the profile elements to the applicable DICOM attributes. Only one profile element can be applied to a DICOM attribute. **The profile elements are applied in the order defined in the yaml file** and, therefore, the first applicable profile element will modify the value of a DICOM attribute. If other profile elements were applicable to that specific tag, they won't be applied since it has already been modified.

Currently, the profile must be a yaml file (MIME-TYPE: **application/x-yaml**) and respect the definition as below.

## Profile metadata

All these metadata are optional, but for a better user experience we recommend defining at least the name and the version. They will be used to identify and select your profile in a Project.

* `name` - The name of your profile

* `version` - The version of your profile

* `minimumKarnakVersion` - The version of Karnak when the profile has been imported

* `defaultIssuerOfPatientID` - Default value in case the IssuerOfPatientID value is not available in DICOM file, it is used to build the patient's pseudonym when applying de-identification

* `profileElements` - The list of profile elements, the elements are applied accordingly to their position in the list

## Profile element

A profile element is defined as below in the yaml file.

* `name` - The name of your profile element

* `codename` - The codename represents the type of profile element. The available types of profiles elements and their codename are described in details in this documentation

* `condition` - A boolean condition that defines some requirements to apply this profile element

* `action` - The type of action that will be applied

* `option` - Required for certain types of profile elements, contains a single value

* `arguments` - Required for certain types of profile elements, contains a list of key-value pairs

* `tags` - List of tags or pattern that identifies the DICOM attributes this profile should be applied to

* `excludedTags` - List of tags or pattern that identifies the DICOM attributes this profile should not be applied to. These attributes can then be modified by another profile element if applicable

### Tag

DICOM Tags can be defined in different formats: `(0010,0010)`; `0010,0010`; `00100010`;

A tag pattern represent a group of tags and can be defined as follows: e.g. `(0010,XXXX)` represent all the tags of group 0010.
The pattern `(XXXX,XXXX)` targets all the DICOM attributes.

### Condition

A condition can be added to any type of profile element. It contains an expression that will be evaluated for each tag the profile element is applied to.

The syntax and usage of these conditions is detailed in the [Conditions](profiles/conditions) page.

## Validation

The content of the yaml file is validated upon import. If the structure or parameters are not defined correctly, detailed errors will be displayed to the user. 

Please refer to the [Profiles](/content/userguide/profiles#3-profile-details) page for more information.

## Basic Dicom Profile

The [Basic DICOM Profile](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html) is defined by DICOM to remove all the attributes that could contain Individually Identifying Information (III) about the patient or other individuals or organizations associated with the data.
The details of this profile element can be found in the DICOM Standard. 

Further details on this profile element and its implementation in Karnak can be found in the [How Karnak does ?](profiles/rules) page. 

**We strongly recommend including this profile as basis for de-identification.**

This profile element can be included in the profile definition by referencing its codename:

```yaml
- name: "DICOM basic profile"
  codename: "basic.dicom.profile"
```

---

Example of a complete and valid profile yaml file that applies only the Basic DICOM Profile:

```yaml
name: "Dicom Basic Profile"
version: "1.0"
minimumKarnakVersion: "0.9.2"
defaultIssuerOfPatientID:
profileElements:
  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"
```
---
