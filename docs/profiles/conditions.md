---
layout: default
title: Conditions
parent: Profiles
nav_order: 6
permalink: /docs/profiles/conditions


---

# Conditions

A condition is an expression evaluated in a certain context and that returns a boolean value (true or false). 

Some constants are also defined to improve readability and reduce errors.

- `#Tag` is a constant that can be used to retrieve any tag integer value in the DICOM standard. For example, `#Tag.PatientBirthDate` corresponds to (0010,0030).
- `#VR` is a constant that can be used to retrieve any VR value in the DICOM standard. For example, `#VR.LO` corresponds to the Long String type.

Utility functions are available to define the conditions and are detailed below.

---

`tagValueIsPresent(int tag, String value)` or `tagValueIsPresent(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and check if the `value` parameter is the same as the tag value.

```
# Check if the study description is equals to "755523-st222-GE"
tagValueIsPresent(#Tag.StudyDescription, "755523-st222-GE")
tagValueIsPresent("0008,1030", "755523-st222-GE")

# Check if the study description is not equals to "755523-st222-GE"
!tagValueIsPresent(#Tag.StudyDescription, "755523-st222-GE")
!tagValueIsPresent("0008,1030", "755523-st222-GE")
```

---

`tagValueContains(int tag, String value)` or `tagValueContains(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and check if the `value` parameter appears in the tag value.

```
# Check if the study description contains "st222"
tagValueContains(#Tag.StudyDescription, "st222")
tagValueContains("0008,1030", "st222")

# Check if the study description does not contain "st222"
!tagValueContains(#Tag.StudyDescription, "st222")
!tagValueContains("0008,1030", "st222")
```

---

`tagValueBeginsWith(int tag, String value)` or `tagValueBeginsWith(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and check if the tag value begins with the parameter `value`

```
# Check if the study description begins with "755523"
tagValueBeginsWith(#Tag.StudyDescription, "755523")
tagValueBeginsWith("0008,1030", "755523")

# Check if the study description does not begin with "755523"
!tagValueBeginsWith(#Tag.StudyDescription, "755523")
!tagValueBeginsWith("0008,1030", "755523")
```

---

`tagValueEndsWith(int tag, String value)` or `tagValueEndsWith(String tag, String value)`

This function will retrieve the `tag` value of the DICOM and check if the tag value ends with the parameter `value`

```
# Check if the study description ends with "GE"
tagValueEndsWith(#Tag.StudyDescription, "GE")
tagValueEndsWith("0008,1030", "GE")

# Check if the study description does not end with "GE"
!tagValueEndsWith(#Tag.StudyDescription, "GE")
!tagValueEndsWith("0008,1030", "GE")
```

---

`tagIsPresent(int tag)` or `tagIsPresent(String tag)`

This function will check if the `tag` is present in the DICOM instance.

```
# Check if the tag study description is present in the DICOM file
tagIsPresent(#Tag.StudyDescription)
tagIsPresent("0008,1030")

# Check if the tag study description is not present in the DICOM file
!tagIsPresent(#Tag.StudyDescription)
!tagIsPresent("0008,1030")
```

---

Multiple conditions can be combined using logical operators.

`&&` corresponds to the AND logical operator

`||` corresponds to the OR logical operator

```
# Check if the tag study description ends with "GE" and if the station name is "CT1234"
tagValueEndsWith(#Tag.StudyDescription, "GE") && tagValueContains(#Tag.StationName, "CT1234")

# Check if the tag study description ends with "GE" or if the station name is "CT1234"
tagValueEndsWith(#Tag.StudyDescription, "GE") || tagValueContains(#Tag.StationName, "CT1234")
```

