---
layout: default
title: Profiles
parent: De-identification
nav_order: 1
permalink: /docs/deidentification/profiles
---

# Profile

In the Karnak user interface, you can click on the Profile button on the left. This allows you to see the list of profiles and to import new profiles.

A profile file is one or a list of profile elements that are defined for a group of DICOM attributes and with a particular action. During de-identification, Karnak will apply the profile elements to all applicable DICOM attributes. The principle is that it is not possible to apply multiple profile elements to a DICOM attribute. **The profile elements are applied in the order defined in the yaml file** and, therefore, it is the first applicable profile element that will modify the value of a DICOM attribute and the following profile elements will not be applied.

Currently, the profile must be a yaml file (MIME-TYPE: **application/x-yaml**) and respect the definition as below.

## Profile metadata

All these metadata are optional, but for a better user experience we recommend defining at least the name and the version.

`name` - The name of your profile.

`version` - The version of your profile.

`minimumKarnakVersion` - The version of Karnak when the profile has been built.

`defaultIssuerOfPatientID` - this value will be used to build the patient's pseudonym when IssuerOfPatientID value is not available in DICOM file.

`profileElements` - The list of profile element applied on the de-identification. The first profile element of this list is first called.

## Profile element

A profile element is defined as below in the yaml file.

`name` - The name of your profile element

`codename` - The ID of the profile element. It must related to the list profile elements defined below.

`condition` - Apply a profile element if the condition is met

`action` - The action to apply to the profile element. For example K (keep), X (remove)

`option` - Some profiles contain option.

`arguments` - Some profiles contain one or more arguments. Arguments is a list that contain a key and a value.

`tags` - List of tags for the current profile. The action defined in the profile will be applied on this list of tags.

`excludedTags` - This list represents the tags where the action will not be applied. This means that if a tag defined in this list appears for this profile, it will give control to the next profile.

### Tag

DICOM Tags can be defined in different formats: `(0010,0010)`; `0010,0010`; `00100010`;

A tag pattern represent a group of tags and can be defined as follows: e.g. `(0010,XXXX)` represent all the tags of group 0010.

### codename

---

`basic.dicom.profile` is the [basic profile defined by DICOM](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html). This profile applied the action defined by DICOM on the tags that identifies.

**We strongly recommend including this profile as basis for de-identification.**

This profile element requires the following parameters:

```yaml
- name: "DICOM basic profile"
  codename: "basic.dicom.profile"
```

---

`action.on.specific.tags` is a profile that apply a action on a group of tags defined by the user. The action possible is:
* K - keep
* X - remove

This profile element requires the following parameters:

* name
* codename
* action
* tags

This profile element can have these optional parameters:

* excludedTags

In this example, all tags starting with 0028 will be removed excepted (0028,1199) which will be kept.

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

`action.on.privatetags` is a profile that apply a action given on all private tags or a group of private tags defined by the user. If the tag given isn't private, the profile will not be called. The action possible is:

* K - keep
* X - remove

This profile element requires the following parameters:

* name
* codename
* action

This profile can have these optional parameters:

* tags
* excludedTags

In this example, all tags starting with 0009 will be kept and all private tags will be deleted.

```yaml
- name: "Keep private tags starint with 0009"
  codename: "action.on.privatetags"
  action: "K"
  tags:
    - "(0009,xxxx)"

- name: "Remove all private tags"
  codename: "action.on.privatetags"
  action: "X"
```

---

`action.on.dates` is a profile element that applies actions on dates. This profile element contains several options and  will be executed only on the following VR types: AS, DA, DT ,TM.

This profile need this parameters:

* name
* codename
* option

This profile element need one of this options:

* shift
* shift_range
* date_format

This profile element can have these optional parameters:

* tags
* excludedTags

Below, the examples with the different possible options

1 . **shift** option allows to shift a date according to the following arguments:

* seconds (required)
* days (required)

In this example, all the tags starting with 0010 and that are date fields are offset by 30 seconds and 10 days.

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

2 . **shift_range** option allows to shift a date according to a date range according to the following arguments:

- max_seconds (required)
- max_days (required)
- min_seconds (Optional)
- min_days (Optional)

