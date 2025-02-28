---
title: Actions on dates
weight: 30
description: Definition of the actions applied to dates
---

This profile element applies a specific action on a tag or a group of tags containing a date. 
This action can only be applied to the following Value Representations: Age String (AS), Date (DA), Date Time (DT) and Time (TM).

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `action.on.dates`
* `option`: the action to be applied, detailed below
* `arguments`: additional parameters depending on the chosen option

This profile can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance
* `tags`: list of tags the action should be applied to. If not specified, the profile element will be applied to all the tags that have a AS, DA, DT or TM VR in the instance
* `excludedTags`: list of tags that will be ignored by this action

The parameter option can have one of the following values:

* `shift`
* `shift_range`
* `shift_by_tag`
* `date_format`

## Shift option

The **shift** option applies a shift to a date according to the following required arguments:

* `seconds`: integer representing the number of seconds the shift operation should apply
* `days`: integer representing the number of days the shift operation should apply

In the case of a shift action applied to an Age String (AS) Value Representation, the seconds and days will be added to the existing value.

In case of a shift action applied to a Date (DA), Date Time (DT) or Time (TM) Value Representation, the seconds and days will be subtracted to the existing value.

In this example, all the tags starting with 0010 and that have a AS, DA, DT or TM VR will be shifted by 30 seconds and 10 days.

```yaml
- name: "Shift Date"
  codename: "action.on.dates"
  arguments:
    seconds: 30
    days: 10
  option: "shift"
  tags:
    - "0010,XXXX"
```

## Shift Range option

The **shift range** option applies a random shift to a date parametrized by ranges defined by the user as follows:

- `max_seconds` (required): integer representing the upper bound of the number of seconds the shift operation should apply
- `max_days` (required): integer representing the upper bound of the number of days the shift operation should apply
- `min_seconds` (optional): integer representing the lower bound of the number of seconds the shift operation should apply, the default value is 0 is not specified
- `min_days` (optional): integer representing the lower bound of the number of days the shift operation should apply, the default value is 0 is not specified

The random operation is deterministic and reproducible based on the patient and the project. For more details about the usage of randomization in Karnak, please refer to the [Karnak Project](https://github.com/OsiriX-Foundation/karnak/tree/master/doc).

In this example, all the tags starting with 0008,002 and that have a AS, DA, DT or TM VR will be shifted randomly in a range of 0 to 60 seconds and 50 to 100 days.

```yaml
  - name: "Shift Range Date"
    codename: "action.on.dates"
    arguments:
      max_seconds: 60
      min_days: 50
      max_days: 100
    option: "shift_range"
    tags:
      - "0008,002X"
```
## Date Format option

The **date format** option applies a partial deletion to a date depending on the specified option:

* `day`: this option will delete the day information and replace it with the first day of the month
* `month_day`: this option will delete the day and month information and replace it with the first month and first day of the month

This action can only be applied to the following Value Representations: Date (DA) and Date Time (DT).

### *day* example

In this example, all the tags starting with 0008,003 and that have a DA or DT VR will have the day information removed.

```yaml
  - name: "Date format"
    codename: "action.on.dates"
    arguments:
      remove: "day"
    option: "date_format"
    tags:
      - "0008,003X"
```

For example, if the value contained in the tag is `20230512`, the output value will be `20230501`.

### *month_day* example

In this example, all the tags starting with 0008,003 and that have a DA or DT VR will have the day and month information removed.

```yaml
  - name: "Date format"
    codename: "action.on.dates"
    arguments:
      remove: "month_day"
    option: "date_format"
    tags:
      - "0008,003X"
```

For example, if the value contained in the tag is `20230512`, the output value will be `20230101`.

## Shift By Tag option

The **shift by tag** option applies a shift to a date according to a value contained in a specified DICOM tag. The arguments are detailed below:

* `seconds_tag`: tag that contains the number of seconds the shift operation should apply
* `days_tag`: tag that contains the number of days the shift operation should apply

Both arguments are not required, but at least one of them must be specified in order to apply the action.

In this example, all the tags starting with 0010 and that have a AS, DA, DT or TM VR will be shifted by the number of days stored in the private tag `(0015,0011)`.

```yaml
- name: "Shift Date By Tag"
  codename: "action.on.dates"
  arguments:
    days_tag: "(0015,0011)"
  option: "shift_by_tag"
  tags:
    - "0010,XXXX"
```

---

Example of a complete and valid profile yaml file that applies the following profile elements in order:
* Shift the tags that match (0008,003X) and (0008,0012) except for (0008,0030) and (0008,0032) and that have a AS, DA, DT or TM VR will be shifted randomly in a range of 0 to 60 seconds and 10 to 50 days.
* Remove the day and month information contained in the tags (0008,0023) and (0008,0021) and that have a DA or DT VR
* Shift the tags that match (0010,XXXX) except for (0010,0010) and that have a AS, DA, DT or TM VR will be shifted by 30 seconds and 10 days.
* Apply the Basic DICOM Profile to all the tags that did not undergo a modification previously

```yaml
name: "De-identification profile"
version: "1.0"
minimumKarnakVersion: "0.9.2"
defaultIssuerOfPatientID:
profileElements:
  - name: "Shift Range Date with arguments"
    codename: "action.on.dates"
    arguments:
      max_seconds: 60
      min_days: 10
      max_days: 50
    option: "shift_range"
    tags:
      - "0008,0012"
      - "0008,003X"
    excludedTags:
      - "0008,0030"
      - "0008,0032"
      
  - name: "Date Format"
    codename: "action.on.dates"
    arguments:
      remove: "month_day"
    option: "format_date"
    tags:
      - "0008,0023"
      - "0008,0021"

  - name: "Shift Date with arguments"
    codename: "action.on.dates"
    arguments:
      seconds: 30
      days: 10
    option: "shift"
    tags:
      - "0010,XXXX"
    excludedTags:
      - "0010,1010"

  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"
```
---


