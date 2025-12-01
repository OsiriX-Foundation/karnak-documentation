---
title: Actions on tags
weight: 20
description: Definition of the actions applied to specific tags
---

This page describes profile elements that apply actions to DICOM tags. These actions allow you to selectively keep, remove, or add tags during de-identification.

## Actions on specific tags

This profile element applies an action to a tag or group of tags defined by the user.

### Required Parameters

| Parameter | Description |
|-----------|-------------|
| **name** | Description of the action applied |
| **codename** | `action.on.specific.tags` |
| **action** | Remove (`X`) or Keep (`K`) |
| **tags** | List of tags the action should be applied to |

### Optional Parameters

| Parameter | Description |
|-----------|-------------|
| **condition** | Defines a condition to evaluate if this profile element should be applied to this DICOM instance |
| **excludedTags** | List of tags that will be ignored by this action |

### Example

In this example, all tags starting with 0028 will be removed except (0028,1199) which will be kept.


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

## Actions on private tags

This profile element applies an action to private tags or groups of private tags defined by the user. This action is only applied if the tag is private (where the group number is odd).

### Required Parameters

| Parameter | Description |
|-----------|-------------|
| **name** | Description of the action applied |
| **codename** | `action.on.privatetags` |
| **action** | Remove (`X`) or Keep (`K`) |

### Optional Parameters

| Parameter | Description |
|-----------|-------------|
| **condition** | Defines a condition to evaluate if this profile element should be applied to this DICOM instance |
| **tags** | List of tags the action should be applied to. If not specified, the action is applied to all private tags |
| **excludedTags** | List of tags that will be ignored by this action |

### Example

In this example, all tags starting with 0009 will be kept and all other private tags will be removed.


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

## Add new tags

This profile element adds a tag if it is not already present in the instance. This action is ignored if the tag already exists.

### Required Parameters

| Parameter | Description                                                                                                        |
|-----------|--------------------------------------------------------------------------------------------------------------------|
| **name** | Description of the action applied                                                                                  |
| **codename** | `action.add.tag`                                                                                                   |
| **arguments** | Contains:<br>• `value`: Value to set for the tag |
| **tags** | Must contain exactly one tag - the tag to add                                                                      |

### Optional Parameters

| Parameter | Description |
|-----------|-------------|
| **condition** | Defines a condition to evaluate if this profile element should be applied to this DICOM instance |

### Behavior

**When the tag is not present:**
- The tag will be added by this action
- Further profile element actions that match this tag won't be applied

**When the tag already exists:**
- The add action is ignored
- A subsequent profile element action can be applied to that tag

> [!WARNING]
> Only tags at the root level of the DICOM instance can be added. Adding elements inside a sequence is not supported.

> [!INFO]
> The tag must exist in the DICOM Standard, otherwise the profile validation will fail. The VR is retrieved from the Standard.
>
> If this profile element is applied to a SOP class that doesn't contain this tag, it won't be added to prevent corrupting the DICOM instance. A warning will be generated in the logs.

### Use Case

This feature is especially useful when applying masks to non-compliant SOPs by using the Burned In Annotation attribute. See the [Cleaning Data Pixel Exceptions](../masks/#complete-example-equipment-specific-cleaning) page for a complete example.

### Example

In this example, we add the optional tag Recognizable Visual Features (0028,0302).

```yaml
- name: "Add Recognizable Visual Features tag"
  codename: "action.add.tag"
  arguments:
    value: "YES"
  tags:
    - "(0028,0302)"
```

## Add new private tags

This profile element adds a private tag if it is not already present in the instance. This action is ignored if the tag already exists or if there is a collision with a different Private Creator ID.

### Required Parameters

| Parameter | Description |
|-----------|-------------|
| **name** | Description of the action applied |
| **codename** | `action.add.private.tag` |
| **arguments** | Contains:<br>• `value`: Value to set for the tag<br>• `vr`: Value Representation of the tag<br>• `privateCreator`: (Optional) Value of the PrivateCreatorID |
| **tags** | Must contain exactly one tag - the tag to add |

### Optional Parameters

| Parameter | Description |
|-----------|-------------|
| **condition** | Defines a condition to evaluate if this profile element should be applied to this DICOM instance |

### PrivateCreator Management

The `privateCreator` argument is optional but recommended for consistency.

**When the PrivateCreatorID tag does not exist:**
- The `privateCreator` argument must be specified
- The PrivateCreatorID tag will be created automatically

**When the PrivateCreatorID tag exists:**
- If `privateCreator` is not specified: The tag will be added as part of the existing PrivateCreatorID
- If `privateCreator` is specified: The existing value is matched against the argument
  - If they match: The new tag is added
  - If they don't match: A collision occurs and the tag is not added (a warning is logged)

> [!INFO]
> The behavior for root-level tags and sequences follows the same rules as [Add new tags](#add-new-tags).
>
> For details about private tag management and element numbers, see the [DICOM Standard Part 5 Section 7.8](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_7.8.html).

### Example

In this example, we add a private tag containing the value "sample-project" linked to the PrivateCreatorID "KARNAK-PRIVATE".

```yaml
- name: "Add Private Tag"
  codename: "action.add.private.tag"
  arguments:
    value: "sample-project"
    vr: "LO"
    privateCreator: "KARNAK-PRIVATE"
  tags:
    - "(0057,1000)"
```

**What happens:**
- If the PrivateCreatorID tag (0057,0010) doesn't exist, it's created with the value "KARNAK-PRIVATE" and the tag (0057,1000) is added
- If (0057,0010) exists with the value "KARNAK-PRIVATE", the tag (0057,1000) is added
- If (0057,0010) exists with a different value, the tag (0057,1000) is not added to prevent linking it to the wrong PrivateCreatorID


## Complete Profile Examples

### Example 1: Basic De-identification Profile

This profile applies the following operations in order:

1. Remove tags matching (0008,00XX) and (0010,00XX) except (0008,0008) and (0008,0013)
2. Keep the previously excluded tags specifically (so they won't be removed by subsequent profile elements)
3. Remove all private tags
4. Apply the Basic DICOM Profile to all remaining tags

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

### Example 2: Conditional Profile with Private Tags

This profile demonstrates conditional actions and preserving specific private tags:

1. Keep Study Description tag only if it contains 'R2D2'
2. Keep the Philips PET private group
3. Apply the Basic DICOM Profile

> [!INFO]
> The tag patterns (7053,xx00) and (7053,xx09) are defined in [Philips PET Private Group by DICOM](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_E.3.10.html).


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