In this example, all the tags starting with 0008,002 and that are date fields are shifted randomly in a range of maximum second 60 maximum days 100 and minimum days 50. For each same patient belonging to the same project the random value shifted randomly will always be the same (For more details about the project and the Karnak random, see [Karnak Doc](https://github.com/OsiriX-Foundation/karnak/tree/master/doc))


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

3 .**date_format** option allows you to delete the days or the month and days.

With this example profile element, a tag value contains this entry date for example  `20140504 `  with the argument key  `remove ` and value  `month_day `, will have this output date  `20140101`.

```yaml
  - name: "Date format"
    codename: "action.on.dates"
    arguments:
      remove: "month_day"
    option: "date_format"
    tags:
      - "0008,003X"
```

With this example profile element, a tag value contains this entry date for example  `20140504 `  with the argument key  `remove ` and value  `month_day `, will have this output date  `20140501`.

```yaml
  - name: "Date format"
    codename: "action.on.dates"
    arguments:
      remove: "day"
    option: "date_format"
    tags:
      - "0008,003X"
```

4 .**shift_by_tag** option to allows you to shift a date according to increment stored in a DICOM tag according to the following arguments:

- seconds_tag (optional)
- days_tag (optional)

In this example, all the tags starting with 0010 and that are date fields are offset by the number of days stored in the private tag `(0015,0011)`.

```yaml
- name: "Shift Date By Tag"
  codename: "action.on.dates"
  arguments:
    seconds_tag: null
    days_tag: "(0015,0011)"
  option: "shift_by_tag"
  tags:
    - "0010,XXXX"
```

---

`expression.on.tags` is a profile which applies an expression to the given tag. An expression is a programming language that allows you to return a value or an action according to a certain condition.

This profile element requires the following parameters:

* name
* codename
* arguments
* tags

This profile can have these optional parameters:

* excludedTags

Expression profile needs the `expr`  `arguments`. This argument will contain the expression to execute. If the value returned by the expression is null, it will pass to the next profile `DICOM basic profile`. If the value returned by the expression is an action, it will execute the action. See the profile example below.

```yaml
name: "Example"
version: "1.0"
minimumKarnakVersion: "1.0"
defaultIssuerOfPatientID: ""
profileElements:
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "tag == #Tag.PatientName? Keep() : null"
    tags: 
      - "(xxxx,xxxx)"

  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"
```

The following variables are used to retrieve the information of the current attribute to be de-identified.

- `tag` contain the current attribute tag.
- `vr` contain the current attribute VR.
- `stringValue` contain the element current value. 

For example, in the case of the deidentification of PatientName, the variables are setted like following values.

 `tag`: Integer value of (0010,0010)

`stringValue` :'Jorge'

`vr` : Person Name

Here are the constant that can be used in expressions.

- `#Tag` is a constant that permit to access to the all tag integer value in the DICOM standard. Example, `#Tag.PatientBirthDate` allow to access to the integer value of tag (0010,0030).
- `#VR` is a constant that permit to access tho the all VR value in the DICOM standard. Example, `#VR.LO` is the Long String type.

Here are the functions that can be used in expressions.

Available functions for get a value:

- `getString(int tag)` Get the value of the given tag in the DICOM instance. In the case of the tag is not present in the DICOM instance, the method return null. 

- `tagIsPresent(int tag)` Checks if tag is present in the DICOM instance.

  

Available actions:

- `ReplaceNull()` Set to empty the tag value. 
- `Replace(String dummyValue)` Replace tag value.
- `Remove()` Remove the tag.
- `Keep()` Keep the tag.

For a better comprehension of an expression profile, there are some example below.

In this example, for each tags value that equal to 'Jorge' and VR that equal to a Person Name are removed. Otherwise, it will pass to the next profile, because it return null value.

```yaml
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "stringValue == 'Jorge' and vr == #VR.PN? Remove() : null"
    tags: 
      - "(xxxx,xxxx)"
```

In this example, all string value of tag contain 'Jorge' are replaced by the InstitutionName value. Otherwise, the tag is keep. **Warning** the next profiles will not be executed, because an action is always return.

```yaml
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "stringValue == 'Jorge' and tag == #Tag.PatientName? Replace(getString(#Tag.InstitutionName)) : Keep()"
    tags: 
      - "(xxxx,xxxx)"
```

In this example, the expression will be apply on the two following tags (0010,0010) and (0010,0212). if (0010,0010) or (0010,0212) tags, contain a string value with 'UNDEFINED' it will execute the Keep() action. Otherwise, the tag is removed.

```yaml
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "stringValue == 'UNDEFINED'? Keep() : Remove()"
    tags: 
      - "(0010,0010)" #PatientName
      - "(0010,0212)" #StrainDescription
```

In this example, the expression will be apply on the following tag (0008,1030) and it will be replace the value of StudyDescriptuon with 'InstitutionName - StationName'.  

```yaml
- name: "Expression"
  codename: "expression.on.tags"
  arguments:
    expr: "Replace(getString(#Tag.InstitutionName) + '-' + getString(#Tag.StationName))"
  tags: 
    - "(0008,1030)" #StudyDescription
```

___

`clean.recognizable.visual.features`is a profile that apply defacing.

This profile is applied only on the following SOP:

* 1.2.840.10008.5.1.4.1.1.2 - CT Image Storage
* 1.2.840.10008.5.1.4.1.1.2 - Enhanced CT Image Storage

This profile is applied only on Axial Image Orientation.

This profile element requires the following parameters:

* name
* codename
* condition (optional)

```yaml
- name: "Clean Recognizable Visual Features Option"
  codename: "clean.recognizable.visual.features"
```

---

`clean.pixel.data` is a profile that apply the masks defined on the instance image. To defined the masks, please refer [Masks](#masks)

This profile is applied only on the following SOP:

* 1.2.840.10008.5.1.4.1.1.6.1 - Ultrasound Image Storage
* 1.2.840.10008.5.1.4.1.1.7.1 - Multiframe Single Bit Secondary Capture Image Storage
* 1.2.840.10008.5.1.4.1.1.7.2 - Multiframe Grayscale Byte Secondary Capture Image Storage
* 1.2.840.10008.5.1.4.1.1.7.3 - Multiframe Grayscale Word Secondary Capture Image Storage
* 1.2.840.10008.5.1.4.1.1.7.4 - Multiframe True Color Secondary Capture Image Storage
* 1.2.840.10008.5.1.4.1.1.3.1 - Ultrasound Multiframe Image Storage
* 1.2.840.10008.5.1.4.1.1.77.1.1 - VL endoscopic Image Storage

**Or** if the tag value Burned In Annotation (0028,0301) is "YES"

This profile element requires the following parameters:

* name
* codename

```yaml
profileElements:
  - name: "Clean pixel data"
    codename: "clean.pixel.data"
```

---

## Masks

The masks requires the following parameters:

* stationName
* color
* rectangles

`stationName` is the station name source.

This is used to provide the possibility to apply a different mask per station. If the station is set to `*`, the mask will be applied to all except the one where the station name matches.

You can find the station name of your DICOM instance in the tag (0008, 1010), this value in this tag will be used for matching.

`color` defines the color of the mask. It's in Hex Color.

`rectangles` defines the list of the rectangle to apply.

A rectangle take 4 parameters as follow (x, y, width, height). The upper left corner of the image corresponds to the coordinates (0,0).

The illustration below shows you how a rectangle of (25, 75, 150, 50) is created.

![Rectangles example](resources/CleanPixel_rectangle.png)

The following example shows how to define a default mask ("*") and a mask that will be applied only to instances coming from the R2D2 station.

```yaml
masks:
  - stationName: "*"
    color: "ffff00"
    rectangles:
      - "25 75 150 50"
  - stationName: "R2D2"
    color: "00ff00"
    rectangles:
      - "25 25 150 50"
      - "350 15 150 50"
```

## A full example of profile

This example keep a Study Description tag if that as contain 'R2D2' value, keep the Philips PET private group and apply the basic DICOM profile.

The tag pattern (0073,xx00) and (7053,xx09) are defined in [Philips PET Private Group by DICOM](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_E.3.10.html).

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

