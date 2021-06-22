---
layout: default
title: Destination Conditions
parent: User guide
nav_order: 7
permalink: /docs/userguide/conditions


---

# Destination Conditions

A condition is an expression to apply to a given tag. An expression is a programming language that allows you to return a value or an object according to a certain condition. In this case, the value returned is always a Boolean (True/False).

You can use some constants defined in an expression.

- `#Tag` is a constant that permit to access to the all tag integer value in the DICOM standard. Example, `#Tag.StudyDescription` allow to access to the integer value of tag (0008,1030).
- `#VR` is a constant that permit to access tho the all VR value in the DICOM standard. Example, `#VR.LO` is the Long String type.

The list of functions available to create your conditions are listed below. **Beware** all these functions are case sensitive.

---

`tagValueIsPresent(int tag, String value)` or `tagValueIsPresent(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and checks if the `value` given is the same as the tag value.

```
# Check if the study description is "755523-st222-GE"
tagValueIsPresent(#Tag.StudyDescription, "755523-st222-GE")
tagValueIsPresent("0008,1030", "755523-st222-GE")

# Check if the study description is not "755523-st222-GE"
!tagValueIsPresent(#Tag.StudyDescription, "755523-st222-GE")
!tagValueIsPresent("0008,1030", "755523-st222-GE")
```

---

`tagValueContains(int tag, String value)` or `tagValueContains(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and checks if the `value` given appear in the tag value.

```
# Check if the study description contains "st222"
tagValueContains(#Tag.StudyDescription, "st222")
tagValueContains("0008,1030", "st222")

# Check if the study description not contains "st222"
!tagValueContains(#Tag.StudyDescription, "st222")
!tagValueContains("0008,1030", "st222")
```

---

`tagValueBeginsWith(int tag, String value)` or `tagValueBeginsWith(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and checks if the `tag` value begins with the `value`

```
# Check if the study description begins with "755523"
tagValueBeginsWith(#Tag.StudyDescription, "755523")
tagValueBeginsWith("0008,1030", "755523")

# Check if the study description not begins with "755523"
!tagValueBeginsWith(#Tag.StudyDescription, "755523")
!tagValueBeginsWith("0008,1030", "755523")
```

---

`tagValueEndsWith(int tag, String value)` or `tagValueEndsWith(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and checks if the `tag` value ends with the `value`

```
# Check if the study description end with "GE"
tagValueEndsWith(#Tag.StudyDescription, "GE")
tagValueEndsWith("0008,1030", "GE")

# Check if the study description not end with "GE"
!tagValueEndsWith(#Tag.StudyDescription, "GE")
!tagValueEndsWith("0008,1030", "GE")
```

---

`tagIsPresent(int tag)` or `tagIsPresent(String tag)`

This function will check if the `tag` given is present in the DICOM.

```
# Check if the tag study description is present in the DICOM file
tagIsPresent(#Tag.StudyDescription)
tagIsPresent("0008,1030")

# Check if the tag study description is not present in the DICOM file
!tagIsPresent(#Tag.StudyDescription)
!tagIsPresent("0008,1030")
```

---

You can combine multiple conditions with a logical operator.

`&&` is the AND

`||` is the OR

```
# Check if the tag study description end with "GE" and if the station name is "CT1234"
tagValueEndsWith(#Tag.StudyDescription, "GE") && tagValueContains(#Tag.StationName, "CT1234")

# Check if the tag study description end with "GE" or if the station name is "CT1234"
tagValueEndsWith(#Tag.StudyDescription, "GE") || tagValueContains(#Tag.StationName, "CT1234")
```

