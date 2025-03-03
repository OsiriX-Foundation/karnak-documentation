---
title: Actions on tags
weight: 20
description: Definition of the actions applied to specific tags
---

## Actions on specific tags

This profile element applies an action on a tag or a group of tags defined by the user. Its codename is `action.on.specific.tags`.

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `action.on.specific.tags`
* `action`: Remove (`X`) / Keep (`K`)
* `tags`: list of tags the action should be applied to

This profile element can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance
* `excludedTags`: list of tags that will be ignored by this action

In this example, all the tags starting with 0028 will be removed excepted (0028,1199) which will be kept.

```yaml
- name: "Remove tags"
  codename: "action.on.specific.tags"
  action: "X"
  tags:
    - "(0028,xxxx)"
  excludedTags:
    - "(0028,1199)"

- name: "Keep tags 0028,1199"
  codename: "action.on.specific.tags"
  action: "K"
  tags:
    - "0028,1199"
```

---

## Actions on private tags

This profile element applies an action on a private tag or a group of private tags defined by the user. This action won't be applied in case the tag is not private. 

Its codename is `action.on.privatetags`.

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `action.on.privatetags`
* `action`: Remove (`X`) / Keep (`K`)

This profile can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance
* `tags`: list of tags the action should be applied to. If not specified, the action is applied to all private tags
* `excludedTags`: list of tags that will be ignored by this action

In this example, all tags starting with 0009 will be kept and all the other private tags will be removed.

```yaml
- name: "Keep private tags starting with 0009"
  codename: "action.on.privatetags"
  action: "K"
  tags:
    - "(0009,xxxx)"

- name: "Remove all private tags"
  codename: "action.on.privatetags"
  action: "X"
```

---

## Add new tags

This profile element adds a tag if it is not already present in the instance. This action will be ignored if the tag is already present in the instance.

Its codename is `action.add.tag`.

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `action.add.tag`
* `arguments`:
  * `value`: required value to set the tag's value to
  * `vr`: VR of the tag, if not specified, its VR will be retrieved from DICOM Standard
* `tags`: must contain exactly one tag, the one to add

This profile can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance

Regarding the application of profile elements, if the tag is not initially present in the instance, then it will be added by this action. Further actions that match this tag won't be applied.
If the tag was already present in the instance, then the add action is ignored and a further action can be applied to that tag.

This feature is especially useful when applying masks to non-compliant SOPs by using the attribute Burned In Annotation. Please refer to the [Cleaning Data Pixel Exceptions](../masks#pixel-data-cleaning-exceptions) page for an exhaustive example.

In this example, we add the optional tag Recognizable Visual Features (0028,0302).

```yaml
- name: "Add Recognizable Visual Features tag"
  codename: "action.add.tag"
  arguments:
    value: "YES"
    vr: "CS"
  tags:
    - "(0028,0302)"
```

---

Example of a complete and valid profile yaml file that applies the following profile elements in order:
* Remove the tags that match (0008,00XX) and (0010,00XX) except for (0008,0008) and (0008,0013)
* Keep specifically the previously excluded tags (so that they won't be removed by a profile element applied afterward)
* Remove all the private tags
* Apply the Basic DICOM Profile to all the tags that did not undergo a modification previously

```yaml
name: "De-identification profile"
version: "1.0"
minimumKarnakVersion: "0.9.2"
defaultIssuerOfPatientID:
profileElements:
  - name: "Remove tags"
    codename: "action.on.specific.tags"
    action: "X"
    tags:
      - "(0008,00XX)"
      - "0010,00XX"
    excludedTags:
      - "0008,0008"
      - "0008,0013"

  - name: "Keep tags"
    codename: "action.on.specific.tags"
    action: "K"
    tags:
      - "0008,0008"
      - "0008,0013"

  - name: "Remove all private tags"
    codename: "action.on.privatetags"
    action: "X"

  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"

```

This example keeps a Study Description tag if that as contain 'R2D2' value, keep the Philips PET private group and apply the basic DICOM profile.

The tag patterns (0073,xx00) and (7053,xx09) are defined in [Philips PET Private Group by DICOM](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_E.3.10.html).

```yaml
name: "Profile Example"
version: "1.0"
minimumKarnakVersion: "0.9.7"
profileElements:
  - name: "Keep StudyDescription tag according to a condition"
    codename: "action.on.specific.tags"
    condition: "tagValueContains(#Tag.StudyDescription, 'R2D2')"
    action: "K"
    tags:
      - "(0008,1030)"

  - name: "Keep Philips PET Private Group"
    codename: "action.on.privatetags"
    action: "K"
    tags:
      - "(7053,xx00)"
      - "(7053,xx09)"

  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"
```

---


